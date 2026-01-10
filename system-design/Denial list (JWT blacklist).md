JWT (JSON Web Token) is **stateless** — once issued, it cannot be invalidated until it expires.  
So, if a user logs out or their account is revoked, their JWT is still technically **valid** until its expiration time.

➡️ That’s where a **denial list** (or **blacklist**) comes in. 👇

---
### 🔒 **Definition:**

> A _denial list_ (or _JWT blacklist_) is a list of tokens that have been **explicitly revoked** or **invalidated** before their expiry time.

When a user logs out, their JWT is added to this list.  
Every subsequent request checks whether the token exists in the denial list — if it does, access is denied.

---

## ⚙️ 2️⃣ Why JWT Needs a Denial List

Let’s visualize the problem 👇

|Event|Without Denial List|With Denial List|
|---|---|---|
|User logs out|Token still valid until expiry|Token becomes invalid immediately|
|Admin blocks user|Old tokens still usable|Old tokens rejected|
|Token leaked|Attacker can use it until expiry|Can revoke token immediately|

✅ **So we use a denial list to revoke tokens early.**

---

## 🧠 3️⃣ When to Use It

You should use a **JWT denial list** when:

1. You want **user logout** to invalidate the token immediately.
2. You have **short-lived** tokens but still want manual revocation.
3. You allow **admin revocation** of tokens for banned/suspended users.
4. You want **refresh-token rotation** (old refresh tokens are blacklisted once used).

---

## ⚙️ 4️⃣ How It Works — Step by Step

Let’s see the **flow** 👇

### 🧩 Step 1: User Logs In

- Server generates JWT (with expiry and claims)
- JWT is returned to the client
- Token is **not** in denial list → valid
### 🧩 Step 2: User Logs Out

- The token is **added to denial list** (usually stored in Redis or DB)
- Example: key = JWT token, value = expiry timestamp

### 🧩 Step 3: Each Request

- Spring Security intercepts the request
- Extracts the JWT from Authorization header
- Before validating claims, it checks:
    
    ```java
    if (denialListService.isBlacklisted(token)) {
        throw new JwtException("Token revoked");
    }
    ```
    
- If found → reject the request
---

## 💾 5️⃣ Where to Store the Denial List

You can store it in:

|Storage|Pros|Cons|
|---|---|---|
|**In-memory (Map)**|Fast, simple|Lost after restart, not scalable|
|**Redis** ✅|Distributed, fast|Slight extra setup|
|**Database**|Persistent|Slower for large data|

💡 **Best practice:** use **Redis** (TTL = token expiry time)

---

## 🧩 6️⃣ Example Implementation in Spring Boot

### Step 1: Add Redis Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### Step 2: DenialListService

```java
@Service
public class JwtDenialListService {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    private static final String BLACKLIST_PREFIX = "jwt_blacklist:";

    public void addToBlacklist(String token, long expirationMillis) {
        long ttl = expirationMillis - System.currentTimeMillis();
        redisTemplate.opsForValue().set(BLACKLIST_PREFIX + token, true, ttl, TimeUnit.MILLISECONDS);
    }

    public boolean isBlacklisted(String token) {
        Boolean value = (Boolean) redisTemplate.opsForValue().get(BLACKLIST_PREFIX + token);
        return Boolean.TRUE.equals(value);
    }
}
```

---

### Step 3: Add to Denial List on Logout

```java
@RestController
@RequestMapping("/auth")
public class AuthController {

    @Autowired
    private JwtDenialListService denialListService;

    @PostMapping("/logout")
    public ResponseEntity<String> logout(@RequestHeader("Authorization") String tokenHeader) {
        String token = tokenHeader.replace("Bearer ", "");
        long expiryTime = getExpiryFromToken(token); // extract from JWT claims
        denialListService.addToBlacklist(token, expiryTime);
        return ResponseEntity.ok("User logged out successfully");
    }
}
```

---

### Step 4: Check Denial List in Filter

You integrate this check in your JWT authentication filter:

```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Autowired
    private JwtTokenProvider jwtTokenProvider;

    @Autowired
    private JwtDenialListService denialListService;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
            throws IOException, ServletException {

        String token = jwtTokenProvider.resolveToken(request);

        if (token != null && jwtTokenProvider.validateToken(token)) {
            if (denialListService.isBlacklisted(token)) {
                response.sendError(HttpServletResponse.SC_UNAUTHORIZED, "Token revoked");
                return;
            }
            Authentication auth = jwtTokenProvider.getAuthentication(token);
            SecurityContextHolder.getContext().setAuthentication(auth);
        }

        chain.doFilter(request, response);
    }
}
```

---
## ⚙️ 7️⃣ Best Practices

✅ Use **short-lived JWTs** (5–15 min) and **refresh tokens**.  
✅ Store denial list in **Redis with TTL** equal to JWT expiration.  
✅ Automatically clean up expired tokens using Redis TTL.  
✅ Use **refresh token rotation** — revoke old refresh tokens once used.

---
## 🚀 8️⃣ Summary

|Concept|Explanation|
|---|---|
|**Denial List / Blacklist**|List of revoked JWT tokens|
|**Purpose**|Invalidate tokens before expiry|
|**When to Use**|Logout, token leak, user ban|
|**Storage**|Redis (best), DB, in-memory|
|**Check**|In JWT filter before authentication|

---
