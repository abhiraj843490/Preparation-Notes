**Redis (REmote DIctionary Server)** is an **in-memory data store** that supports:

- Caching
- Pub/Sub messaging
- Session storage
- Distributed locks
- Leaderboards, queues, etc.

Redis stores data in **key-value pairs** and is extremely fast because everything happens **in memory (RAM)**.

---

## 🧩 2️⃣ Why Use Redis for Caching?

Caching means temporarily storing frequently accessed data to speed up responses.  
Example: If you frequently fetch user details from a DB, you can store it in Redis and return it directly next time.

|Without Cache|With Redis Cache|
|---|---|
|DB hit every time (slow)|First time: DB hit; next time: Redis hit (fast)|
|Slower (I/O intensive)|Much faster (in-memory access)|
|More DB load|Reduced DB load|

---

## ⚙️ 3️⃣ Setup Redis in Spring Boot

### Step 1️⃣ — Add Dependency

If using **Maven**, add this in `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

---
### Step 2️⃣ — Run Redis Server

You can run Redis via:

- 🐳 **Docker**
    
    ```bash
    docker run -d --name redis -p 6379:6379 redis:latest
    ```
    
- or install locally (via Redis website or package manager)
---
### Step 3️⃣ — Configure Redis Connection

Add in `application.yml` or `application.properties`:

```yaml
spring:
  data:
    redis:
      host: localhost
      port: 6379
  cache:
    type: redis
```

---

## ⚙️ 4️⃣ Enable Caching in Spring Boot

Add this annotation in your main class:

```java
@SpringBootApplication
@EnableCaching
public class RedisCacheApp {
    public static void main(String[] args) {
        SpringApplication.run(RedisCacheApp.class, args);
    }
}
```

---

## 🧩 5️⃣ Use Redis Caching in Your Service

Example: Suppose we have a service that fetches user details from DB.

### Entity

```java
public class User {
    private int id;
    private String name;
    private String email;
}
```

### Repository (simulation)

```java
@Repository
public class UserRepository {
    public User findUserById(int id) {
        simulateSlowService();
        return new User(id, "Abhiraj", "abhiraj@example.com");
    }

    private void simulateSlowService() {
        try { Thread.sleep(3000); } catch (InterruptedException e) { e.printStackTrace(); }
    }
}
```

### Service Layer (with cache)

```java
@Service
public class UserService {

    @Autowired
    private UserRepository repository;

    @Cacheable(value = "userCache", key = "#id")
    public User getUser(int id) {
        System.out.println("Fetching from DB...");
        return repository.findUserById(id);
    }
}
```

🟢 Explanation:

- `@Cacheable` — Tells Spring to first check Redis.
    
- If cache **miss**, method executes and result stored in Redis.
- If cache **hit**, method is skipped and value returned directly from Redis.

---

## 🧠 6️⃣ Redis Cache Flow Diagram

```
      +-------------------+
      |     Controller    |
      +-------------------+
                 |
                 v
         @Cacheable Method
                 |
   +-------------+-------------+
   |  Redis Cache hit?         |
   |   Yes → Return cache      |
   |   No  → Call DB & cache   |
   +----------------------------+
```

---

## ⚙️ 7️⃣ Other Useful Cache Annotations

|Annotation|Description|
|---|---|
|`@Cacheable`|Checks cache first. If not found, executes method and stores result.|
|`@CachePut`|Always executes method and updates cache with new result.|
|`@CacheEvict`|Removes data from cache (used after updates or deletes).|
|`@Caching`|Combines multiple cache annotations.|
### Example:

```java
@CacheEvict(value = "userCache", key = "#id")
public void deleteUser(int id) {
    repository.delete(id);
}
```

---

## ⚙️ 8️⃣ Custom Redis Configuration (Optional but important)

You can define your own Redis template or cache manager for serialization and TTL:

```java
@Configuration
public class RedisConfig {

