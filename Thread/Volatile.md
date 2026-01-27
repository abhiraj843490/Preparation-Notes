
> **`volatile` ensures visibility of variable changes across threads.**

When a variable is marked `volatile`, **every thread reads it from main memory, not from its CPU cache**.

---

## 🧠 Problem Without `volatile`

Each thread keeps its own **cached copy** of variables.

```java
class Test {
    static boolean flag = false;

    public static void main(String[] args) {

        new Thread(() -> {
            while (!flag) { }   // may loop forever ❌
            System.out.println("Stopped");
        }).start();

        flag = true;
    }
}
```

👉 Worker thread may never see updated value → infinite loop.

---

## ✅ With `volatile`

```java
static volatile boolean flag = false;
```

Now:

- Update by one thread
    
- Immediately visible to others

---

# 🔥 What `volatile` Guarantees

|Feature|Supported?|
|---|---|
|Visibility|✅ Yes|
|Atomicity|❌ No|
|Prevents reordering|✅ Yes|

---

## 🔴 Very Important: `volatile` ≠ thread safety

```java
volatile int count = 0;

count++;  // NOT atomic ❌
```

Because `count++` = 3 steps:

1. Read
    
2. Increment
    
3. Write

Multiple threads can corrupt value.

✔ Use:

```java
AtomicInteger
synchronized
Lock
```

---

# 🔹 When to Use `volatile`

✔ Flags  
✔ Status variables  
✔ Stop signals

```java
volatile boolean running = true;
```

---

# 🔹 When NOT to Use

❌ Counters  
❌ Banking operations  
❌ Shared mutable data

---

# 🔥 Real Interview Example

```java
class Worker extends Thread {
    private volatile boolean running = true;

    public void run() {
        while (running) {
            // work
        }
    }

    public void stopWorker() {
        running = false;
    }
}
```

---

# 🧠 Interview One-Liner

> `volatile` ensures visibility of changes to a variable across threads and prevents instruction reordering, but it does not guarantee atomicity.

---
