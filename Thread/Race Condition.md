A **race condition** in Java ==occurs when multiple threads access and modify the same shared data concurrently==, and the outcome depends on the unpredictable timing or order of their execution. This can lead to -
1. Data corruption 
2. Inconsistent results
3. Other unpredictable program behaviors.

```java
class Counter {
    int count = 0;

    public void increment() {
        count++;   // NOT atomic
    }
}
```

```java
Counter counter = new Counter();

Thread t1 = new Thread(() -> {
    for(int i=0;i<1000;i++) counter.increment();
});

Thread t2 = new Thread(() -> {
    for(int i=0;i<1000;i++) counter.increment();
});

t1.start();
t2.start();

t1.join();
t2.join();

System.out.println(counter.count);
```

## Ways to solve this Problems:
1️⃣ `synchronized`
```java
public synchronized void increment() {
    count++;
}
```
or,
```java
public void increment() {
    synchronized(this) {
        count++;
    }
}
```
2️⃣ `volatile`

```java
volatile int count;
```

3️⃣ `Atomic`
```java
import java.util.concurrent.atomic.AtomicInteger;

AtomicInteger count = new AtomicInteger(0);

count.incrementAndGet();
```

4️⃣ `Lock` Interface
```java
import java.util.concurrent.locks.ReentrantLock;

ReentrantLock lock = new ReentrantLock();

public void increment() {
    lock.lock();
    try {
        count++;
    } finally {
        lock.unlock();
    }
}
```
5️⃣ Immutable Objects
```java
final class User {
    private final int id;
}
```

6️⃣ Thread-safe Collections
```java
ConcurrentHashMap map = new ConcurrentHashMap<>();
```
