When you run a Java program, the JVM divides memory mainly into two parts:

|**Memory Area**|**Purpose**|
|---|---|
|🧱 **Stack Memory**|Stores **method calls**, **local variables**, and **reference variables**.|
|🗃️ **Heap Memory**|Stores **objects** and **instance variables** created using `new`.|

## ⚙️ **2️⃣ How They Work Together**

```java
public class MemoryExample {
    public static void main(String[] args) {
        int x = 10;                        // stored in Stack
        String s = new String("Hello");    // reference 's' in Stack, object in Heap
    }
}
```

🧩 **Explanation:**

- The variable `x` (primitive) → stored directly in **stack memory**.
    
- The reference variable `s` → stored in **stack**, but it points to an object `"Hello"` stored in **heap**.
    
---

## 🧱 **3️⃣ Stack Memory (Details)**

- **Stores:** Local variables, method calls, references.
- **Memory Size:** Smaller (per thread).
- **Managed By:** JVM automatically — LIFO (Last In First Out).
- **Thread Behavior:** Each thread has its **own stack**.
- **Access Speed:** Very fast.
- **Lifetime:** Exists until the method finishes execution.

### ⚡ Example:

```java
void methodA() {
    int a = 5;
    int b = 10;
}
```

➡️ Both `a` and `b` are stored on the stack.  
Once `methodA()` finishes, this memory is cleared automatically.

---

## 🗃️ **4️⃣ Heap Memory

- **Stores:** Objects and instance variables.
- **Memory Size:** Larger.
- **Managed By:** JVM Garbage Collector (GC).
- **Thread Behavior:** Shared among all threads (⚠️ needs thread safety).
- **Access Speed:** Slower than stack (because it’s global).
- **Lifetime:** Until GC removes unreferenced objects.

### ⚡ Example:

```java
Person p = new Person(); // p is in stack, Person object is in heap
```

---

## 🔁 **5️⃣ Relationship Between Stack & Heap**

```
Stack (per thread)                 Heap (shared)
------------------                 ---------------------
| ref: p ---------------> points to | Person object     |
| int x = 5;           |            | name="John"       |
| method call frames   |            | age=30            |
------------------                 ---------------------
```

---

## 🔄 **6️⃣ Memory Cleanup**

|Type|Cleanup Method|
|---|---|
|**Stack**|Automatically cleared after method execution.|
|**Heap**|Cleared by **Garbage Collector** when no references remain.|

---

## 🚦 **7️⃣ Common Interview Points**

|Question|Answer|
|---|---|
|What happens if stack memory is full?|`StackOverflowError`|
|What happens if heap memory is full?|`OutOfMemoryError: Java heap space`|
|Who manages garbage collection?|JVM Garbage Collector|
|Are objects thread-safe by default in heap?|❌ No, heap is shared between threads|

---

## 🧩 **8️⃣ Summary Table**

|Feature|**Stack Memory**|**Heap Memory**|
|---|---|---|
|Stores|Method calls, local & reference variables|Objects, instance variables|
|Access|Fast|Relatively slow|
|Managed By|JVM (auto push/pop)|JVM Garbage Collector|
|Thread Access|Each thread has own stack|Shared by all threads|
|Lifetime|Until method ends|Until GC cleans up|
|Error Type|StackOverflowError|OutOfMemoryError|

---

## 🧠 Example Visual

```
main() Stack Frame
-------------------------
| int x = 10             |
| String s --> (ref) ----|--------> Heap: "Hello"
-------------------------
```

---

✅ **In One Line:**

> Stack stores _where_ things are (references & method calls);  
> Heap stores _what_ things are (actual objects and data).
