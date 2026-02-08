
Spring beans describes the java object 
- It is a fundamental building blocks of a spring app and are created, configured and managed by the spring container.
    
- @Component,  @Service,  @Repository, and @Controller to mark classes as beans. 

Bean Scope:
- Defines the lifecycle of a bean, i.e., how many instances of the bean will be created and how they will be shared:
1. Singleton: One instance per Spring IoC container (default).
2. Prototype: A new instance each time a bean is requested.
3. Request: One instance per HTTP request (web applications).
4. Session: One instance per HTTP session (web applications).
5. GlobalSession: One instance per global HTTP session (web applications).
6. Application: One instance per Spring ApplicationContext.

Life cycle:
1. constructor call (init)
2. dependency injection
3. post construct
4. bean ready to use
5. destroy


# 🌱 Spring Bean Life Cycle

---

## What is a Spring Bean?

A **Spring Bean** is an object that is:

- Created
- Managed
- Configured
- Destroyed  
    by the **Spring IoC Container**

---
## Interview-Ready Answer

> “Spring bean lifecycle includes instantiation, dependency injection, awareness callbacks, initialization using @PostConstruct or InitializingBean, post-processing via BeanPostProcessor, usage, and destruction via @PreDestroy or DisposableBean.”

---
## 🔁 High-Level Bean Life Cycle Flow

```
1. Bean Instantiation (from constructor call)
2. Populate Properties (Dependency Injection)
3. Aware Interfaces
4. BeanPostProcessor (Before Init)
5. InitializingBean / @PostConstruct
6. BeanPostProcessor (After Init)
7. Bean Ready for Use
8. Destruction (@PreDestroy / DisposableBean)
```

---

## 1️⃣ Bean Instantiation

### What happens?

Spring creates the bean object using:

- Constructor
- Factory method

### Example:

```java
@Component
public class UserService {
    public UserService() {
        System.out.println("1. Bean Instantiated");
    }
}
```

---

## 2️⃣ Dependency Injection (Populate Properties)

### What?

Spring injects dependencies via:

- Constructor Injection
    
- Setter Injection
    
- Field Injection
    

```java
@Autowired
private UserRepository userRepo;
```

---

## 3️⃣ Aware Interfaces (Optional but Powerful)

If bean implements **Aware interfaces**, Spring injects internal objects.

|Interface|Purpose|
|---|---|
|BeanNameAware|Get bean name|
|BeanFactoryAware|Access BeanFactory|
|ApplicationContextAware|Access context|

```java
public class MyBean implements BeanNameAware {
    public void setBeanName(String name) {
        System.out.println("Bean name: " + name);
    }
}
```

---

## 4️⃣ BeanPostProcessor – Before Initialization

### What?

Allows modification of beans **before initialization**.

```java
postProcessBeforeInitialization(Object bean, String beanName)
```

👉 Used in:

- Spring AOP
    
- Proxy creation
    
- Security
    

---

## 5️⃣ Initialization Phase

Spring offers **3 ways**:

### a) @PostConstruct (Recommended)

```java
@PostConstruct
public void init() {
    System.out.println("Initializing Bean");
}
```

### b) InitializingBean Interface

```java
public void afterPropertiesSet() {
    System.out.println("After properties set");
}
```

### c) Custom init-method

```java
@Bean(initMethod = "customInit")
```

---

## 6️⃣ BeanPostProcessor – After Initialization

```java
postProcessAfterInitialization(Object bean, String beanName)
```

Used to:

- Wrap beans with proxies
    
- Add cross-cutting logic
    

---

## 7️⃣ Bean is Ready for Use 🎯

Now Spring injects this bean wherever needed.

---

## 8️⃣ Bean Destruction Phase

Occurs when:

- Context is closed
    
- Application stops
    

### Destruction Methods:

|Method|Example|
|---|---|
|@PreDestroy|Recommended|
|DisposableBean|destroy()|
|destroy-method|XML / @Bean|

```java
@PreDestroy
public void cleanup() {
    System.out.println("Cleaning resources");
}
```

---

## 🔄 Complete Lifecycle Example

```java
@Component
public class LifeCycleBean {

    public LifeCycleBean() {
        System.out.println("1. Constructor");
    }

    @Autowired
    private UserService service;

    @PostConstruct
    public void init() {
        System.out.println("2. PostConstruct");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("3. PreDestroy");
    }
}
```

---

## 🧠 Interview Trick Question

### Q: Order between @PostConstruct and BeanPostProcessor?

✔ `postProcessBeforeInitialization`  
✔ `@PostConstruct`  
✔ `afterPropertiesSet`  
✔ `postProcessAfterInitialization`

---

## 🧪 Bean Scopes Impact on Lifecycle

|Scope|Lifecycle|
|---|---|
|Singleton|Created once, destroyed at shutdown|
|Prototype|Created each time, not destroyed by Spring|
|Request|Per HTTP request|
|Session|Per HTTP session|

⚠ Prototype beans don’t call destroy methods.

---

## 🔥 Real-Time Use Cases

|Scenario|Hook Used|
|---|---|
|DB connection setup|@PostConstruct|
|Resource cleanup|@PreDestroy|
|Logging proxies|BeanPostProcessor|
|Security filters|After Init|


---

## ❓ Frequently Asked Interview Questions

✔ Difference between @PostConstruct and init-method  
✔ BeanPostProcessor vs BeanFactoryPostProcessor  
✔ Why prototype beans aren’t destroyed  
✔ How AOP works using lifecycle  
✔ When destroy method is called
