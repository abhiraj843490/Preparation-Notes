
> Dependency Injection is a design principle where dependencies are provided externally by the Spring container, promoting loose coupling, testability, and maintainability. 
> 
> Constructor injection is preferred because it ensures immutability and mandatory dependencies.


## 🔹 What is Dependency Injection?

**Dependency Injection** is a design principle where:

> **Objects do NOT create their dependencies. Instead, dependencies are provided by an external container (Spring IoC).**

📌 This promotes **loose coupling**, **testability**, and **maintainability**.

---

## ❌ Without Dependency Injection (Tight Coupling)

```java
class OrderService {
    private PaymentService paymentService = new PaymentService(); // ❌ tight coupling
}
```

Problems:

- Cannot change implementation easily
    
- Hard to unit test
    
- Violates SOLID (Dependency Inversion)


---

## ✅ With Dependency Injection

```java
class OrderService {
    private PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

✔ Loose coupling  
✔ Easy to test  
✔ Flexible

---

# 🧠 DI + IoC Relationship

|Term|Meaning|
|---|---|
|IoC (Inversion of Control)|Control of object creation shifted to Spring|
|DI|A way to implement IoC|

👉 **Spring container performs DI**.

---

# 🧩 Types of Dependency Injection (INTERVIEW MUST)

---

## 1️⃣ Constructor Injection (⭐ BEST PRACTICE)

```java
@Service
public class UserService {

    private final UserRepo repo;

    @Autowired
    public UserService(UserRepo repo) {
        this.repo = repo;
    }
}
```

### ✔ Advantages:

- Immutable dependencies
    
- Mandatory dependencies enforced
    
- Thread-safe
    
- Best for unit testing
    

### ✔ Interview Answer:

> Constructor injection ensures dependencies are available at object creation time.

---

## 2️⃣ Setter Injection

```java
@Service
public class UserService {

    private UserRepo repo;

    @Autowired
    public void setRepo(UserRepo repo) {
        this.repo = repo;
    }
}
```

### ✔ Use when:

- Dependency is optional
    
- Needs runtime change
    

### ❌ Cons:

- Object can be in invalid state
    

---

## 3️⃣ Field Injection (❌ NOT Recommended)

```java
@Autowired
private UserRepo repo;
```

### ❌ Why avoid?

- Hard to test
    
- Breaks immutability
    
- Reflection-based
    

📌 Interview Tip:

> Use field injection only for prototypes or test demos.

---

# 🎯 Autowiring Modes

|Mode|Description|
|---|---|
|byType|Default|
|byName|Matches bean name|
|constructor|Uses constructor|
|@Qualifier|Resolve ambiguity|

---

## 🔍 @Qualifier Example

```java
@Autowired
@Qualifier("mysqlRepo")
private UserRepo repo;
```

---

## 🏷 @Primary Example

```java
@Primary
@Repository
public class MySqlRepo implements UserRepo {}
```

---

# 🧪 Dependency Injection & Unit Testing

Constructor injection makes mocking easy:

```java
UserRepo repo = mock(UserRepo.class);
UserService service = new UserService(repo);
```

✔ No Spring context required

---

# 🔄 DI During Bean Lifecycle

DI happens **after instantiation and before initialization**.

```
1. Constructor call
2. Dependency Injection
3. @PostConstruct
4. Bean Ready
```

---

# 🧠 DI vs Factory Pattern

|DI|Factory|
|---|---|
|Container-managed|Manual|
|Loose coupling|Moderate|
|Easy testing|Hard|

---

# 🔐 DI in Spring Security (Real Use)

```java
@Service
public class JwtService {
    private final UserDetailsService service;

    public JwtService(UserDetailsService service) {
        this.service = service;
    }
}
```

---

# 🌍 Real-World DI Examples

|Scenario|Injection Type|
|---|---|
|Database repositories|Constructor|
|Email service|Setter|
|Config values|@Value|
|External API clients|Constructor|

---

# ⚠ Common Interview Traps

### Q: Why constructor injection is preferred?

✔ Immutable  
✔ Thread-safe  
✔ Clear dependencies

---

### Q: Can DI work without Spring?

✔ Yes (Manual DI)

---

### Q: When DI happens?

✔ After object creation

---

### Q: Can static fields be autowired?

❌ No (Container manages instances, not class)





---

## 🔥 Want Next?

✔ DI vs Autowiring  
✔ Circular Dependency problem  
✔ @Lazy usage  
✔ Spring internal DI flow  
✔ DI interview coding problem

Just say **“next”** 🚀