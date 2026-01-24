
> **There are 4 types of interfaces in Java.**

---

## 1️⃣ Normal Interface (Classic Interface)

- Can have:
    
    - abstract methods
        
    - `default` methods (Java 8+)
        
    - `static` methods
    
    - `public static final` constants
    
- Cannot have instance fields (only `public static final` constants)
    

```java
interface PaymentService {
    void pay();
}
```

---

## 2️⃣ Functional Interface (Java 8+)

- **Exactly one abstract method**
    
- Can have:
    
    - default methods
    
    - static methods
    
- Used in **lambda expressions**

```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}
```

Examples:

- `Runnable`
- `Callable`
- `Comparator`
- `Predicate`, consumer, supplier, Function

---

## 3️⃣ Marker Interface

Marker interface is an empty interface used to mark a class. 
JVM or frameworks use this marker to enable or restrict certain behavior at runtime. 
They were heavily used before annotations
useful for type-safe, low-level checks in production systems.

- **No methods**
    
- Used to **mark** a class
    
- JVM or framework checks presence using `instanceof`

```java
interface Serializable { }
```

Examples:

- `Serializable`
    
- `Cloneable`
    
- `RandomAccess`

---

## 4️⃣ Nested Interface

- Interface inside another class or interface
    
- Used for **scoping and grouping**
    

```java
class Outer {
    interface Inner {
        void show();
    }
}
```

---

## ⚠️ Alternative Interview Classification (Advanced)

Some interviewers may also accept:

### 🔹 SAM Interface

(Single Abstract Method → same as Functional Interface)

### 🔹 Sealed Interface (Java 17+)

```java
sealed interface Shape permits Circle, Square {}
```

---

## 🎯 Interview-Ready Summary Table

|Type|Methods|Java Version|
|---|---|---|
|Normal|Multiple abstract|Since Java 1.0|
|Functional|One abstract|Java 8+|
|Marker|No methods|Since Java 1.0|
|Nested|Any|Since Java 1.1|
|Sealed|Restricted implementations|Java 17+|

---

## 🔥 One-Line Killer Answer

> **Java supports Normal, Functional, Marker, and Nested interfaces; additionally, Java 17 introduced Sealed interfaces.**

If you want:

- 💡 **Difference between abstract class and interface**
    
- ⚡ **Why marker interfaces exist**
    
- 🧠 **Functional interface interview traps**
    

Just tell me 👍




