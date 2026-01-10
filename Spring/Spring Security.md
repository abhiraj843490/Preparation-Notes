### **1. What is Spring Security?**

**Answer:**  
A framework that provides **authentication**, **authorization**, and **protection** against common attacks (CSRF, CORS, Session Fixation etc).

---
### **2. Difference between Authentication vs Authorization**

|Authentication|Authorization|
|---|---|
|Who are you?|What can you access?|
|Login process|Permission checking|
|Username/password|Roles like ADMIN/USER|

---

### **3. How does Spring Security filter chain work?**

Spring Security uses a **FilterChain**.

Common filters:

- `UsernamePasswordAuthenticationFilter`
    
- `BasicAuthenticationFilter`
    
- `JwtAuthenticationFilter` _(custom)_
    
- `ExceptionTranslationFilter`
    
- `FilterSecurityInterceptor`
    

Every incoming request goes through the filter chain.

---

# 🟩 **2️⃣ Spring Boot Security Configuration**

### **4. What is SecurityFilterChain used for?**

In Spring Security 6+, instead of `WebSecurityConfigurerAdapter`, we use:

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .requestMatchers("/public").permitAll()
            .anyRequest().authenticated()
        )
        .httpBasic();
    return http.build();
}
```

---

### **5. What is the purpose of PasswordEncoder?**

Spring Security does NOT allow plain passwords.

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

---

### **6. How to bypass Spring Security for specific APIs?**

Use `permitAll()`:

```java
.authorizeHttpRequests(auth -> auth
     .requestMatchers("/login", "/register").permitAll()
     .anyRequest().authenticated()
)
```

---

# 🟩 **3️⃣ JWT Based Security (Most Important)**

### **7. What is JWT?**

JSON Web Token — used to implement **stateless authentication**.

Contains:

- Header
    
- Payload (user info)
    
- Signature
    

---

### **8. Why do we use JWT instead of session-based login?**

|JWT|Session|
|---|---|
|Stateless|Stateful|
|Scales easily in microservices|Hard to scale|
|No server memory needed|Stores session on server|

---

### **9. Explain the JWT Flow in Spring Security**

1️⃣ User sends username/password  
2️⃣ Server validates  
3️⃣ Generates JWT token  
4️⃣ Client sends JWT on every request  
5️⃣ Custom filter validates the token  
6️⃣ If valid → request forwarded  
7️⃣ If invalid → return 401 Unauthorized

---

### **10. How do you validate JWT in Spring Boot?**

Using a custom filter:

```java
String username = jwtService.extractUsername(token);

