These are **design patterns** that deal with **object creation mechanisms** — how you instantiate classes while keeping the code **flexible, reusable, and maintainable**.

---
## **Types of Creational Design Patterns**

There are **5 main creational patterns**:

|Pattern|Main Idea|When to Use|
|---|---|---|
|**Singleton**|Only one instance of a class exists|Global config, logging, DB connection pool|
|**Factory Method**|Creates objects without exposing instantiation logic|When subclasses decide what to create|
|**Abstract Factory**|Creates families of related objects without specifying their concrete classes|When you need multiple related factories|
|**Builder**|Step-by-step creation of complex objects|When object construction has many optional parameters|
|**Prototype**|Clones objects instead of creating new instances|When object creation is costly|

---

### **1. Singleton Pattern**

> Ensures a class has **only one instance** and provides a global access point.

**Example (Java)**

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() { } // Private constructor

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton(); // Lazy initialization
        }
        return instance;
    }
}
```

**When to Use**

- Logger classes
- Database connection pools
- Configuration managers

---

### **2. Factory Method Pattern**

> Defines an interface for creating objects, but **subclasses decide** which object to create.

**Example (Java)**

```java
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() { System.out.println("Drawing Circle"); }
}

class Square implements Shape {
    public void draw() { System.out.println("Drawing Square"); }
}

class ShapeFactory {
    public Shape getShape(String type) {
        if ("circle".equalsIgnoreCase(type)) return new Circle();
        else if ("square".equalsIgnoreCase(type)) return new Square();
        return null;
    }
}

public class Main {
    public static void main(String[] args) {
        ShapeFactory factory = new ShapeFactory();
        Shape shape = factory.getShape("circle");
        shape.draw();
    }
}
```

**When to Use**

- When object creation logic is complex
- When you want to avoid directly calling constructors in client code

---

### **3. Abstract Factory Pattern**

> Provides an interface to create **families of related objects** without specifying their concrete classes.  
> It's like a **factory of factories**.

**Example (Java)**

```java
interface Button {
    void paint();
}
class WindowsButton implements Button {
    public void paint() { System.out.println("Windows Button"); }
}
class MacButton implements Button {
    public void paint() { System.out.println("Mac Button"); }
}

interface GUIFactory {
    Button createButton();
}
class WindowsFactory implements GUIFactory {
    public Button createButton() { return new WindowsButton(); }
}
class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
}

public class Main {
    public static void main(String[] args) {
        GUIFactory factory = new WindowsFactory(); // Could switch to MacFactory
        Button button = factory.createButton();
        button.paint();
    }
}
```

**When to Use**

- Cross-platform UI components
- Families of products (buttons, textboxes, menus)

---

### **4. Builder Pattern**

> Separates object construction from its representation, allowing step-by-step creation.

**Example (Java)**

```java
class Computer {
    private String CPU;
    private String RAM;
    private String storage;

    public Computer(String CPU, String RAM, String storage) {
        this.CPU = CPU;
        this.RAM = RAM;
        this.storage = storage;
    }

    public String toString() {
        return CPU + ", " + RAM + ", " + storage;
    }

    static class ComputerBuilder {
        private String CPU;
        private String RAM;
        private String storage;

        public ComputerBuilder setCPU(String CPU) {
            this.CPU = CPU; return this;
        }
        public ComputerBuilder setRAM(String RAM) {
            this.RAM = RAM; return this;
        }
        public ComputerBuilder setStorage(String storage) {
            this.storage = storage; return this;
        }
        public Computer build() {
            return new Computer(CPU, RAM, storage);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Computer comp = new Computer.ComputerBuilder()
                .setCPU("Intel i7")
                .setRAM("16GB")
                .setStorage("512GB SSD")
                .build();
        System.out.println(comp);
    }
}
```

**When to Use**

- When objects have many optional parameters
- When constructing complex immutable objects
---

### **5. Prototype Pattern**

> Creates new objects by **cloning** an existing object.

**Example (Java)**

```java
class Employee implements Cloneable {
    String name;

    Employee(String name) { this.name = name; }

    public Employee clone() throws CloneNotSupportedException {
        return (Employee) super.clone();
    }
}

public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {
        Employee emp1 = new Employee("John");
        Employee emp2 = emp1.clone();
        System.out.println(emp1.name); // John
        System.out.println(emp2.name); // John
    }
}
```

**When to Use**

- When object creation is expensive (loading from DB, network, etc.)
- When you need exact copies of objects
---

✅ **Summary Table**

|Pattern|Intent|Example Use Case|
|---|---|---|
|Singleton|One instance only|Logger, config|
|Factory Method|Let subclasses decide what to create|Shape creation|
|Abstract Factory|Create families of related objects|Cross-platform UI|
|Builder|Build complex objects step-by-step|Computer builder|
|Prototype|Clone existing objects|Object templates|

---

If you want, I can make you a **mind map** linking these **Creational Patterns** with **real-world analogies** so you’ll remember them instantly.  
Do you want me to make that next?