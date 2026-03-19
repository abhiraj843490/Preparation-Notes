
- `Set` is an **interface** in the Java Collections Framework (`java.util.Set`).
    
- It represents a **collection of unique elements** — meaning **no duplicates allowed**.
    
- Think of it like a **mathematical set** — `{A, B, C}` — you can’t have another “A”.

Example:

```java
Set<String> names = new HashSet<>();
names.add("Abhi");
names.add("Raj");
names.add("Abhi"); // duplicate — will be ignored
System.out.println(names);
```

Output:

```
[Abhi, Raj]
```

---

## 🧠 2️⃣ Why Use a Set?

- When you need to ensure uniqueness (e.g., user IDs, email addresses).
    
- Faster lookups (compared to lists for `contains()` checks).
    
- Ideal for operations like **union**, **intersection**, **difference**.

---

## ⚙️ 3️⃣ Common Implementations

|Implementation|Ordering|Null Allowed|Thread Safe|Underlying DS|Performance Notes|
|---|---|---|---|---|---|
|**HashSet**|No order|✅ Yes (1 null)|❌ No|HashMap|O(1) add, remove, contains|
|**LinkedHashSet**|Maintains insertion order|✅ Yes (1 null)|❌ No|LinkedHashMap|Slightly slower but predictable order|
|**TreeSet**|Sorted (natural or custom Comparator)|❌ No|❌ No|TreeMap (Red-Black tree)|O(log n) operations|
|**CopyOnWriteArraySet**|Insertion order|✅ Yes (1 null)|✅ Yes|CopyOnWriteArrayList|Slower for writes, good for concurrent reads|

---

## 🔍 4️⃣ Internal Working — `HashSet`

`HashSet` is actually backed by a **`HashMap`** internally.

When you do:

```java
Set<String> set = new HashSet<>();
set.add("Hello");
```

Internally, it does something like:

```java
map.put("Hello", PRESENT);
```

Where `PRESENT` is a static dummy object.

So the `HashSet` only cares about the **keys** of the map, which ensures uniqueness.

---

## 5️⃣ How Uniqueness Works (hashCode + equals)

When you add an element:

1. The element’s **`hashCode()`** determines the bucket.
    
2. If another element exists in the same bucket, it checks **`equals()`**.
    
3. If both hashCode and equals are same → duplicate, not added.
    
4. Else → added as a new unique entry.

Example:

```java
Set<String> set = new HashSet<>();
set.add("Apple");
set.add(new String("Apple")); // same value, different object
System.out.println(set.size()); // 1 (because equals() = true)
```

---

## 🧮 6️⃣ Example – Sorting and Uniqueness

```java
Set<Integer> nums = new TreeSet<>();
nums.add(5);
nums.add(1);
nums.add(5);
nums.add(3);

System.out.println(nums);
```

Output:

```
[1, 3, 5] // sorted and unique
```

---

## 🧵 7️⃣ Example – LinkedHashSet (Maintain Order)

```java
Set<String> set = new LinkedHashSet<>();
set.add("A");
set.add("C");
set.add("B");
set.add("A");

System.out.println(set);
```

Output:

```
[A, C, B] // maintains insertion order
```

---

## ⚔️ 8️⃣ Example – HashSet vs TreeSet

|Feature|HashSet|TreeSet|
|---|---|---|
|Order|Unordered|Sorted|
|Null Allowed|Yes|No|
|Performance|Faster (O(1))|Slower (O(log n))|
|Comparator|Not applicable|Can use Comparator|

---

## ⚙️ 9️⃣ Set Operations (Java 8 style)

```java
Set<Integer> a = new HashSet<>(List.of(1, 2, 3, 4));
Set<Integer> b = new HashSet<>(List.of(3, 4, 5, 6));

// Union
Set<Integer> union = new HashSet<>(a);
union.addAll(b); // [1,2,3,4,5,6]

// Intersection
Set<Integer> intersection = new HashSet<>(a);
intersection.retainAll(b); // [3,4]

// Difference
Set<Integer> difference = new HashSet<>(a);
difference.removeAll(b); // [1,2]
```

---

## 💡 10️⃣ When to Use Which

- ✅ **HashSet** → Best for performance, uniqueness only.
    
- ✅ **LinkedHashSet** → If you want to preserve insertion order.
    
- ✅ **TreeSet** → If you want elements to be sorted.
    
- ✅ **CopyOnWriteArraySet** → For thread-safe read-heavy collections.

---

Would you like me to continue with a **visual diagram of internal HashSet working (buckets, hash collisions, equals checks)** to make it clearer?