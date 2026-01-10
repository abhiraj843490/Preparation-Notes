## Features:
``` java
1. lambda expression:  helps us to write our code in functional style. It provides a clear and concise way to implement SAM interface (Single Abstract Method) by using an expression.
   
2. functional interface
   
3. stream api: Java 8 java.util.stream package consists of classes, interfaces and an enum to allow functional-style operations on the elements. It performs lazy computation.
   
4. collectors class: is a final class that accumulate the elements
   
5. default method
   
6. static method in interface
   
7. forEach loop
   
8. Optional class: It is public final class which is used to deal with NullPointerException in Java application.
   
9. **method reference**: Method reference is used to refer method of functional interface   
```



``` java
### Types of Method References
1. Static Method References
2. Instance Method References of a particular object
3. Instance Method References of an Arbitrary Object of a Particular Type
4. Constructor References

```


# Diff b/w map and flatmap

## Map
```
map() is used to transform each element of a stream into another value.

- Each input element produces exactly one output element.
- Output Stream has the same number of elements as the input.
  
  - Purpose: 
map() applies a function to each element of a collection or stream, transforming each element into a single, new element.

- Output: It produces a new collection or stream of the same size as the original, where each element is the result of applying the mapping function to the corresponding original element.
- One-to-one mapping: For every input element, map() generates exactly one output element.
- Example (Java): [[1](https://medium.com/@rajeevmathurinfo/what-is-the-difference-between-flatmap-and-map-in-kotlin-ae1a602de555)]

```

```java
List<String> names = Arrays.asList("Abhi", "Raj", "Kumar");
List<Integer> nameLengths = names.stream()
    .map(String::length)
    .collect(Collectors.toList());

System.out.println(nameLengths); // [4, 3, 5]

List<String> words = Arrays.asList("hello", "world");    
List<Integer> lengths = words.stream()                                .map(String::length).collect(Collectors.toList());   

// lengths will be [5, 5]
```

## FlatMap

```
flatMap() : is used when each element can produce **multiple elements (a stream)**, and you want to **flatten** all those streams into one single stream.

- Each input element produces 0, 1, or many output elements.
- Output Stream may have different size than input.
  
  - Purpose: 
    flatMap() applies a function to each element of a collection or stream, where the function itself returns a new collection or stream. flatMap() then "flattens" these resulting collections/streams into a single, combined collection or stream.
    
- Output: It produces a new collection or stream that contains all the elements from the flattened results of the mapping function. The size of the output collection/stream can be different from the original, as each input element can produce zero or more output elements.
  
- One-to-many mapping and flattening: For every input element, flatMap() can generate zero or more output elements, and it then combines all these generated elements into a single, flat structure.

```

```java
List<List<Integer>> listOfLists = Arrays.asList(
    Arrays.asList(1, 2, 3), Arrays.asList(4, 5), Arrays.asList(6, 7, 8));

List<Integer> flatList = listOfLists.stream()
    .flatMap(List::stream).collect(Collectors.toList());

System.out.println(flatList); // [1, 2, 3, 4, 5, 6, 7, 8]



 List<List<String>> nestedLists = Arrays.asList(Arrays.asList("a", "b"), Arrays.asList("c", "d"));    List<String> flattenedList = nestedLists.stream()                                            .flatMap(Collection::stream).collect(Collectors.toList());   
 
  // flattenedList will be ["a", "b", "c", "d"]

```


The map() and flatMap() methods are both used for transforming data within collections or streams, but they differ in how they handle the results of the transformation, particularly when the mapping function produces another collection or stream.


Key Difference Summarized:

- map()
    performs a one-to-one transformation, where each input element yields one output element.
    
- flatMap()
    performs a one-to-many transformation and then flattens the resulting multiple collections/streams into a single, unified collection/stream.

  
  
### Functional Interface examples:
- Runnable
- Callable
- Comparator
- Predicate
- Function
- Consumer
- Supplier