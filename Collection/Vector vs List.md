| Feature       | Vector                         | ArrayList            |
| ------------- | ------------------------------ | -------------------- |
| Thread Safety | **Synchronized** (thread-safe) | **Not synchronized** |
| Performance   | Slower due to synchronization  | Faster               |
| Legacy        | Legacy class                   | Modern collection    |
| Package       | `java.util`                    | `java.util`          |
| Introduced    | Java 1.0                       | Java 1.2             |
### Vector

All methods are **synchronized**, so multiple threads can access it safely.
```java
Vector<Integer> v = new Vector<>();  
v.add(10);  
v.add(20);
```
Internally:

``` java 
public synchronized boolean add(E e)

```

Because of synchronization, **every operation acquires a lock**.

### Array List

Not thread-safe.
```java
ArrayList<Integer> list = new ArrayList<>();  
list.add(10);  
list.add(20);
```

Multiple threads can cause **race conditions**.

If you want thread-safe list:
```java
List<Integer> list = Collections.synchronizedList(new ArrayList<>());
```


