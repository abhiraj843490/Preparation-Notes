
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