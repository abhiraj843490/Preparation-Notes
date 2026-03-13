Behavioral patterns are concerned with **how objects interact and communicate** with each other.  
They focus on **the behavior of classes and objects** and **the flow of control** in a system.

---
In short —

> 🗣️ They define **how responsibilities are distributed** among classes and **how objects cooperate** to achieve a task.
---
## 🧩 **List of Common Behavioral Design Patterns**

| Pattern                        | Description                                                                         | Real-life Analogy                                                   |
| ------------------------------ | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **1. Strategy**                | Defines a family of algorithms and lets you choose one at runtime.                  | Different payment methods (UPI, Credit Card, PayPal)                |
| **2. Observer**                | When one object changes, all its dependents are notified automatically.             | YouTube subscribers get notified when a channel uploads a new video |
| **3. Command**                 | Encapsulates a request as an object — helps with undo/redo, queueing, etc.          | Remote control buttons sending commands to a TV                     |
| **4. State**                   | Object changes its behavior when its internal state changes.                        | A fan changing speed (off → low → medium → high)                    |
| **5. Template Method**         | Defines the skeleton of an algorithm but lets subclasses define certain steps.      | Making tea vs coffee (same process, different ingredients)          |
| **6. Chain of Responsibility** | Passes a request along a chain of handlers until one handles it.                    | Customer support escalation (Level 1 → Level 2 → Level 3)           |
| **7. Mediator**                | Reduces complex communication between objects by using a mediator object.           | Air traffic controller coordinating multiple airplanes              |
| **8. Iterator**                | Provides a way to access elements sequentially without exposing internal structure. | For-each loop over a list                                           |
| **9. Memento**                 | Captures an object’s internal state to restore it later (undo).                     | Undo feature in a text editor                                       |
| **10. Visitor**                | Adds new operations to existing object structures without changing the classes.     | Tax calculator visiting different asset types                       |
| **11. Interpreter**            | Defines a language grammar and interprets sentences in that language.               | Regular expression engines, calculators                             |

---
## **Strategy Pattern (Very Common in Interviews)**

### Problem:

You want to support multiple **payment methods** in your app — UPI, Credit Card, or PayPal —  
without writing `if-else` or `switch` logic everywhere.
### Solution:

Use the **Strategy Pattern** — define a common interface for all payment methods,  
and inject the one you want at runtime.

```java
// Strategy Interface
interface PaymentStrategy {
    void pay(int amount);
}

// Concrete Strategies
class CreditCardPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card");
    }
}

class UPIPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using UPI");
    }
}

// Context
class PaymentService {
    private PaymentStrategy strategy;

    public PaymentService(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void makePayment(int amount) {
        strategy.pay(amount);
    }
}

// Test
public class Main {
    public static void main(String[] args) {
        PaymentService service = new PaymentService(new UPIPayment());
        service.makePayment(500); // Paid 500 using UPI
    }
}
```

**✅ Advantages:**

- Open/Closed Principle — new strategies can be added easily.
- Avoids large `if-else` blocks.
- Makes behavior interchangeable at runtime.

---

## 💬 Quick Tip for Interviews

When an interviewer asks:

> “What are Behavioral Design Patterns?”

You can say:

> “Behavioral design patterns focus on how objects communicate and assign responsibilities among them. Examples include Strategy, Observer, Command, State, Template Method, and Chain of Responsibility.”
---
