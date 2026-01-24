# 1️⃣ What is Reflection in Java?

**Reflection** is a Java feature that allows a program to:

> **Inspect, access, and modify classes, methods, fields, and constructors at runtime**, even if their access is `private`.

📦 It is provided by the package:

```java
java.lang.reflect
```

---

## Simple Definition

> _Reflection allows Java code to examine and manipulate the internal structure of classes at runtime without knowing their details at compile time._

---

# 2️⃣ Why Reflection is Needed?

Normally:

```java
MyClass obj = new MyClass();
obj.myMethod();
```

➡️ Class and method **must be known at compile time**

But in real frameworks:

- Class names come from **config**
    
- Objects are created **dynamically**
    
- Methods are invoked **without knowing them in advance**
    

👉 Reflection makes this possible.

---

# 3️⃣ Where Reflection is Used (Real Projects)

### 🔹 Frameworks (MOST IMPORTANT)

- **Spring** – Dependency Injection, AOP
    
- **Hibernate / JPA** – Entity mapping
    
- **Jackson / Gson** – JSON ↔ Object conversion
    
- **JUnit** – Finding and executing test methods
    
- **Lombok** – Analyzing fields/methods
    

> 🔥 Spring cannot exist without reflection.

---

### 🔹 Other Common Uses

- Plugin systems
    
- IDEs (IntelliJ, Eclipse)
    
- Serialization / Deserialization
    
- Annotation processing at runtime
    

---

# 4️⃣ How Reflection Works (Core Classes)

|Class|Purpose|
|---|---|
|`Class`|Represents class metadata|
|`Method`|Represents a method|
|`Field`|Represents a field|
|`Constructor`|Represents a constructor|

---

# 5️⃣ How to Get Class Object (3 Ways)

```java
Class<?> c1 = MyClass.class;

Class<?> c2 = obj.getClass();

Class<?> c3 = Class.forName("com.example.MyClass");
```

👉 **`Class.forName()` is heavily used by frameworks**

---

# 6️⃣ How to Create Object Using Reflection

```java
Class<?> clazz = Class.forName("com.example.User");
Object obj = clazz.getDeclaredConstructor().newInstance();
```

✔ No `new` keyword  
✔ Class name can come from config

---

# 7️⃣ Access Methods Using Reflection

```java
Method method = clazz.getDeclaredMethod("sayHello");
method.setAccessible(true);
method.invoke(obj);
```

---

# 8️⃣ Access Private Fields Using Reflection ⚠️

```java
Field field = clazz.getDeclaredField("name");
field.setAccessible(true);
field.set(obj, "Abhiraj");
```

⚠️ Breaks encapsulation → use carefully

---

# 9️⃣ Reflection + Annotations (VERY IMPORTANT)

```java
if (clazz.isAnnotationPresent(Service.class)) {
    System.out.println("This is a service class");
}
```

👉 **Spring scans annotations using reflection**

---

# 🔟 Spring Example (Behind the Scenes)

```java
@Service
public class OrderService { }
```

Spring internally:

1. Scans classpath
    
2. Finds `@Service`
    
3. Uses reflection to:
    
    - Create object
        
    - Inject dependencies
        
    - Apply AOP proxies
        

---

# 1️⃣1️⃣ Performance Impact ❌

Reflection is:

- Slower than normal method calls
    
- Harder to debug
    
- Less type-safe
    

✔ Acceptable for frameworks  
❌ Avoid in business logic

---

# 1️⃣2️⃣ Security Concerns ⚠️

- Can access `private` members
    
- May violate security manager rules
    

---

# 1️⃣3️⃣ Interview Comparison Table

|Aspect|Normal Code|Reflection|
|---|---|---|
|Compile-time safety|Yes|No|
|Performance|Fast|Slower|
|Flexibility|Low|Very High|
|Used in frameworks|❌|✅|

---

# 1️⃣4️⃣ One-Liner Interview Answer 🎯

> _Reflection is a Java mechanism that allows inspecting and manipulating classes, methods, and fields at runtime. It is heavily used by frameworks like Spring and Hibernate for dependency injection, annotation processing, and dynamic object creation._

---

# 1️⃣5️⃣ Common Interview Follow-up Questions

**Q:** Can we avoid reflection in Spring?  
➡️ _No, core Spring features depend on it._

**Q:** Is reflection used in `@Autowired`?  
➡️ _Yes, Spring uses reflection to inject dependencies._

**Q:** Is reflection bad practice?  
➡️ _Not in frameworks, but should be avoided in application logic._

---

If you want next:

- Reflection vs ClassLoader
    
- How Spring uses reflection step-by-step
    
- Difference between Reflection & AOP
    
- Real interview coding questions on reflection
    

Just say 👍