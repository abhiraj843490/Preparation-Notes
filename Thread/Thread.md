
==Synchronous tasks execute one after the other, and each task must complete before the next one can start.== 

==Asynchronous tasks, however, run in parallel;== ==a task can begin before the previous one finishes, making it non-blocking and more efficient for I/O-bound operations like network requests==.

```java

Q: Sum 1 to N number using Executor service, Future and callable
   
import java.util.concurrent.*;

public class SumUsingExecutor {
    public static void main(String[] args) throws Exception {
        int n = 10;
        ExecutorService executor = Executors.newSingleThreadExecutor();
        // Submit the task
        Future<Integer> future = executor.submit(new Callable<Integer>() {
            @Override
            public Integer call() throws Exception {
                int sum = 0;
                for (int i = 1; i <= n; i++) {
                    sum += i;
                }
                return sum;
            }
        });

        // Get the result
        int result = future.get();
        System.out.println("Sum from 1 to " + n + " = " + result);

        executor.shutdown();
    }
}

```

# ✅ **Ways to Achieve Parallelism in Java**

---

# **1️⃣ Using ExecutorService (Fixed Thread Pool) → Most Common**

This is the strongest and most standard way to achieve parallelism.

```java
ExecutorService executor = Executors.newFixedThreadPool(4); // 4 threads

Future<Integer> f1 = executor.submit(() -> compute(1, 25));
Future<Integer> f2 = executor.submit(() -> compute(26, 50));
Future<Integer> f3 = executor.submit(() -> compute(51, 75));
Future<Integer> f4 = executor.submit(() -> compute(76, 100));

int total = f1.get() + f2.get() + f3.get() + f4.get();
executor.shutdown();
```

Utility:

```java
private static int compute(int start, int end) {
    int sum = 0;
    for (int i = start; i <= end; i++) sum += i;
    return sum;
}
```

✔ Runs 4 tasks in parallel  
✔ Each task uses a separate thread  
✔ Best for CPU-bound tasks

---

# **2️⃣ Using `CompletableFuture.supplyAsync()` (Java 8+)**

Best for async programming & parallel pipelines.

```java
CompletableFuture<Integer> f1 = CompletableFuture.supplyAsync(() -> compute(1, 50));
CompletableFuture<Integer> f2 = CompletableFuture.supplyAsync(() -> compute(51, 100));

int result = f1.thenCombine(f2, Integer::sum).join();
```

✔ Automatically uses ForkJoinPool (parallel threads)  
✔ Non-blocking  
✔ Cleaner syntax

---

# **3️⃣ Using `parallelStream()` (Java 8)**

Quick and easiest way — but should be used for large computations.

```java
int sum = IntStream.rangeClosed(1, 100)
                   .parallel()
                   .sum();
```

✔ Divide the workload automatically  
✔ Uses ForkJoinPool internally  
✔ Super efficient for large collections

---

# **4️⃣ Using ForkJoinPool (Low-level parallelism)**

Used for divide-and-conquer tasks.

```java
ForkJoinPool pool = new ForkJoinPool(4);
int sum = pool.invoke(new SumTask(1, 100));
```

✔ Very powerful  
✔ Used by parallel streams internally

---

# 🧠 Interview Tip

> To achieve true parallelism, we must use multiple threads running at the same time. In Java, the most common ways are: ExecutorService with fixed thread pools, CompletableFuture for async pipelines, or parallelStream for parallel computation.


# Question & Answer:

``` java 
class Buffer {
    private int data;
    private boolean hasData = false;

    public synchronized void produce(int value) throws InterruptedException {
        while (hasData) {
            wait();
        }
        data = value;
        hasData = true;
        System.out.println("Produced: " + value);
        notify();
    }

    public synchronized int consume() throws InterruptedException {
        while (!hasData) {
            wait();
        }
        hasData = false;
        System.out.println("Consumed: " + data);
        notify();
        return data;
    }
}

public class ProducerConsumerDemo {
    public static void main(String[] args) {
        Buffer buffer = new Buffer();

        new Thread(() -> {
            for (int i = 1; i <= 5; i++) {
                try { buffer.produce(i); } catch (Exception ignored) {}
            }
        }).start();

        new Thread(() -> {
            for (int i = 1; i <= 5; i++) {
                try { buffer.consume(); } catch (Exception ignored) {}
            }
        }).start();
    }
}
```

# ✅ **1. What is the problem Producer–Consumer is solving?**

