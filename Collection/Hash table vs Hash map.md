In Java, **`Hashtable` vs `HashMap`** are both implementations of the **Java Collections Framework** used to store **key–value pairs**, but they have some important differences.

---

# 1️⃣ Basic Difference

|Feature|Hashtable|HashMap|
|---|---|---|
|Thread Safety|**Synchronized**|**Not synchronized**|
|Performance|Slower|Faster|
|Null Key|❌ Not allowed|✅ Allowed (one null key)|
|Null Values|❌ Not allowed|✅ Allowed|
|Introduced|Java 1.0 (Legacy)|Java 1.2|
|Iterator|Enumeration|Iterator|

---

# 2️⃣ Thread Safety

### Hashtable

All methods are **synchronized**, so it is **thread-safe**.

```java
Hashtable<Integer, String> table = new Hashtable<>();
table.put(1, "A");
```

Internally:

```java
public synchronized V put(K key, V value)
```

Because of synchronization, performance is slower.

---

### HashMap

Not thread-safe.

```java
HashMap<Integer, String> map = new HashMap<>();
map.put(1, "A");
```

If multiple threads modify it, **race conditions may occur**.

To make it thread-safe:

```java
Map<Integer, String> map =
Collections.synchronizedMap(new HashMap<>());
```

---

# 3️⃣ Null Handling

### HashMap

```java
HashMap<Integer,String> map = new HashMap<>();
map.put(null, "Hello");
map.put(1, null);
```

Allowed.

---

### Hashtable

```java
Hashtable<Integer,String> table = new Hashtable<>();
table.put(null,"Hello"); // throws NullPointerException
```

Not allowed.

---

# 4️⃣ Performance

Because **Hashtable synchronizes every method**, it is slower.

Example execution:

```
Hashtable
Thread → Lock → Execute → Unlock

HashMap
Thread → Execute
```

So **HashMap performs better** in single-threaded environments.

---

# 5️⃣ Iteration

### Hashtable (Old style)

```java
Enumeration e = table.keys();
```

### HashMap (Modern)

```java
Iterator<Map.Entry<Integer,String>> it =
map.entrySet().iterator();
```

---

# 6️⃣ Which One Should Be Used Today?

Modern applications prefer:

- **HashMap**
    
- **ConcurrentHashMap** (for thread safety)

Instead of Hashtable.

---
