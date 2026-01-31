

## Runnable Interface

```java
class MyRunnable implements Runnable {

    @Override
    public void run() {
        System.out.println("Runnable Task running by: " +
                Thread.currentThread().getName());
    }
}

public class RunnableExample {

    public static void main(String[] args) {

        Thread t1 = new Thread(new MyRunnable());
        Thread t2 = new Thread(new MyRunnable());

        t1.start();
        t2.start();

        System.out.println("Main thread: " +
                Thread.currentThread().getName());
    }
}

```

## Callable Interface
```java 
import java.util.concurrent.*;

class MyCallable implements Callable<Integer> {

    @Override
    public Integer call() throws Exception {
        System.out.println("Callable running by: " +
                Thread.currentThread().getName());
        return 10 + 20;
    }
}

public class CallableExample {

    public static void main(String[] args) throws Exception {

        ExecutorService executor = Executors.newFixedThreadPool(2);
        Future<Integer> future = executor.submit(new MyCallable());

        // Getting result (blocks until done)
        Integer result = future.get();
        System.out.println("Result from Callable: " + result);

        executor.shutdown();
    }
}

```


|Feature|Runnable|Callable|
|---|---|---|
|Method|`run()`|`call()`|
|Return value|❌ No|✅ Yes|
|Exception handling|Cannot throw checked exception|Can throw checked exception|
|Execution|Thread|ExecutorService|
|Result holder|❌|`Future`|