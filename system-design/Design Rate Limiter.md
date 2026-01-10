# What is a Rate Limiter?

> A **Rate Limiter** controls how many requests a user/client can make to a system in a given time window.

Example:

- 100 requests per minute per user
- 10 requests per second per IP

---

# 🔐 Sliding Window Rate Limiter – LLD

---
## 1️⃣ Why Sliding Window?

### Problem with Fixed Window

```
Limit: 5 requests / minute

12:00:58 → 5 requests ✔
12:01:01 → 5 requests ✔   ❌ burst of 10 requests in 3 seconds
```

👉 Fixed window allows **burst traffic at window boundaries**

---

### ✅ Sliding Window Solution

- Considers **last N seconds**
    
- Smooth and accurate
    
- Common in **API Gateways**

---

## 2️⃣ Sliding Window Concept (Simple)

> “Allow at most **X requests in the last Y seconds**”

Example:

```
Limit = 5 requests
Window = last 60 seconds
```

For every request:

1. Remove timestamps older than `(currentTime - window)`
    
2. If remaining requests < limit → allow
    
3. Else → reject

---

## 3️⃣ Key Design Decisions (Interview Must)

### 🔹 Rate limiting key

- userId
- ipAddress
- apiKey

```text
rate_limit:user:123
```

---

## 4️⃣ Data Structure Choice ⭐

### Best DS: **Deque / Queue of timestamps**

Why?

- Fast insert (O(1))
- Fast removal from front (O(1))
- Maintains order
```java
Deque<Long>
```

---

## 5️⃣ Core Interface (LLD)

```java
public interface RateLimiter {
    boolean allowRequest(String key);
}
```

---

## 6️⃣ Sliding Window Rate Limiter – LLD Code

### Token storage per user

```java
class SlidingWindowRateLimiter implements RateLimiter {

    private final int maxRequests;
    private final long windowSizeMillis;

    private final Map<String, Deque<Long>> requestLogs = new ConcurrentHashMap<>();

    public SlidingWindowRateLimiter(int maxRequests, long windowSizeMillis) {
        this.maxRequests = maxRequests;
        this.windowSizeMillis = windowSizeMillis;
    }

    @Override
    public synchronized boolean allowRequest(String key) {
        long now = System.currentTimeMillis();

        requestLogs.putIfAbsent(key, new ArrayDeque<>());
        Deque<Long> timestamps = requestLogs.get(key);

        // Remove expired requests
        while (!timestamps.isEmpty() &&
                timestamps.peekFirst() <= now - windowSizeMillis) {
            timestamps.pollFirst();
        }

        // Check limit
        if (timestamps.size() < maxRequests) {
            timestamps.addLast(now);
            return true;
        }

        return false;
    }
}
```

---

## 7️⃣ How It Works (Step-by-Step)

Example:

```
Limit: 3 requests / 10 seconds
```

Requests at:

```
t=1s → allowed
t=3s → allowed
t=6s → allowed
t=8s → ❌ rejected
t=12s → allowed (old timestamps removed)
```

---

## 8️⃣ API Integration Example (Spring Boot)

```java
@Component
public class RateLimitFilter extends OncePerRequestFilter {

    private final RateLimiter rateLimiter =
            new SlidingWindowRateLimiter(100, 60_000); // 100 req/min

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws IOException, ServletException {

        String userId = request.getHeader("userId");

        if (!rateLimiter.allowRequest(userId)) {
            response.setStatus(429);
            response.getWriter().write("Too Many Requests");
            return;
        }

        filterChain.doFilter(request, response);
    }
}
```

---

## 9️⃣ Time & Space Complexity

|Operation|Complexity|
|---|---|
|Request check|O(1) amortized|
|Space per user|O(maxRequests)|

---

## 🔟 Distributed Sliding Window (Production)

### ❌ Problem

- Local memory won’t work across instances
    

### ✅ Solution

Use **Redis Sorted Set**

```
ZADD rate_limit:user:123 timestamp timestamp
ZREMRANGEBYSCORE key 0 (now - window)
ZCARD key
```

✔ Atomic  
✔ Distributed  
✔ Scalable

---

## 1️⃣1️⃣ Sliding Window vs Token Bucket (Interview Table)

|Feature|Sliding Window|Token Bucket|
|---|---|---|
|Accuracy|High|Medium|
|Burst handling|Limited|Excellent|
|Complexity|Medium|Low|
|API Gateways|✅|✅|

---

## 1️⃣2️⃣ Interview Final Answer (Say This)

> “I designed a sliding window rate limiter where each user maintains a queue of request timestamps. For each request, old timestamps outside the window are removed, and if the remaining count is below the limit, the request is allowed. This ensures accurate rate limiting compared to fixed window.”

---

## 🔥 Follow-up Questions Interviewers Ask

1. How will you scale this?
    
2. How Redis implementation works?
    
3. Memory cleanup?
    
4. Different limits per API?
    
5. Compare with Token Bucket
    

---

If you want next:

- **Redis Lua Sliding Window**
    
- **API Gateway design**
    
- **Rate limiter HLD**
    
- **Interview cross-questions (mock round)**
    

Just say **next** 🚀