## 🔷 **1. What is an Abstract Class?**

An **abstract class** is a class that:

- **Cannot be instantiated**
- Can contain **abstract methods** (without body)
- Can also contain **concrete methods** (with body)
- Can have **constructors**, **fields**, and **state**

```java
abstract class Animal {
    String name;            // state
    Animal(String name){    // constructor
        this.name = name;
    }

    abstract void sound();  // abstract method

    void sleep(){           // concrete method
        System.out.println("Sleeping");
    }
}
```

---

## 🔷 **2. What is an Interface?**

An **interface** is a fully abstract contract that:

- Contains **abstract methods** (implicitly public and abstract)
- Cannot hold instance fields (only constants)
- **Cannot have constructors**
- A class can implement **multiple interfaces**

Since Java 8+, an interface can also have:

- **default methods**
- **static methods**
- **private methods (Java 9+)**


### ➤ Example:

```java
interface Animal {
    void sound(); // abstract

    default void sleep() {
        System.out.println("Default Sleep");
    }
}
```

---

# 🔥 **Key Differences (Interview-Ready Table)**

| Feature                  | Abstract Class                                                    | Interface                                       |
| ------------------------ | ----------------------------------------------------------------- | ----------------------------------------------- |
| **Methods**              | Can have abstract + concrete methods                              | Abstract + default + static + private           |
| **Fields**               | Can have fields (state)                                           | Only public static final constants              |
| **Constructor**          | ✔ Yes                                                             | ✖ No                                            |
| **Multiple Inheritance** | ✖ No (one superclass only)                                        | ✔ Yes (class can implement many interfaces)     |
| **Access Modifiers**     | public, protected, private                                        | All methods are public by default               |
| **Use Case**             | When you want to share **common code + state**                    | When you want **pure abstraction / a contract** |
| **Performance**          | Slightly faster (because no dynamic dispatch for default methods) | Slightly slower (multiple method dispatch)      |

---

# 🎯 **When to Use What? (Critical for Interviews)**

### ✔ Use **Abstract Class** when:

- You need a **base class with shared code**
- You want to maintain **state (instance variables)**
- You want to provide **some default common behavior**
- You expect future extension through inheritance

#### Example:

`Vehicle` → Car, Truck (common fields like speed, color)

---

### ✔ Use **Interface** when:

- You want to provide a **capability** or **behavior**  
    (e.g., Comparable, Serializable)
- You want **multiple inheritance**
- You want to define a **contract without state**
- You want **loose coupling**

#### Example:

`Runnable`, `Callable`, `Comparator`, `List`, `Map`

---

# 🚀 Real-World Example (Best for Interview)

### ⚡ Abstract Class Example

```java
abstract class Payment {
    double amount;

    Payment(double amount) {
        this.amount = amount;
    }

    abstract void pay();
}
```

### ⚡ Interface Example

```java
interface Online {
    void authenticate();
}
```

### ⚡ Usage

```java
class UpiPayment extends Payment implements Online {

    UpiPayment(double amt) {
        super(amt);
    }

    public void authenticate() {
        System.out.println("UPI Auth");
    }

    void pay() {
        System.out.println("Paid via UPI: " + amount);
    }
}
```

---

# 🧠 **One-line Interview Answer**

> **Use an abstract class when you need shared state and default behavior.  
> Use an interface when you need a contract that supports multiple inheritance and loose coupling.**

---

If you want, I can also give:  
✔ **Top 20 abstract class vs interface interview questions**  
✔ **Trick MCQs asked in HackerRank/OA**  
✔ **Real coding questions related to interfaces & abstraction**

