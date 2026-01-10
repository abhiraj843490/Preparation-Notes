An **immutable class** is a class whose **state (field values) cannot be changed** after it’s created.

---
## 🧩 **1️⃣ What is an Immutable Class?**

Once you create an object, you **can’t modify** any of its fields — instead, you create a new object if you need changes.

👉 Example:  
`String` in Java is the most famous **immutable** class.

---
## 🧩 **2️⃣ Rules to Create an Immutable Class**

To make a class immutable, follow these **5 golden rules**:

| Step | Rule                                                                                                           | Why                                                  |
| ---- | -------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| 1    | Declare the class as `final`                                                                                   | Prevent subclassing (which can add mutable behavior) |
| 2    | Make all fields `private`                                                                                      | Prevent direct access                                |
| 3    | Make all fields `final`                                                                                        | Prevent reassignment after constructor               |
| 4    | Initialize all fields via constructor                                                                          | Ensure they are set only once                        |
| 5    | Do not provide any setter methods                                                                              | Prevent modification after creation                  |
| 6    | If a field is a mutable object (like `Date`, `List`, `Map`), return a **deep copy** in getters and constructor | Prevent indirect mutation                            |

---

## 🧩 **3️⃣ Example: Immutable Class**

```java
import java.util.Date;

public final class Employee {
    private final String name;
    private final int id;
    private final Date joiningDate;  // mutable object!

    public Employee(String name, int id, Date joiningDate) {
        this.name = name;
        this.id = id;
        // Defensive copy
        this.joiningDate = new Date(joiningDate.getTime());
    }

    public String getName() {
        return name;
    }

    public int getId() {
        return id;
    }

    // Defensive copy again to prevent external modification
    public Date getJoiningDate() {
        return new Date(joiningDate.getTime());
    }

    @Override
    public String toString() {
        return "Employee{name='" + name + "', id=" + id + ", joiningDate=" + joiningDate + '}';
    }
}
```

---

## 🧠 **4️⃣ Explanation**

✅ **`final class`** — prevents subclass modification.  
✅ **`private final fields`** — ensure once assigned, never changed.  
✅ **No setters** — external classes can’t modify fields.  
✅ **Defensive copying** — prevents changes to mutable objects like `Date`.

---

## 🧩 **5️⃣ Test the Immutability**

```java
public class TestImmutable {
    public static void main(String[] args) {
        Date date = new Date();
        Employee emp = new Employee("John", 101, date);

        System.out.println(emp);

        // Try to modify the original date
        date.setTime(123456789);

        // Employee's joining date remains unchanged
        System.out.println(emp);
    }
}
```

🟢 Output:

```
Employee{name='John', id=101, joiningDate=Sun Oct 05 13:00:00 IST 2025}
Employee{name='John', id=101, joiningDate=Sun Oct 05 13:00:00 IST 2025}
```

💡 The internal state didn’t change — ✅ class is immutable.

---

## 🧩 **6️⃣ Bonus: Using Java Record (Shortcut)**

Since Java 16+, **records** are inherently immutable.  
Equivalent record:

```java
public record Employee(String name, int id, Date joiningDate) {
    public Employee {
        joiningDate = new Date(joiningDate.getTime()); // defensive copy
    }

    public Date joiningDate() {
        return new Date(joiningDate.getTime()); // defensive copy
    }
}
```
## Note
## Records are **_shallowly immutable by design_**.
### 1. Fields are implicitly `private final`

When you write:

``` java 

public record User(int id, String name) {}

The compiler generates:

private final int id; 
private final String name;

✔ `final` → cannot be reassigned after construction.

```
### 2. No setters are generated

Records automatically generate:
- Constructor
- Getters (called accessors, not getters)
- `equals()`, `hashCode()`, `toString()`
❌ They **do NOT generate setters**.

So this is invalid:
```java
user.id = 10;     // ❌ compile-time error 
user.setName("X"); // ❌ no setter
```

### 3. Canonical constructor assigns values once

Generated constructor:

```java 
public User(int id, String name) {     
	this.id = id;     
	this.name = name; 
}
```

Once assigned → cannot change.

---
### 🔹 4. Class is implicitly `final`

Records cannot be extended:

``` java
public record User(int id, String name) {}
```

❌ This is illegal:

``` java 
class Admin extends User {} // ❌ compile-time error
```
✔ Prevents mutation via subclassing.

---

### 🔹 5. Accessors return values, not references

Accessor methods:
``` java
int id() { return id; } String name() { return name; }
```
No mutation possible through accessors.

---

## ⚠️ Important Interview Point: Shallow Immutability

Records are **NOT deeply immutable** by default.

Example:

``` java
public record User(List<String> roles) {}
```

This is dangerous:
```java
user.roles().add("ADMIN"); // ❌ mutation allowed
```
### ✔ How to make it deeply immutable?

Use **defensive copying**:

```java
public record User(List<String> roles) {     
	public User {         
		roles = List.copyOf(roles);     
	} 
}
```

---

## 🧠 **Interview Answer**

> “To make a class immutable, I declare it as final, make all fields private and final, initialize them through the constructor, avoid providing setters, and for any mutable object fields, I perform deep copies in both the constructor and getters. This ensures the internal state of the object cannot change after construction.”

---

Would you like me to give a **mutable vs immutable** example side-by-side for clarity (often asked as a follow-up)?