if(username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
    UserDetails userDetails = userDetailsService.loadUserByUsername(username);

    if(jwtService.isTokenValid(token, userDetails)) {
        UsernamePasswordAuthenticationToken auth =
                new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());

        SecurityContextHolder.getContext().setAuthentication(auth);
    }
}
```

---

# 🟩 **4️⃣ UserDetails + Authentication**

### **11. What is UserDetails?**

Represents the logged-in user.

---

### **12. What is UserDetailsService?**

Loads user from DB:

```java
public UserDetails loadUserByUsername(String username) {
    return userRepo.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found"));
}
```

---

### **13. Why do we override loadUserByUsername()?**

Because Spring Security must know:

- username
    
- password
    
- roles/authorities  
    to perform authentication.
    

---

# 🟩 **5️⃣ OAuth2 Questions (If asked)**

### **14. What is OAuth2?**

Authorization framework used by Google, Facebook, GitHub login.

---

### **15. OAuth vs JWT**

|OAuth2|JWT|
|---|---|
|Login by third-party|Our own authentication|
|Authorization protocol|Token format|
|Heavy|Lightweight|

---

# 🟩 **6️⃣ CSRF, CORS & Security Attacks**

### **16. What is CSRF?**

Cross-Site Request Forgery → attacker FORCES user to send request unknowingly.

We disable in stateless APIs:

```java
http.csrf().disable();
```

---

### **17. What is CORS?**

Cross-Origin Resource Sharing — allows frontend URL to call backend.

```java
http.cors().and()...
```

---

### **18. How to enable CORS for specific origins?**

```java
@Bean
public WebMvcConfigurer corsConfigurer() {
    return new WebMvcConfigurer() {
        @Override
        public void addCorsMappings(CorsRegistry registry) {
            registry.addMapping("/**")
                    .allowedOrigins("http://localhost:3000")
                    .allowedMethods("GET","POST","PUT","DELETE");
        }
    };
}
```

---

# 🟩 **7️⃣ Practical Scenario Questions (HR + Technical Favorite)**

### **19. If token expires, how do you handle it?**

Return:

```
401 Unauthorized
{"message": "Token expired"}
```

Frontend must call refresh-token API.

---

### **20. How do you secure microservices?**

- API Gateway
- JWT validation filter
- Role-based access
- Service-to-service token

---

### **21. How do you prevent brute-force attacks?**

- IP rate limiting
- Account lock after failures
- Captcha
---

### **22. How to secure passwords in DB?**

Use BCrypt hashing, NOT encryption.

---

### **23. What is stateless authentication?**

Server does NOT store session.  
Each request must bring token (JWT).

---

### **24. What is the difference between roles and authorities?**

- ROLE_ADMIN
- ROLE_USER

Authorities are **permissions**  
(Role is a group of permissions)

---

# 🟩 **8️⃣ Spring Security 6 Changes (VERY HOT QUESTION)**

Spring Security 6 REMOVED:

❌ `WebSecurityConfigurerAdapter`

New approach uses:

✔ `SecurityFilterChain`  
✔ Lambda-based HttpSecurity  
✔ `authorizeHttpRequests()`  
✔ `requestMatchers()`

---

In Spring Security, **roles** and **authorities** are related but **NOT the same** — and this is a very common interview question.  
Let’s break it down **very clearly**.

---

# ✅ **Roles vs Authorities in Spring Security**

### **1️⃣ Authorities → These are permissions**

They represent **what actions the user can perform**.

Examples:

- `"READ_USER"`
    
- `"WRITE_USER"`
    
- `"DELETE_USER"`
    
- `"VIEW_DASHBOARD"`
    

So, **authority = exact permission**.

---

### **2️⃣ Roles → A group (collection) of authorities**

A role is a **higher-level** concept.

Examples:

- `"ROLE_ADMIN"`
    
- `"ROLE_USER"`
    
- `"ROLE_MANAGER"`
    

**Spring Security automatically adds the prefix `ROLE_`**  
when checking roles (unless disabled).

---

# 🔥 Example to show the difference

### **Authorities**

```java
new SimpleGrantedAuthority("READ_USER")
new SimpleGrantedAuthority("WRITE_USER")
```

### **Roles**

```java
new SimpleGrantedAuthority("ROLE_ADMIN")
```

A role usually bundles multiple authorities.  
Example:

**ROLE_ADMIN** may contain:

- READ_USER
    
- WRITE_USER
    
- DELETE_USER
    
- VIEW_DASHBOARD
    

---

# ⭐ Correct Interview Explanation

**Roles are high-level, coarse-grained permissions**  
→ example: "ADMIN", "USER", "MANAGER"

**Authorities are fine-grained permissions**  
→ example: "READ", "WRITE", "DELETE"

**Role = Set of authorities**

---

# 📌 How to Define Role & Authority in Spring Security (Very Important)

### **1. Defining roles**

```java
.authorizeHttpRequests(auth -> auth
    .requestMatchers("/admin/**").hasRole("ADMIN")   // ROLE_ADMIN
)
```

### **2. Defining authorities**

```java
.authorizeHttpRequests(auth -> auth
    .requestMatchers("/user/read").hasAuthority("READ_USER")
)
```

---

# 🧠 Example: User with both Role + Authorities

```java
UserDetails user = User.builder()
        .username("abhiraj")
        .password(passwordEncoder.encode("12345"))
        .roles("ADMIN")  // Spring will add ROLE_ADMIN
        .authorities("READ_USER", "WRITE_USER")
        .build();
```

---

# 📝 How to answer in interview:

**“In Spring Security, authorities are specific permissions like READ_USER or WRITE_USER.  
Roles are a group of these authorities and Spring automatically prefixes them with ROLE_.  
Roles represent what a user is, authorities represent what a user can do.”**


___
Here are **annotation-level ways** to define **roles** and **authorities** in Spring Security — this is exactly what interviewers expect.
# ✅ **1. Using `@PreAuthorize`**

You can secure methods using **roles** or **authorities**.

### **Using Role**

```java
@PreAuthorize("hasRole('ADMIN')")
public String adminOnly() {
    return "Admin Access";
}
```

> Spring internally checks for **ROLE_ADMIN**

### **Using Authority**

```java
@PreAuthorize("hasAuthority('READ_USER')")
public String readUser() {
    return "Read User Data";
}
```

---

# ✅ **2. Using `@Secured`**

Supports only **roles**, not expressions.

### **Using Role**

```java
@Secured("ROLE_ADMIN")
public void performAdminTask() {}
```

### **Multiple Roles**

```java
@Secured({"ROLE_ADMIN", "ROLE_MANAGER"})
public void performTask() {}
```

---

# ✅ **3. Using `@RolesAllowed`**

Part of Jakarta/Javax security (JSR-250).

### **Assign Roles**

```java
@RolesAllowed({"ROLE_USER", "ROLE_ADMIN"})
public void accessData() {}
```

---

# ❗ Important: Enable Annotation-based Security

You must enable method-level security in config:

### For Spring Boot 3 (Spring Security 6+)

```java
@EnableMethodSecurity
@Configuration
public class SecurityConfig {
}
```

### For older versions (Boot 2.x)

```java
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
@Configuration
public class SecurityConfig {
}
```

---

# 🧠 Summary (Interview Ready)

|Annotation|Supports Role|Supports Authority|Supports SPEL|
|---|---|---|---|
|**@PreAuthorize**|✔️ (without ROLE_)|✔️|✔️|
|**@Secured**|✔️ (must prefix ROLE_)|❌|❌|
|**@RolesAllowed**|✔️ (must prefix ROLE_)|❌|❌|

---

# ⭐ Example With Both Role & Authority

```java
@PreAuthorize("hasRole('ADMIN') and hasAuthority('WRITE_USER')")
public void updateUser() {}
```
