
# **High Level Design (HLD)**

## 1. **What is the system supposed to do? (Requirements)**

### ✔ Functional Requirements

- Convert long URL → short URL
- Redirect short URL → original URL
- Handle huge traffic (read-heavy system)
- Custom URL support (/customname)
- URL expiry
- Analytics (optional)

---

# ⭐ **2. High-Level Architecture Diagram**

```
Client
   ↓
Load Balancer
   ↓
API Gateway
   ↓
Application Service (URL Shortening Service)
   ↓
Cache (Redis)
   ↓
Database (SQL/NoSQL)
   ↓
Background Workers (TTL Expiry)
```

---

# ⭐ **3. Key Components**

### **A. URL Generation Service**

Responsible for generating short, unique keys (like `abc123`).

### **B. Redirect Service**

Given short key → returns long URL.

### **C. Storage Layer**

You can use:

| Storage                        | Reason                            |
| ------------------------------ | --------------------------------- |
| **NoSQL (Cassandra/DynamoDB)** | High write + high read throughput |
| **SQL (MySQL/Postgres)**       | Simple + ACID + indexing          |
| **Redis Cache**                | Fast redirect lookup              |

### **D. Hashing / Encoding**

Short URL is generated using:

1. **Base62 encoding** (0–9, a–z, A–Z) → 62^6 = 56B combinations
2. Random string
3. Hash function + collision resolution

---

# ⭐ **4. Database Design**

### **Table: URL_Mapping**

|Column|Type|Description|
|---|---|---|
|id|BIGINT|Auto-increment primary key|
|short_key|VARCHAR(10)|“abc123”|
|long_url|TEXT|Original long URL|
|created_at|TIMESTAMP|Creation time|
|expiry_at|TIMESTAMP|Optional expiry|

Index:

```
INDEX(short_key)
```

---

# ⭐ **5. Flow: Create Short URL**

### Step-by-step:

1. User sends:

```
POST /create
{
  "longURL": "https://youtube.com/watch?v=xyz"
}
```

2. Service checks Redis cache (optional)
3. Generates unique ID from DB (auto-increment).
4. Encode the ID using Base62  
    Example:
    
    ```
    ID = 125
    Base62(125) → "cb"
    ```
    
5. Store:

```
short_key = "cb"
long_url = "youtube.com/watch?v=xyz"
```

6. Return:

```
tinyurl.com/cb
```

---
# ⭐ **6. Flow: Redirect**

User hits:

```
GET tinyurl.com/cb
```

Service flow:

1. Check Redis cache → found? redirect immediately
2. Cache miss → check DB
3. Save to cache
4. HTTP 302 redirect to original URL

---

# ⭐ **7. HLD Summary (What to say in Interview)**

**“My URL shortener uses a Base62 encoder + auto-increment ID, stored in a scalable database like Cassandra, with Redis cache for fast redirect. Load balancer and application servers handle the traffic, and background workers clean expired URLs.”**

---

# ⭐ **Low Level Design (LLD)**

Now the interviewer wants:

- Classes
- Interfaces
- Responsibilities
- Interaction diagram


Let’s do it.

---
# ⭐ **6. Controller Layer (REST API)**

```java
@RestController
public class URLController {

    @Autowired
    private URLShortenerService service;

    @PostMapping("/shorten")
    public String create(@RequestBody Map<String,String> req) {
        return service.shortenURL(req.get("longURL"));
    }

    @GetMapping("/{shortKey}")
    public ResponseEntity<?> redirect(@PathVariable String shortKey) {
        String url = service.getLongURL(shortKey);
        return ResponseEntity.status(302)
                .header("Location", url)
                .build();
    }
}
```

# ⭐ **5. Service Implementation**

```java
public class URLShortenerServiceImpl implements URLShortenerService {

    private URLRepository repo;
    private Cache cache;

    public URLShortenerServiceImpl(URLRepository repo, Cache cache) {
        this.repo = repo;
        this.cache = cache;
    }

    @Override
    public String shortenURL(String longURL) {
        long id = repo.save(longURL);
        String shortKey = Base62Encoder.encode(id);
        return shortKey;
    }

    @Override
    public String getLongURL(String shortKey) {
        // Check cache
        String url = cache.get(shortKey);
        if (url != null) return url;

        // DB fallback
        url = repo.findLongURL(shortKey);
        cache.put(shortKey, url);
        return url;
    }
}
```

# ⭐ **4. Repository Layer**

```java
public interface URLRepository {
    long save(String longURL);
    String findLongURL(String shortKey);
}
```

# ⭐ **3. Data Model**

```java
public class URLMapping {
    private long id;
    private String shortKey;
    private String longURL;
    private Date createdAt;
    private Date expiryAt;
}
```

# ⭐ 2. Base62 Encoder

```java
public class Base62Encoder {

    private static final String CHAR_SET =
            "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";

    public static String encode(long id) {
        StringBuilder sb = new StringBuilder();
        while (id > 0) {
            sb.append(CHAR_SET.charAt((int)(id % 62)));
            id /= 62;
        }
        return sb.reverse().toString();
    }
}
```

# ⭐ **1. Interfaces**

### **I. URLShortenerService**

```java
public interface URLShortenerService {
    String shortenURL(String longURL);
    String getLongURL(String shortKey);
}
```

# ⭐ **7. LLD Sequence (What to say in interview)**

**“When user calls /shorten, controller delegates to service.  
Service calls repository to create ID, encodes it to Base62 key, stores mapping.  
For redirection, controller asks service → service checks cache → DB fallback → returns long URL.”**

---
# ⭐ **Final Combined Summary (Best Interview Answer)**

**HLD:**

- Use Base62 to generate short keys
- Use SQL/NoSQL to store
- Use Redis for caching
- Load Balancer + App Servers
- API Gateway
- Expiry cleanup worker


**LLD:**

- Interface: URLShortenerService   
- Base62Encoder class
- URLRepository class
- URLShortenerServiceImpl class
- URLController class

---

If you want, I can also give:

✅ **ER Diagram**  
✅ **Sequence Diagram**  
✅ **Class Diagram**  
✅ **Complete Java code**  
Just tell me!