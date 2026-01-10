A **sealed class** (introduced in **Java 17**) is a special kind of class that **restricts which other classes can extend or implement it**.

It’s a way to have **controlled inheritance** — unlike normal classes where _anyone_ can extend, a sealed class allows _only specific subclasses_.

---

### 🔹 Why do we need Sealed Classes?

They bring **more control, safety, and clarity** to your class hierarchies.

Use cases:

- To limit inheritance (for security or domain modeling).
    
- To make `switch` expressions exhaustive (the compiler knows all possible subtypes).
    
- To combine the best of `enum` and polymorphism.
    

---

### 🧠 Syntax

```java
public sealed class Shape permits Circle, Rectangle, Square {
    // common fields and methods
}

final class Circle extends Shape {
    // no further subclassing allowed
}

non-sealed class Rectangle extends Shape {
    // can be subclassed freely
}

sealed class Square extends Shape permits SmallSquare {
    // only SmallSquare can extend it
}

final class SmallSquare extends Square {}
```

---

### ⚙️ Keywords Explained

|Keyword|Meaning|
|---|---|
|**sealed**|Marks class/interface as sealed and restricts who can extend it.|
|**permits**|Lists allowed subclasses.|
|**final**|Subclass cannot be extended further.|
|**non-sealed**|Removes the restriction — subclass can be extended freely.|

---

### 🧩 Rules

1. Every permitted subclass **must** be in the same module (or package if no module).
    
2. Each permitted subclass **must explicitly declare** itself as `final`, `sealed`, or `non-sealed`.
    
3. The `permits` clause can be omitted if all permitted subclasses are defined in the same file.
    

---

### ✅ Example in Real Context

Let’s say you are modeling **Payment Methods**:

```java
public sealed class Payment permits CreditCard, UPI, Cash {}

final class CreditCard extends Payment {}
final class UPI extends Payment {}
final class Cash extends Payment {}
```

Here — only `CreditCard`, `UPI`, and `Cash` can represent `Payment`.  
If someone tries:

```java
class CryptoPayment extends Payment {}
```

👉 Compiler error ❌ (not permitted).

---

### 💡 With Pattern Matching `switch` (Java 17+)

Sealed classes make `switch` exhaustive — the compiler knows all subtypes.

```java
static String paymentInfo(Payment p) {
    return switch (p) {
        case CreditCard c -> "Paid by Card";
        case UPI u -> "Paid by UPI";
        case Cash cash -> "Paid in Cash";
    };
}
```

No `default` needed — compiler ensures all types are handled.

---

### 🚀 Summary Table

|Concept|Description|
|---|---|
|Introduced|Java 17|
|Purpose|Restrict inheritance|
|Keywords|sealed, permits, final, non-sealed|
|Benefits|Type safety, exhaustive pattern matching, clearer class design|

---

### 💬 Interview Tip

If asked **"Why not just make everything final?"**  
✅ Say:

> Sealed classes allow some extension — but only controlled ones — providing flexibility + safety.  
> They sit between `final` (no extension) and `open` (anyone can extend).
