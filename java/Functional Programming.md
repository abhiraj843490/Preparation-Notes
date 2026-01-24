
==Functional programming is a style of programming where computation is treated as the evaluation of pure functions without changing state or data. 
It focuses on immutability, declarative code, and higher-order functions.   In Java, this is achieved using lambda expressions, streams, and functional interfaces introduced in Java 8.

Functional Programming = “Programming using functions as first-class citizens.”

---
## 2️⃣ Key Idea

Instead of **how to do** (imperative style), you focus on **what to do** (declarative style).

| Imperative (OOP)              | Functional                                  |
| ----------------------------- | ------------------------------------------- |
| Step-by-step instructions     | Describe transformations                    |
| Focus on _state and behavior_ | Focus on _data and functions_               |
| Uses loops and conditionals   | Uses higher-order functions and expressions |
| Mutates data                  | Uses immutable data                         |

---

## ⚙️ 3️⃣ Core Principles of Functional Programming

|Principle|Description|Example|
|---|---|---|
|**Pure Functions**|Same input → same output, no side effects|`x -> x * 2`|
|**Immutability**|Data doesn’t change once created|Use `final` or immutable objects|
|**First-class & Higher-order functions**|Functions can be passed, returned, or stored|`map()`, `filter()`, `reduce()`|
|**Declarative style**|Focus on _what to achieve_|`list.stream().filter(x -> x > 10)`|
|**No side effects**|Avoid modifying external state|Don’t modify global vars or I/O inside functions|
|**Recursion over loops**|Loops replaced by recursion or stream APIs|`reduce()` instead of `for` loop|



## ⚙️ 5️⃣ Example — Traditional vs Functional
```java
🧱 Traditional (Imperative)

List<Integer> numbers = Arrays.asList(1,2,3,4,5);
List<Integer> evenNumbers = new ArrayList<>();

for (Integer n : numbers) {
    if (n % 2 == 0) {
        evenNumbers.add(n);
    }
}
System.out.println(evenNumbers);


🚀 Functional (Declarative)

List<Integer> numbers = Arrays.asList(1,2,3,4,5);
List<Integer> evenNumbers = numbers.stream()
                                   .filter(n -> n % 2 == 0)
                                   .toList();
System.out.println(evenNumbers);

```


## 🧩 6️⃣ Functional Interfaces

A Functional Interface is an interface with exactly **one abstract method**.

Examples:

- `Predicate<T>` → returns boolean
- `Function<T,R>` → takes input, returns output
- `Consumer<T>` → takes input, returns nothing
- `Supplier<T>` → returns a value, takes nothing
    

Custom example:

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}
```

You can use it like:

```java
Calculator add = (a, b) -> a + b;
System.out.println(add.calculate(5, 10));  // 15
```

---

## 🧠 7️⃣ Pure Function Example

```java
// Pure function
int square(int x) {
    return x * x;
}

// Impure function (side-effect)
int total = 0;
int addToTotal(int x) {
    total += x; // modifies external state ❌
    return total;
}
```

## ⚙️ 8️⃣ Immutability Example

```java 
List<String> names = List.of("Abhi", "Raj", "Sam"); // immutable list
names.add("New"); // ❌ UnsupportedOperationException
```

Instead, create a **new list**:

```java
List<String> updated = Stream.concat(names.stream(), Stream.of("New")).toList();
```

---

## 💡 9️⃣ Higher-Order Functions

A **Higher-Order Function** is a function that:

- Takes another function as an argument, or
    
- Returns a function.

Example:

```java
Function<Integer, Integer> square = x -> x * x;
Function<Integer, Integer> doubleIt = x -> x * 2;

// compose two functions
Function<Integer, Integer> squareThenDouble = square.andThen(doubleIt);
System.out.println(squareThenDouble.apply(3)); // (3*3)=9 -> (9*2)=18
```

---

## 🧩 🔟 Advantages of Functional Programming

|Advantage|Description|
|---|---|
|✅ Cleaner, concise code|Fewer lines, easy to read|
|✅ Easier parallelization|No mutable shared state|
|✅ Better for concurrent programming|Functions are independent|
|✅ Easier testing|Pure functions are predictable|
|✅ Encourages immutability|Reduces bugs and side-effects|

---
## ⚠️ 11️⃣ When to Avoid FP

- When you need **complex mutable state** (e.g., game loops, simulations)
    
- When performance suffers due to **object creation in streams**
    
- When debugging complex stream chains becomes hard

---

## 🚀 13️⃣ Quick Comparison Summary

|Concept|OOP (Imperative)|FP (Declarative)|
|---|---|---|
|Focus|Objects and state|Functions and data transformation|
|State|Mutable|Immutable|
|Control Flow|Loops, conditionals|Map, filter, reduce|
|Reusability|Inheritance|Composition|
|Example|`for` loop|`stream().map()`|
```java
import java.util.*;
import java.util.stream.*;

public class ThirdLargest {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(5, 2, 9, 1, 7, 3, 9);

        Integer thirdLargest = list.stream()
            .distinct()                  // remove duplicates if needed
            .sorted(Comparator.reverseOrder()) // sort in descending order
            .skip(2)                     // skip first two (largest and 2nd largest)
            .findFirst()                 // get the 3rd element
            .orElse(null);               // handle empty case

        System.out.println("3rd Largest: " + thirdLargest);
    }
}
```



