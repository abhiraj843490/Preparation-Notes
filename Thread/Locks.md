
## Fine-Grained Lock ✅
It improves **concurrency and performance** by allowing multiple threads to work simultaneously on different parts of data.

```java
class Counter {
    private int a = 0;
    private int b = 0;

    private final Object lockA = new Object();
    private final Object lockB = new Object();

    public void incrementA() {
        synchronized (lockA) {
            a++;
        }
    }

    public void incrementB() {
        synchronized (lockB) {
            b++;
        }
    }
}

```

# Why Do We Need It?

If you lock everything:

- Threads wait unnecessarily
- Performance drops
- CPU underutilized

Fine-grained locking reduces contention.

