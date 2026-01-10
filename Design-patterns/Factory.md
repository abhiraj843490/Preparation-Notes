What is the Factory Design Pattern

---
## **Factory Design Pattern**

👉 **Definition**  
The **Factory Design Pattern** is a **creational design pattern** that provides an interface for creating objects in a superclass but allows subclasses or a dedicated "factory class" to decide which object to create.

In short:

- It **hides object creation logic** from the client.
- Client code **depends on abstraction**, not concrete classes.
---

### **Key Points for Interview**

- **Type**: Creational Pattern
- **Purpose**: To delegate the instantiation logic to a separate factory class.
- **Benefit**: Promotes loose coupling, improves maintainability, and adheres to **Open/Closed Principle**.
    
- **Real-life analogy**:
    - You go to a restaurant and order “Pizza”.
    - The **factory (kitchen)** decides whether to make a Veg Pizza or Non-Veg Pizza — you don’t need to know the creation details.
---

### **Code Example (Java)**

```java
// Step 1: Product interface
interface Shape {
    void draw();
}

// Step 2: Concrete products
class Circle implements Shape {
    public void draw() { System.out.println("Drawing Circle"); }
}
class Square implements Shape {
    public void draw() { System.out.println("Drawing Square"); }
}

// Step 3: Factory class
class ShapeFactory {
    public Shape getShape(String shapeType) {
        if ("CIRCLE".equalsIgnoreCase(shapeType)) {
            return new Circle();
        } else if ("SQUARE".equalsIgnoreCase(shapeType)) {
            return new Square();
        }
        return null;
    }
}

// Step 4: Client code
public class Main {
    public static void main(String[] args) {
        ShapeFactory factory = new ShapeFactory();

        Shape shape1 = factory.getShape("CIRCLE");
        shape1.draw();  // Output: Drawing Circle

        Shape shape2 = factory.getShape("SQUARE");
        shape2.draw();  // Output: Drawing Square
    }
}
```

### **Advantages**

1. Centralizes object creation logic.
2. Promotes **loose coupling** (client depends on abstraction, not implementation).
3. Makes code **easier to maintain and extend**.
4. Supports **OCP (Open/Closed Principle)**.
---

### **Disadvantages**

- May add **extra classes** (factories).
- Can lead to **complex hierarchy** if overused.
---

✅ **Interview-ready one-liner**:

> _“The Factory Design Pattern is a creational pattern that delegates object creation to a factory class, helping achieve loose coupling by letting the client work with interfaces instead of concrete classes.”_

---

Would you like me to also cover **Factory vs Abstract Factory** differences? (This is a common **follow-up interview question**).