
How completable Feature is working and how will make it async and having multiple apis calls inside the completable feature then how they are going to wait for each other and combined the response to get the final response and what will happen when any api got failed in completable feature and how will fix it?

Completable Future in java provides a way to write asynchronous, non-blocking code. It allows you to run tasks in the background, chain tem combine results, and handle errors.


## How it works:

1. you create a completable future for each async task
2. chain tasks using methods like ``thenApply, thenCompose``, or combine multiple futures with ``allOf or anyOf``.
3. when all tasks complete you can combine their results.

## Making API calls async:
1. use ``CompletableFuture.supplyAsync(()->apiCall())`` for each api
2. combine them using ``CompletableFuture.allOf(f1, f2, ..)``
3. After all complete, extract results with ``f1.get()``

## waiting and combining responses:
1. ``completableFuture.allOf(f1 . .)`` waits for all to finish
2. After that you can combine their results as needed.


## Error handling
1. if any api call fails, the combined future will also complete exceptionally
2. you can handle errors using .execeptionally() or .handle()

```java

CompletableFuture<Res1> f1 = CompletableFuture.supplyAsync(()->callApi1())
CompletableFuture<Res2> f2 = CompletableFuture.supplyAsync(()->callApi2())
CompletableFuture<Res3> f3 = CompletableFuture.supplyAsync(()->callApi3())

CompletableFuture<Void> allf = CompletableFuture.allOf(f1,f2, f3)

CompletableFuture<CombineRes> combined = allFuture.thenApply(v->{
	Res1 r1 = f1.join();
	Res2 r2 = f2.join();
	return combine(r1, r2);
}).exceptionally(ex ->{
	return handleError(ex);
});
```


```java
import java.util.concurrent.*;

public class ApiAggregator {

    private static final ExecutorService executor =
            Executors.newFixedThreadPool(10); // custom thread pool

    public static void main(String[] args) {

        CompletableFuture<Res1> f1 =
                CompletableFuture.supplyAsync(() -> callApi1(), executor);

        CompletableFuture<Res2> f2 =
                CompletableFuture.supplyAsync(() -> callApi2(), executor);

        CompletableFuture<Res3> f3 =
                CompletableFuture.supplyAsync(() -> callApi3(), executor);

        CompletableFuture<CombineRes> combined =
                CompletableFuture.allOf(f1, f2, f3)
                        .thenApplyAsync(v ->
                                combine(f1.join(), f2.join(), f3.join()),
                                executor
                        )
                        .exceptionally(ex -> handleError(ex));

        // If you need final result (like in main thread)
        CombineRes result = combined.join();
        System.out.println(result);

        executor.shutdown();
    }

    // ---------- Mock Methods ----------

    static Res1 callApi1() {
        sleep(1000);
        return new Res1("Data1");
    }

    static Res2 callApi2() {
        sleep(1200);
        return new Res2("Data2");
    }

    static Res3 callApi3() {
        sleep(800);
        return new Res3("Data3");
    }

    static CombineRes combine(Res1 r1, Res2 r2, Res3 r3) {
        return new CombineRes(r1.value + " " + r2.value + " " + r3.value);
    }

    static CombineRes handleError(Throwable ex) {
        System.out.println("Error occurred: " + ex.getMessage());
        return new CombineRes("Fallback Response");
    }

    static void sleep(long ms) {
        try { Thread.sleep(ms); } catch (InterruptedException ignored) {}
    }

    // ---------- DTOs ----------
    static class Res1 { String value; Res1(String v){ value=v; } }
    static class Res2 { String value; Res2(String v){ value=v; } }
    static class Res3 { String value; Res3(String v){ value=v; } }
    static class CombineRes {
        String data;
        CombineRes(String d){ data=d; }
        public String toString(){ return data; }
    }
}

```