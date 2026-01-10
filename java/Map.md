
A **Map** is an interface in the Java Collection Framework (`java.util.Map<K,V>`)  
that stores data as **key–value pairs**.

Each key is **unique**, but values **can be duplicated**.

### Example:

```java
Map<Integer, String> map = new HashMap<>();
map.put(1, "Apple");
map.put(2, "Banana");
map.put(3, "Cherry");

System.out.println(map.get(2));  // Output: Banana
```

---
# 🧠 2️⃣ Key Characteristics of Map

|Property|Description|
|---|---|
|Key|Unique (no duplicates allowed)|
|Value|Can be duplicated|
|Nulls|Depends on implementation (`HashMap` allows one null key, many null values)|
|Order|Depends on type of Map (`HashMap`, `LinkedHashMap`, `TreeMap`)|

---

# ⚙️ 3️⃣ Common Map Implementations

| Class                 | Ordering        | Nulls                        | Thread Safe                    | Internal Structure                               |
| --------------------- | --------------- | ---------------------------- | ------------------------------ | ------------------------------------------------ |
| **HashMap**           | No order        | 1 null key, many null values | ❌ No                           | Hash table + Linked List (and Tree since Java 8) |
| **LinkedHashMap**     | Insertion order | Yes                          | ❌ No                           | Hash table + Doubly Linked List                  |
| **TreeMap**           | Sorted by keys  | ❌ No null key                | ❌ No                           | Red-Black Tree (Balanced BST)                    |
| **Hashtable**         | No order        | ❌ No null key/value          | ✅ Yes                          | Hash table (synchronized)                        |
| **ConcurrentHashMap** | No order        | ❌ No null key/value          | ✅ Yes (lock-based concurrency) | Segmented Hash Table                             |

---

# 💡 4️⃣ Internal Working of **HashMap**

Let’s understand this deeply (a favorite in interviews 👇)

---

### 🧱 (A) Internal Structure (Pre-Java 8)

- HashMap uses an **array of buckets** internally.
    
- Each bucket is a **Linked List** of key-value pairs (`Node` objects).

### 🧠 Hashing Process

1. You call:
    
    ```java
    map.put("name", "John");
    ```
    
2. Java calculates the **hash code** of `"name"`.
    
3. HashMap uses this hash code to find the **bucket index** in the internal array.
    
4. If no key exists → it creates a new node.  
    If key already exists → updates the value.
---

### ⚡ (B) Internal Structure (Java 8+)

- If too many elements land in the same bucket (collision),  
    and the **bucket size > 8**, the **Linked List becomes a Red-Black Tree** 🌳  
    → Improves performance from **O(n)** to **O(log n)**.
---

# 🧩 5️⃣ Important Concepts

### 🧮 (A) hashCode() and equals() Contract

These two methods are **critical** for how `HashMap` compares keys.

|Method|Purpose|
|---|---|
|**hashCode()**|Determines which bucket the key goes to|
|**equals()**|Checks if two keys in the same bucket are equal|

### Contract:

👉 If two objects are **equal (equals() == true)** → their **hashCodes must be equal**.  
Otherwise, HashMap may store duplicates incorrectly.

✅ Always override both methods when using custom objects as keys!

---

### 🔄 Example of Correct hashCode/equals:

```java
class Employee {
    int id;
    String name;

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Employee)) return false;
        Employee e = (Employee) obj;
        return this.id == e.id;
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}
```

Now this class can safely be used as a **key** in a `HashMap`.

---

# 🧠 6️⃣ Collision Handling

When two different keys produce the **same hash code**,  
they end up in the same **bucket** → called a **collision**.

Java handles collisions using:

- **Linked List chaining** (pre-Java 8)
    
- **Red-Black Tree** (Java 8+)

---
# ⚡ 7️⃣ Common HashMap Methods

```java
map.put(key, value);        // Add or update
map.get(key);               // Retrieve value
map.remove(key);            // Delete by key
map.containsKey(key);       // Check if key exists
map.containsValue(value);   // Check if value exists
map.keySet();               // Returns all keys
map.values();               // Returns all values
map.entrySet();             // Returns key-value pairs
map.size();                 // Returns number of entries
map.clear();                // Remove all entries
```

---

# 🔍 8️⃣ Iterating a Map (3 ways)

### 1️⃣ Using `entrySet()`

```java
for (Map.Entry<Integer, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " : " + entry.getValue());
}
```

### 2️⃣ Using `forEach()` (Java 8)

```java
map.forEach((key, value) -> System.out.println(key + " -> " + value));
```

### 3️⃣ Using `Iterator`

```java
Iterator<Map.Entry<Integer, String>> it = map.entrySet().iterator();
while (it.hasNext()) {
    Map.Entry<Integer, String> entry = it.next();
    System.out.println(entry.getKey() + "=" + entry.getValue());
}
```

---

# 🚀 9️⃣ Performance

|Operation|Average Time|Worst Case (Pre-Java 8)|Worst Case (Java 8+)|
|---|---|---|---|
|put()|O(1)|O(n)|O(log n)|
|get()|O(1)|O(n)|O(log n)|
|remove()|O(1)|O(n)|O(log n)|

---

# 🧵 🔟 Thread Safety

- ❌ `HashMap` is **not thread-safe** (can cause data corruption if modified concurrently).
    
- ✅ Use:
    
    - `Collections.synchronizedMap(new HashMap<>())` → basic sync
        
    - `ConcurrentHashMap` → high-performance concurrent map
        

---

# 🧩 11️⃣ Common Interview Questions

1. How does `HashMap` work internally?
    
2. What happens when two keys have the same hash code?
    
3. What is the difference between `HashMap` and `Hashtable`?
    
4. What’s the time complexity of `get()` and `put()`?
    
5. What is the load factor and threshold in `HashMap`?
    
6. What is rehashing?
    
7. Can HashMap store null keys and values?
    
8. Why should we override `equals()` and `hashCode()` together?
    
9. What is the difference between `HashMap` and `ConcurrentHashMap`?
    
10. What’s new in HashMap since Java 8?
    

---

# 💡 12️⃣ Bonus: Load Factor & Rehashing

- **Load Factor:** Ratio = `size / capacity`  
    (default is 0.75)
    
- When exceeded, the map **doubles its size** and **rehashes** all entries.
    

Example:  
If initial capacity = 16 and load factor = 0.75  
➡️ Rehashing occurs when 12 entries are inserted.

---

# 🧩 13️⃣ Quick Summary

|Concept|HashMap|
|---|---|
|Order|Unordered|
|Nulls|1 null key, many null values|
|Thread Safety|Not thread-safe|
|Internal DS|Array + LinkedList/Tree|
|Performance|O(1) average|
|Java 8 Enhancement|Tree-based buckets|
|Key Rule|Must implement `hashCode()` and `equals()` correctly|

---

Would you like me to show you a **step-by-step visual** (diagram) of how HashMap stores data internally — from `put()` to `get()` — showing buckets, hash, collision, and rehashing?  
It’s extremely useful for interviews like Broadridge or product-based companies.