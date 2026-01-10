							== vs .equals()

==  compare the reference of variable
.equals()  compare the content of variable

String s1 = "hello";
String s2 = "hello";
String s3 = new String("hello");

s1.equals(s2)// true
s1.equals(s3)//ture

- `"hello"` (from literal) exists in the **String pool**.
- `new String("hello")` creates a **separate copy in the heap**.



|Feature|**StringBuffer**|**StringBuilder**|
|---|---|---|
|**Introduced in**|JDK 1.0|JDK 1.5|
|**Synchronization**|✅ **Synchronized** (Thread-safe)|❌ **Not synchronized** (Not thread-safe)|
|**Performance**|Slower (due to synchronization overhead)|Faster (no synchronization)|
|**Use Case**|Use in **multi-threaded** environments|Use in **single-threaded** environments|
|**Mutability**|Both are **mutable** (can change content without creating new object)|Both are **mutable**|

## ⚙️ **3️⃣ Internal Working**

- Both classes store data in a **char[] array (character array)** internally.
- When you append more data and the capacity exceeds, they:
    - Create a new larger array (usually `oldCapacity * 2 + 2`)
    - Copy existing characters into it.
- Unlike `String`, they **don’t create a new object** each time content changes.

## 🧵 **1️⃣ What is “Thread-Safe”?**

> “When multiple threads access the same object _at the same time_, the object behaves correctly (no data corruption, no inconsistent state).”

- In a **thread-safe class**, internal data is protected from concurrent modifications.
- It usually achieves this by using **synchronization**, **locks**, or **immutable design**.