    @Bean
    public RedisCacheConfiguration cacheConfiguration() {
        return RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofMinutes(10)) // cache expiration
                .disableCachingNullValues()
                .serializeValuesWith(
                    RedisSerializationContext.SerializationPair
                        .fromSerializer(new GenericJackson2JsonRedisSerializer())
                );
    }

    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory connectionFactory) {
        return RedisCacheManager.builder(connectionFactory)
                .cacheDefaults(cacheConfiguration())
                .build();
    }
}
```

---

## 🔥 9️⃣ Advanced: Thread Safety & Concurrent Caching

If multiple threads request the same data simultaneously:

- Redis handles this well, as it’s **thread-safe**.
- But if you have a **“cache stampede”** (many threads missing the same key at once), you can:
    
    - Use **synchronized blocks**.
    - Use **Spring’s @Cacheable(sync = true)** (Spring Boot 2.4+).
        
Example:

```java
@Cacheable(value = "userCache", key = "#id", sync = true)
public User getUser(int id) {
    return repository.findUserById(id);
}
```

✅ This ensures only one thread fetches from DB while others wait.

---

## 🧩 🔟 Monitor Redis Cache

You can inspect cache keys and values:

```bash
redis-cli
> KEYS *
> GET userCache::1
```

Or use GUI tools like **RedisInsight** for visualization.

---

## 🧠 11️⃣ Common Interview Questions

| Question                                 | Short Answer                                                                            |
| ---------------------------------------- | --------------------------------------------------------------------------------------- |
| What is Redis?                           | In-memory key-value store used for caching, queues, etc.                                |
| What is @Cacheable?                      | Used to store and retrieve method results from cache.                                   |
| What is @CacheEvict?                     | Used to remove an entry from cache.                                                     |
| How does Redis improve performance?      | Reduces DB hits by serving from in-memory (RAM)cache.                                   |
| How do you set TTL (expiration)?         | Using `RedisCacheConfiguration.entryTtl(Duration.ofMinutes(x))`.                        |
| How to handle concurrent cache writes?   | Use `@Cacheable(sync = true)`                                                           |
| Can we use Redis for session management? | Yes, with `spring-session-data-redis` dependency.                                       |
| What serialization format is used?       | Default JDK serialization, but JSON via `GenericJackson2JsonRedisSerializer` is common. |

---

## 🧩 12️⃣ Bonus: Using Redis for Distributed Caching

If you have multiple instances of your Spring Boot app (microservices),  
Redis acts as a **shared distributed cache** across them — so all instances share the same data, improving consistency and reducing redundant DB calls.

---
## 🧩 Redis Caching Workflow (Inside Spring Boot)

How this works **internally** when you use `@Cacheable`.
### Step-by-step:

1️⃣ You call a method like:

`userService.getUser(101);`

2️⃣ Spring Boot’s **CacheInterceptor** intercepts it.  
3️⃣ The interceptor checks if the cache (Redis) already has a key `userCache::101`.  
4️⃣ It calls Redis via the `RedisTemplate` (through Lettuce/Jedis client).  
5️⃣ Redis server checks its **hash table** in memory:

- If **hit** → returns serialized value immediately.
    
- If **miss** → executes method, serializes object (JSON/JDK), and stores in Redis using:
    
    `SET userCache::101 {serialized value}`

---

## ⚙️ 2️⃣ Default Relationship Between Method Params and Cache Keys

By default, the **cache key = combination of all method parameters**.

### Example:

```java
@Cacheable(value = "userCache")
public User getUserById(int id) { ... }
```

➡️ If you call:

```java
getUserById(10);
```

Spring stores the result with key:

```
"userCache::10"
```

If later you call `getUserById(10)` again → it will fetch from cache.  
If you call `getUserById(20)` → new cache entry.

---
### Example with Multiple Params

```java
@Cacheable(value = "employeeCache")
public Employee getEmployee(String dept, int empId) { ... }
```

Key by default:

```
employeeCache::[dept, empId]
```

So `getEmployee("IT", 1)` and `getEmployee("HR", 1)` are **different cache entries**.

---

## 🧩 3️⃣ Customize the Key Using SpEL (Spring Expression Language)

You can **control** how the key is generated using `key` attribute in `@Cacheable`.

### Example 1:

```java
@Cacheable(value = "userCache", key = "#id")
public User getUser(int id) { ... }
```

🟢 Here key = value of parameter `id`.

---
### Example 2:

If your method has multiple parameters:

```java
@Cacheable(value = "orderCache", key = "#customerId + '-' + #orderId")
public Order getOrder(int customerId, int orderId) { ... }
```

🟢 Key = `"101-5001"` for `getOrder(101, 5001)`.

---

### Example 3:

Use a nested property of an object:

```java
@Cacheable(value = "userCache", key = "#user.email")
public User getUserDetails(User user) { ... }
```

🟢 Key = the `email` field of `user` object.

---

## 🧠 4️⃣ Common Key Patterns

|Expression|Description|
|---|---|
|`#root.methodName`|Use method name as part of key|
|`#root.args`|All arguments as an array|
|`#root.targetClass`|Use target class|
|`#root.method`|Full method reference|
|`#p0` or `#a0`|First parameter|
|`#p1` or `#a1`|Second parameter|

