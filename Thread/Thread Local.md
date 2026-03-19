
`ThreadLocal` is used to define the thread scope: 
Yes ✅ **`ThreadLocal` is used to create thread-specific (thread-scoped) variables.**

It means:

> Each thread gets **its own independent copy** of the variable.

So even if multiple threads access the same `ThreadLocal` variable, **they don’t interfere with each other**.

---

# 🧠 Why `ThreadLocal` is Used

In multithreading sometimes we want:

- Same variable name
    
- But **separate value per thread**

Example use cases:

- User session data
    
- Request context
    
- Database connection per thread
    
- Date formatter (since `SimpleDateFormat` is not thread safe)

---

# ⚡ Simple Example

```java
class ThreadLocalExample {

    private static ThreadLocal<Integer> threadLocal = new ThreadLocal<>();

    public static void main(String[] args) {

        Thread t1 = new Thread(() -> {
            threadLocal.set(100);
            System.out.println("Thread1 value: " + threadLocal.get());
        });

        Thread t2 = new Thread(() -> {
            threadLocal.set(200);
            System.out.println("Thread2 value: " + threadLocal.get());
        });

        t1.start();
        t2.start();
    }
}
```

### Output (order may vary)

```
Thread1 value: 100
Thread2 value: 200
```

✔ Both threads used the **same variable**  
✔ But values are **different**

---

#  Without `ThreadLocal` (Race Condition Risk)

```java
static int value;
```

Threads may overwrite each other.

---

# ⚡ Real Interview Example (Spring Boot Request Context)

```java
class UserContext {

    private static ThreadLocal<String> currentUser = new ThreadLocal<>();

    public static void setUser(String user) {
        currentUser.set(user);
    }

    public static String getUser() {
        return currentUser.get();
    }

    public static void clear() {
        currentUser.remove();
    }
}
```

Usage:

```java
Thread t1 = new Thread(() -> {
    UserContext.setUser("Alice");
    System.out.println(UserContext.getUser());
});

Thread t2 = new Thread(() -> {
    UserContext.setUser("Bob");
    System.out.println(UserContext.getUser());
});
```

Output:

```
Alice
Bob
```

Each thread has **its own user context**.

---

# ⚠️ Important Interview Point

Always **remove ThreadLocal value** after use:

```java
threadLocal.remove();
```

Why?

Because **ThreadPool threads are reused** → can cause **memory leaks**.

---

# Internally How ThreadLocal Works

Each `Thread` has:

```
Thread
   ↓
ThreadLocalMap
   ↓
ThreadLocal → Value
```

So data is stored **inside the Thread**, not in the `ThreadLocal` object.

---

# One-Line Answer

> `ThreadLocal` provides thread-confined variables where each thread has its own independent copy of the variable.