**Answer:**  
Producer–consumer solves the problem of **coordination between threads** when one thread is producing data and another thread is consuming it.  
We avoid:

- race conditions
- inconsistent data reads
- busy waiting

---

# ✅ **2. Why did you use synchronized?**

**Answer:**  
`synchronized` ensures **mutual exclusion**, meaning only one thread can access `produce()` or `consume()` at a time on the same `Buffer` object.  
This prevents race conditions on shared variables:

- `data`
- `hasData`

---

# ✅ **3. Why do we use wait() and notify()?**

**Answer:**  
We use them for **thread coordination**:

- `wait()` → thread gives up the lock and waits
- `notify()` → wakes up a waiting thread

Example:

- Producer waits if buffer already has data
- Consumer waits if buffer is empty

This makes them communicate safely.

---

# ✅ **4. Why do we use while instead of if?**

This is a **top interview question**.

**Answer:**  
Because of **spurious wakeups**.  
Even after `wait()` returns, the condition may no longer be valid.  
So we re-check it using `while()`.

If we use `if`, incorrect behavior may occur.

---

# ✅ **5. What happens if we use notifyAll() instead of notify()?**

**Answer:**  
`notify()` wakes **one** waiting thread.  
`notifyAll()` wakes **all** waiting threads.

In Producer–Consumer:

- notify() → enough, because we know exactly which thread will benefit
- notifyAll() → prevents deadlocks in complex systems with multiple producers/consumers

In general:

> notifyAll() is safer but slightly less efficient.

---

# ✅ **6. Can deadlock happen in this code?**

**Answer:**  
Usually no, because:

- Producer waits only when buffer is full
- Consumer waits only when buffer is empty
- notify() wakes the correct thread

But deadlock **can** occur if:

❌ notify is used but wakes the wrong type  
❌ You extend the logic with multiple producers/consumers  
✔ notifyAll() solves this

---

# ✅ **7. What is the difference between wait() and sleep()?**

**Answer:**

|wait()|sleep()|
|---|---|
|Releases lock|Does NOT release lock|
|Used for coordination|Used for time-based pausing|
|Must be inside synchronized block|No such requirement|

---

# ✅ **8. Why is wait() required to be inside synchronized block?**

**Answer:**  
Because wait() needs to release the monitor lock, and this is only possible if the thread **already owns it**.

If not synchronized → IllegalMonitorStateException.

---

# ✅ **9. What is the meaning of this flag variable: `hasData`?**

**Answer:**  
It acts as a **condition flag**:

- `hasData = true` → buffer full
- `hasData = false` → buffer empty

It tells whether producer or consumer should wait.

---

# ✅ **10. What if producer is faster than consumer?**

**Answer:**  
Producer will wait until consumer consumes.  
Buffer ensures no overwriting.

---

# ✅ **11. What if consumer is faster than producer?**

**Answer:**  
Consumer waits until producer produces.

---

# ✅ **12. Can this be optimized using BlockingQueue?**

Yes. This is a very strong answer.

**Answer:**  
"Yes, Java provides `BlockingQueue` (ArrayBlockingQueue / LinkedBlockingQueue) which automatically handles blocking and waking. It removes the need for wait(), notify(), and synchronized."

### Code:

```java
BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(1);

queue.put(10);  // blocks if full
queue.take();   // blocks if empty
```

---

# ✅ **13. Can we get rid of synchronized and use Lock?**

Yes.

**Answer:**  
We can use `ReentrantLock` + `Condition` for better control:

```java
lock.lock();
condition.await();
condition.signal();
```

This avoids intrinsic locks entirely.

---

# ✅ **14. Difference between intrinsic locks and explicit locks?**

|Intrinsic (synchronized)|Explicit Lock (ReentrantLock)|
|---|---|
|Easier|More flexible|
|No tryLock()|Supports tryLock()|
|No fairness|Fair locks available|
|Auto release|Must manually unlock|

---

# 🚀 Want strong interview impact?

Use this final statement:

> Producer-consumer is a classic concurrency problem. I solved it using `wait()` and `notify()` with proper condition checks using `while`. In real projects, I prefer `BlockingQueue` because it simplifies concurrency and avoids manual synchronization bugs.

---

# 🔹 What is `volatile` in Java?

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

If you want next:

- `volatile vs synchronized`
    
- `AtomicInteger vs volatile`
    
- Java Memory Model in simple terms