Example:

```java
@Cacheable(value = "userCache", key = "#root.methodName + '_' + #p0")
public User findUser(int id) { ... }
```

🟢 Key = `"findUser_10"`

---

## 🧩 5️⃣ What If You Don’t Specify a Key?

Then Spring uses a **default key generator**:

- For **single parameter** → that parameter’s value.
    
- For **multiple parameters** → a list (array) of all parameters.
    
- For **no parameters** → `SimpleKey.EMPTY`.
    

You can also define your own `KeyGenerator` bean if you want to fully customize key logic.

---

## ⚙️ 6️⃣ Custom Key Generator Example

```java
@Component("customKeyGenerator")
public class CustomKeyGenerator implements KeyGenerator {
    @Override
    public Object generate(Object target, Method method, Object... params) {
        return method.getName() + "_" + Arrays.toString(params);
    }
}
```

Use it like this:

```java
@Cacheable(value = "userCache", keyGenerator = "customKeyGenerator")
public User getUser(int id, String name) { ... }
```

🟢 Key example:  
`"getUser_[10, John]"`

---

## 🧩 7️⃣ Example Flow Summary

|Call|Generated Key|Result|
|---|---|---|
|`getUser(101)`|`userCache::101`|Fetch DB and cache result|
|`getUser(101)` again|`userCache::101`|Return from Redis cache|
|`getUser(102)`|`userCache::102`|New cache entry|

---

## 🧠 8️⃣ Interview-style Answer

> In Spring Boot caching, the cache key is derived from the method parameters by default. When a method annotated with `@Cacheable` is called, Spring checks whether a cached entry exists for the given parameters. If it does, it skips the method execution and returns the cached value. Otherwise, it executes the method, stores the result in cache, and uses the method parameters as the key.
> 
> We can customize this key using Spring Expression Language (SpEL) in the `key` attribute or define a custom `KeyGenerator` bean.

---


## 🧠 1st level caching

1st level cache is a session scoped cache that stores entities loaded during a transaction. Each EntityManager instance has it own 1st level cache, which means:
1. it's private to that specific EntityManager instance
2. It's not shared b/w diff EntityManager instaces
3. It gets destroyed when the Entitymanager is closed

Entitymanager Lifecycle is spring Data Jpa:
1. session opening:
	A new Entitymanager session is opened in following scenarios:
	1. at start of a @Transactional method
	2. when calling repository methods without an existing transaction
	3. when explicitly requesting a new transaction using propagation settings.
2. session closing:
	1. when the transaction commits or rolls back
	2. at the end of the @transactional method
	3. when explicilty calling Entitymanager.close()
3. session Recreation:
	1. for each now transaction
	2. when calling methods with a new transaction
	3. after the previous session has been closed.


**Note:**   whenever you fetch an entity within the same Hibernate session (or Entitymanager in springboot), Hibernate  caches it.

## 🧠 2nd level caching

A 2nd level cache is session factory scoped meaning it's shared by all sessions created with the same session factory when an entity instance is looked up by its id and 2nd level caching is enabled for that entity the following happens:
1. if an instance is already present in the 1st level caches, its returned from there.
2. if an instance isnt found in 1st level caceh and the crossponding instance state is cached in the 2nd level cache, then the data is fetched from there and an instance is assembled and returned.
3. otherwise the necessary data are loaded from the database and an instance is assembled and returned.
How to enable:
	Hazelcast, Redis



LRU Cache:
