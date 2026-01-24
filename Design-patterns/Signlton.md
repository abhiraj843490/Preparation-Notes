```java
// LazySingleton
public class Singleton {
    private static Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

```
🟢 **Pros:**
- Instance created only when needed (lazy).
🔴 **Cons:**
- **Not thread-safe** — multiple threads may create multiple instances.

```java 
public class ThreadSafeSingleton {
    private static ThreadSafeSingleton instance;
    private ThreadSafeSingleton() {}
    public static synchronized ThreadSafeSingleton getInstance() {
        if (instance == null) {
            instance = new ThreadSafeSingleton();
        }
        return instance;
    }
}
```
🟢 **Pros:**
- Thread-safe.
🔴 **Cons:**
- Synchronization makes it **slow** for every call (unnecessary after instance created).

**More optimized:**
```java
public class DoubleCheckedSingleton {
    private static volatile DoubleCheckedSingleton instance;
    private DoubleCheckedSingleton() {}
    public static DoubleCheckedSingleton getInstance() {
        if (instance == null) {
            synchronized (DoubleCheckedSingleton.class) {
                if (instance == null) {
                    instance = new DoubleCheckedSingleton();
                }
            }
        }
        return instance;
    }
}
```
🟢 **Pros:**
- Thread-safe + lazy.
- Synchronization used only at first initialization.
- `volatile` ensures visibility across threads.

🔴 **Cons:**
- Slightly more complex to understand.
### **6️⃣ Bill Pugh Singleton (Best Practice in Modern Java)**

```java
public class BillPughSingleton {
    private BillPughSingleton() {}
    private static class SingletonHelper {
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }
    public static BillPughSingleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```
🟢 **Pros:**
- Lazy loading.
- Thread-safe (class loading mechanism ensures that).
- No synchronization overhead.
    
🔴 **Cons:**
- None (considered **best approach** for standard cases).


7️⃣ Enum Singleton (Most Robust and Serialization Safe)
```java
public enum EnumSingleton {
    INSTANCE;
    public void showMessage() {
        System.out.println("Hello from Enum Singleton!");
    }
}
```

🟢 Pros:

Thread-safe, serialization-safe, and protection from reflection.
Simplest and most robust approach.

🔴 Cons:

Enum can’t extend other classes.
Some developers find it less flexible or "non-traditional."