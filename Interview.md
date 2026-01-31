
Q. What is a HashMap in Java, and how does it function internally?

Ans. 

HashMap in Java is a data structure that stores key-value pairs and uses hashing to efficiently retrieve values.

- HashMap is part of the Java Collections framework.
    
- It allows null keys and values.
    
- Internally, it uses an array of linked lists to handle collisions.
    
- The key's hash code is used to determine the index in the array where the key-value pair is stored.
    
- Example: HashMap<String, Integer> map = new HashMap<>(); map.put("key", 123); int value = map.get("key");
    


A Java Developer was asked12mo ago

Q. Implement a custom stack that allows fetching the minimum element in constant time, O(1).

Ans. 

Custom stack with constant time access to minimum element

- Use two stacks - one to store elements and another to store minimum values
    
- When pushing an element, compare with top of min stack and push the smaller value
    
- When popping an element, pop from both stacks
    
- Access minimum element in O(1) time by peeking at top of min stack
    

Answered by AI

Add your answer

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

### Java Developer Interview Questions Asked at Other Companies

asked in [TCS](https://www.ambitionbox.com/interviews/tcs-interview-questions/java-developer "TCS Java Developer Interview Questions")

Q1. What is OOPS in Java?

[View answers (22)![](https://static.ambitionbox.com/static/blue_arrow_right.svg)](https://www.ambitionbox.com/interviews/question/what-is-oops-in-java-bktlHshKe?expandQuestion=true "View answers (22)")

asked in [Infosys](https://www.ambitionbox.com/interviews/infosys-interview-questions/java-developer "Infosys Java Developer Interview Questions")

Q2. Write code to filter out loans with an incomplete status using Ja ... read more

[View answers (6)![](https://static.ambitionbox.com/static/blue_arrow_right.svg)](https://www.ambitionbox.com/interviews/question/write-code-to-filter-out-loans-with-an-incomplete-status-using-java-8-features-bjUaQRmko?expandQuestion=true "View answers (6)")

asked in [TCS](https://www.ambitionbox.com/interviews/tcs-interview-questions/java-developer "TCS Java Developer Interview Questions")

Q3. What is the use of private variables if they are accessible via g ... read more

[View answer (1)![](https://static.ambitionbox.com/static/blue_arrow_right.svg)](https://www.ambitionbox.com/interviews/question/what-is-the-use-of-private-variables-if-they-are-accessible-via-getters-and-setters-from-another-class-bjCF1uiWs?expandQuestion=true "View answer (1)")

asked in [Capgemini](https://www.ambitionbox.com/interviews/capgemini-interview-questions/java-developer "Capgemini Java Developer Interview Questions")

Q4. How does Java achieve platform independence?

[View answers (12)![](https://static.ambitionbox.com/static/blue_arrow_right.svg)](https://www.ambitionbox.com/interviews/question/how-does-java-achieve-platform-independence-bpwpbGMzQ?expandQuestion=true "View answers (12)")

asked in [Accenture](https://www.ambitionbox.com/interviews/accenture-interview-questions/java-developer "Accenture Java Developer Interview Questions")

Q5. Explain the core principles of object-oriented programming (OOP) ... read more

[View answers (4)![](https://static.ambitionbox.com/static/blue_arrow_right.svg)](https://www.ambitionbox.com/interviews/question/explain-the-core-principles-of-object-oriented-programming-oop-in-java-and-how-they-contribute-to-building-robust-and-maintainable-applications-bo3g3dHSK?expandQuestion=true "View answers (4)")

[View All](https://www.ambitionbox.com/profiles/java-developer/interview-questions "Java Developer Interview Questions")

A Java Developer was asked12mo ago

Q. How can one create an immutable class named Employee with fields for name and hobbies defined as List?

Ans. 

To create an immutable class named Employee with fields for name and hobbies defined as List<String>, use private final fields and return new instances in getters.

- Use private final fields for name and hobbies in the Employee class
    
- Provide a constructor to initialize the fields
    
- Return new instances or unmodifiable lists in the getters to ensure immutability
    

Answered by AI

Add your answer

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

Rate your company

![star](https://employer.ambitionbox.com/static/assets/images/star.svg)![star](https://employer.ambitionbox.com/static/assets/images/star.svg)![star](https://employer.ambitionbox.com/static/assets/images/star.svg)![star](https://employer.ambitionbox.com/static/assets/images/star.svg)![star](https://employer.ambitionbox.com/static/assets/images/star.svg)

🤫 100% anonymous

How was your last interview experience?

Share interview![](https://static.ambitionbox.com/alpha/community/assets/arrow-forward-blue.svg)

A Java Developer was asked12mo ago

Q. What changes are required in the Employee class to use it as a key for a HashMap?

Ans. 

Add hashCode() and equals() methods to Employee class.

- Override hashCode() method to generate a unique hash code for each Employee object.
    
- Override equals() method to compare Employee objects based on their attributes.
    
- Ensure that the attributes used in hashCode() and equals() methods are immutable.
    
- Example: Add id, name, and department attributes to Employee class and override hashCode() and equals() methods based on these attributes.
    

Answered by AI

Add your answer

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

A Java Developer was asked12mo ago

Q. What is the implementation of the singleton design pattern?

Ans. 

Singleton design pattern ensures a class has only one instance and provides a global point of access to it.

- Create a private static instance of the class within the class itself.
    
- Provide a public static method to access the instance, creating it if necessary.
    
- Ensure the constructor of the class is private to prevent instantiation from outside the class.
    
- Use lazy initialization to create the instance only when needed.
    
- Thread safety should be considered when implementing the singleton pattern.
    

Answered by AI

Add your answer

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

## What are the roles & responsibilities of a Java Developer at Wissen Technology?

Application Development

- Design, develop, and maintain Java-based applications
- Analyze user requirements to define business objectives

Read full roles & responsibilities![](https://static.ambitionbox.com/static/chevron_down_blue.svg)

A Java Developer was asked12mo ago

Q. What are the SOLID principles in software design, and can you provide examples for each?

Ans. 

SOLID principles are a set of five design principles in object-oriented programming to make software more maintainable, flexible, and scalable.

- Single Responsibility Principle (SRP) - A class should have only one reason to change. Example: A class that handles user authentication should not also handle database operations.
    
- Open/Closed Principle (OCP) - Software entities should be open for extension but closed for modification. Example: Using interfaces to allow for adding new functionality without modifying existing code.
    
- Liskov Substitution Principle (LSP) - Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program. Example: Using polymorphism to substitute different implementations of an interface.
    
- Interface Segregation Principle (ISP) - A client should not be forced to implement interfaces they do not use. Example: Breaking down large interfaces into smaller, more specific ones.
    
- Dependency Inversion Principle (DIP) - High-level modules should not depend on low-level modules. Both should depend on abstractions. Example: Using dependency injection to decouple classes and improve testability.
    

Answered by AI

Add your answer

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

### Wissen Technology HR Interview Questions

16 questions and answers

![](https://www.ambitionbox.com/static/HrWidgetImg.svg)

Q. Can you provide an introduction about yourself, including your current plac ... read more

Q. What is your biggest achievement?

Q. Could you please share some personal details about yourself?

[View all HR interview questions](https://www.ambitionbox.com/skills/hr-interview-questions "HR Interview Questions")

A Java Developer was asked12mo ago

Q. What is the concept of Object-Oriented Programming (OOP) and can you provide an example?

Ans. 

OOP is a programming paradigm based on the concept of objects, which can contain data in the form of fields and code in the form of procedures.

- OOP focuses on creating objects that interact with each other to...read more
    

Add your answer

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

[1](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer)[2](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer?page=2)[3](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer?page=3)[4](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer?page=4)[5](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer?page=5)[6](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer?page=6)[7](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer?page=7)

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer?page=2)

## Wissen Technology Java Developer Interview Experiences

### 29 interviews found

## [Java Developer Interview Questions & Answers](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer "Wissen Technology Java Developer Interview Questions & Answers")

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions "interview questions")![user image](https://static.ambitionbox.com/static/icons/answered_by_anonymous.png "Anonymous")Anonymous

posted on 23 Jan 2026

Interview experience

1

Bad

Difficulty level

Hard

Process Duration

2-4 weeks

Result

No response

I appeared for an interview in Jul 2025, where I was asked the following questions.

- Q1. Multiple Java coding questions to be wrote on a piece of paper

- Q2. Lexicographic coding problem

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote-blue.svg)![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

Are these interview questions helpful?

![g-chip__img](https://static.ambitionbox.com/static/interestedInWorking/yes.svg)Yes![g-chip__img](https://static.ambitionbox.com/static/interestedInWorking/no.svg)No

## [Java Developer Interview Questions & Answers](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer "Wissen Technology Java Developer Interview Questions & Answers")

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions "interview questions")![user image](https://static.ambitionbox.com/static/icons/answered_by_anonymous.png "Anonymous")Anonymous

posted on 12 Jan 2026

Interview experience

1

Bad

Difficulty level

Moderate

Process Duration

Less than 2 weeks

Result

Not Selected

I appeared for an interview in Dec 2025, where I was asked the following questions.

- Q1. Shared a laptop and asked to complete a spring boot project where we have to read from CSV and map to pojo and store in h2 and complete the service and test class need to be pass
- Ans. 
    
    Create a Spring Boot application to read CSV data, map it to POJOs, store in H2, and implement service and test classes.
    
    - Set up a Spring Boot project using Spring Initializr with dependencies: Spring Web, Spring Data JPA, H2 Database, and OpenCSV.
        
    - Create a POJO class that represents the structure of the CSV data, e.g., `User` class with fields like `id`, `name`, and `email`.
        
    - Use OpenCSV to read the CSV file and map each row to the POJO using a `CsvToBean` instance.
        
    - Configure H2 database in `application.properties` for in-memory storage: `spring.h2.console.enabled=true`.
        
    - Create a JPA repository interface for the POJO to handle CRUD operations, e.g., `UserRepository extends JpaRepository<User, Long>`.
        
    - Implement a service class to handle business logic, e.g., `UserService` with methods to save users from CSV.
        
    - Write unit tests using JUnit and Mockito to ensure the service methods work correctly, e.g., testing the CSV reading and saving functionality.
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q2. Round 1 is a hacker rank test

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote-blue.svg)![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

## [Java Developer Interview Questions & Answers](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer "Wissen Technology Java Developer Interview Questions & Answers")

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions "interview questions")![user image](https://static.ambitionbox.com/static/icons/answered_by_anonymous.png "Anonymous")Anonymous

posted on 21 Jan 2025

Interview experience

3

Average

Difficulty level

Moderate

Process Duration

Less than 2 weeks

Result

No response

I appeared for an interview in Dec 2024.

Technical - 1Technical - 2

Round 1 - Technical 

### (6 Questions)

- Q1. Implement a custom stack that allows fetching the minimum element in constant time, O(1).
- Ans. 
    
    Custom stack with constant time access to minimum element
    
    - Use two stacks - one to store elements and another to store minimum values
        
    - When pushing an element, compare with top of min stack and push the smaller value
        
    - When popping an element, pop from both stacks
        
    - Access minimum element in O(1) time by peeking at top of min stack
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q2. How can one create an immutable class named Employee with fields for name and hobbies defined as List?
- Ans. 
    
    To create an immutable class named Employee with fields for name and hobbies defined as List<String>, use private final fields and return new instances in getters.
    
    - Use private final fields for name and hobbies in the Employee class
        
    - Provide a constructor to initialize the fields
        
    - Return new instances or unmodifiable lists in the getters to ensure immutability
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q3. What changes are required in the Employee class to use it as a key for a HashMap?
- Ans. 
    
    Add hashCode() and equals() methods to Employee class.
    
    - Override hashCode() method to generate a unique hash code for each Employee object.
        
    - Override equals() method to compare Employee objects based on their attributes.
        
    - Ensure that the attributes used in hashCode() and equals() methods are immutable.
        
    - Example: Add id, name, and department attributes to Employee class and override hashCode() and equals() methods based on these attributes.
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q4. What is the implementation of the singleton design pattern?
- Ans. 
    
    Singleton design pattern ensures a class has only one instance and provides a global point of access to it.
    
    - Create a private static instance of the class within th...read more
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q5. Different ways to create a thread
- Ans. 
    
    Ways to create a thread in Java include extending the Thread class, implementing the Runnable interface, and using Java 8's lambda expressions.
    
    - Extend the Thread c...read more
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q6. What are the SOLID principles in software design, and can you provide examples for each?
- Ans. 
    
    SOLID principles are a set of five design principles in object-oriented programming to make software more maintainable, flexible, and scalable.
    
    - Single Responsibili...read more
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

Round 2 - Technical 

### (5 Questions)

- Q1. What is the concept of Object-Oriented Programming (OOP) and can you provide an example?
- Ans. 
    
    OOP is a programming paradigm based on the concept of objects, which can contain data in the form of fields and code in the form of procedures.
    
    - OOP focuses on crea...read more
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q2. How do threads communicate with each other in Java?
- Ans. 
    
    Threads communicate in Java through shared memory or message passing.
    
    - Threads can communicate through shared variables in memory.
        
    - Threads can communicate through sy...read more
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q3. Write a program to reverse a linked list.
- Ans. 
    
    Program to reverse a linked list
    
    - Create a new linked list to store the reversed elements
        
    - Traverse the original linked list and insert each node at the beginning of ...read more
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q4. What is the difference between string literals and the string pool?
- Ans. 
    
    String literals are constant strings defined in code, while the string pool is a memory area where unique string objects are stored.
    
    - String literals are created us...read more
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q5. What is the program to print the 90-degree rotated view of a 2D array?
- Ans. 
    
    Program to print the 90-degree rotated view of a 2D array.
    
    - Iterate through each column in reverse order and print the corresponding row elements.
        
    - Repeat this proces...read more
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

### Interview Advice from Real Candidates

**Interview preparation tips**- Concentrate on the fundamental aspects of Java and Spring Boot.

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote-blue.svg)![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

## [Java Developer Interview Questions & Answers](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer "Wissen Technology Java Developer Interview Questions & Answers")

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions "interview questions")![user image](https://media.naukri.com/media/jphotov1/l244%253ALukcMTmy1wQfGLm5VwgEbpk%252Bwnknasg%252Fg1t73KRqDEcAO8MFznpESZtXGN7l "AKSHAY SANJAY ZOTING")AKSHAY SANJAY ZOTING

posted on 17 Jan 2025

Interview experience

4

Good

Difficulty level

Moderate

Process Duration

Less than 2 weeks

Result

Not Selected

I applied via Walk-in and was interviewed in Dec 2024. There were 2 interview rounds.

Coding TestTechnical

Round 1 - Coding Test 

The assessment consists of basic coding and SQL-related questions, including two challenge questions. The total test duration is 1 hour and 30 minutes, featuring a total of 7 questions, with a passing score of 50% out of 120 points.

Round 2 - Technical 

### (4 Questions)

- Q1. Can you provide an introduction about yourself, including your current place of residence?
- Ans. 
    
    I am a Java Developer currently residing in New York City.
    
    - Experienced Java Developer
        
    - Residing in New York City
        
    - Strong knowledge of Java programming language
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q2. What is a HashMap in Java, and how does it function internally?
- Ans. 
    
    HashMap in Java is a data structure that stores key-value pairs and uses hashing to efficiently retrieve values.
    
    - HashMap is part of the Java Collections framework.
        
    - It allows null keys and values.
        
    - Internally, it uses an array of linked lists to handle collisions.
        
    - The key's hash code is used to determine the index in the array where the key-value pair is stored.
        
    - Example: HashMap<String, Integer> map = new HashMap<>(); map.put("key", 123); int value = map.get("key");
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q3. I was asked to solve a question related to a 2D array that involved iterating through the array and storing the elements in a new array.

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q4. Find second highest salary from employee table?
- Ans. 
    
    Query to find the second highest salary from employee table.
    
    - Use SQL query with ORDER BY and LIMIT to get the second highest salary.
        
    - Example: SELECT DISTINCT salary...read more
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

### Interview Advice from Real Candidates

**Interview preparation tips**- Practice coding questions and interview questions from Ambition before attending your technical interview.

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote-blue.svg)![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

## [Java Developer Interview Questions & Answers](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer "Wissen Technology Java Developer Interview Questions & Answers")

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions "interview questions")![user image](https://lh3.googleusercontent.com/a/ACg8ocIka6zaWFXx9DjhKMvG_u_lw4Wa2t8XaG35nePntQuZoUkdd50Z=s96-c "Bharat Burle")Bharat Burle

posted on 27 Dec 2024

Interview experience

5

Excellent

Difficulty level

Hard

Process Duration

Less than 2 weeks

Result

Not Selected

I applied via Company Website and was interviewed in Nov 2024. There were 2 interview rounds.

Coding TestOne-on-one Round

Round 1 - Coding Test 

There were 3 to 4 questions related to the camera that needed to be solved within the given time.

Round 2 - One-on-one 

### (4 Questions)

- Q1. Postmortem of Hashmap
- Ans. 
    
    A postmortem of HashMap explores its structure, performance, and behavior under various conditions.
    
    - HashMap uses an array of buckets to store key-value pairs, where each bucket can hold multiple entries.
        
    - It handles collisions using linked lists or trees (in Java 8 and above) to maintain performance.
        
    - The load factor (default 0.75) determines when to resize the HashMap, impacting performance.
        
    - HashMap allows null keys and values, making it flexible but requiring careful handling.
        
    - Example: Inserting a key-value pair (1, 'One') involves computing the hash, finding the bucket, and adding the entry.
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q2. Locking mechanism in multithreading
- Ans. 
    
    Locking mechanism in multithreading is used to control access to shared resources by multiple threads.
    
    - Locks are used to prevent multiple threads from accessing shared resources simultaneously
        
    - Types of locks include synchronized blocks, ReentrantLock, and ReadWriteLock
        
    - Locks help prevent race conditions and ensure data consistency in multithreaded applications
        
    

Answered by AI

- Q3. Find highest salaried emp from each dept
- Ans. 
    
    Use SQL query to group by department and find employee with highest salary in each department
    
    - Write a SQL query to group by department and select max salary for each department
        
    - Join the result with employee table to get employee details
        
    - Example: SELECT dept, emp_name, MAX(salary) FROM employees GROUP BY dept
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q4. Sorting algorithms

### Interview Advice from Real Candidates

**Topics to prepare for Wissen Technology Java Developer interview:**

- Java fundamentals
- DS
- Multithreading
- SQL

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote-blue.svg)![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

## [Java Developer Interview Questions & Answers](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer "Wissen Technology Java Developer Interview Questions & Answers")

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions "interview questions")![user image](https://static.ambitionbox.com/static/icons/answered_by_anonymous.png "Anonymous")Anonymous

posted on 2 Jul 2025

Interview experience

3

Average

Difficulty level

Moderate

Process Duration

Less than 2 weeks

Result

Not Selected

I appeared for an interview in Jun 2025, where I was asked the following questions.

- Q1. Given a 2D matrix, how can you perform a spiral traversal to obtain a 1D array?
- Ans. 
    
    Spiral traversal of a 2D matrix involves visiting elements in a spiral order to create a 1D array.
    
    - Start from the top-left corner of the matrix.
        
    - Traverse right until the end of the row, then move down the last column.
        
    - Traverse left across the bottom row, then move up the first column.
        
    - Repeat the process for the inner submatrix until all elements are visited.
        
    - Example: For matrix [[1,2,3],[4,5,6],[7,8,9]], the result is [1,2,3,6,9,8,7,4,5].
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q2. What are the features introduced in Java 8?
- Ans. 
    
    Java 8 introduced significant features like lambdas, streams, and the new Date-Time API, enhancing productivity and code readability.
    
    - Lambda Expressions: Enable functional programming by allowing you to write concise code. Example: (a, b) -> a + b.
        
    - Streams API: Facilitates processing sequences of elements, enabling operations like filter, map, and reduce. Example: list.stream().filter(x -> x > 10).collect(Collectors.toList()).
        
    - Default Methods: Allow interfaces to have methods with implementations, promoting backward compatibility. Example: interface MyInterface { default void myMethod() { System.out.println('Hello'); } }
        
    - Optional Class: A container for optional values to avoid null references. Example: Optional<String> name = Optional.ofNullable(getName());
        
    - New Date-Time API: Provides a comprehensive and standardized way to handle dates and times. Example: LocalDate today = LocalDate.now();
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q3. What is a functional interface?
- Ans. 
    
    A functional interface is an interface with a single abstract method, enabling lambda expressions in Java.
    
    - Functional interfaces are used primarily in Java's functional programming features.
        
    - They can have multiple default or static methods but only one abstract method.
        
    - Common examples include Runnable, Callable, and Comparator.
        
    - You can create your own functional interfaces using the @FunctionalInterface annotation.
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q4. What is the purpose of introducing static and default methods in functional interfaces?
- Ans. 
    
    Static and default methods enhance functional interfaces by providing utility methods and default behavior without breaking existing implementations.
    
    - Static methods allow utility functions related to the interface, e.g., 'static int compare(T a, T b)'.
        
    - Default methods enable adding new functionality to interfaces without affecting existing implementations, e.g., 'default void log() { System.out.println(this); }'.
        
    - They help in maintaining backward compatibility when evolving interfaces.
        
    - Default methods can be overridden in implementing classes, providing flexibility.
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote-blue.svg)![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

## [Java Developer Interview Questions & Answers](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer "Wissen Technology Java Developer Interview Questions & Answers")

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions "interview questions")![user image](https://static.ambitionbox.com/static/icons/answered_by_anonymous.png "Anonymous")Anonymous

posted on 1 Aug 2024

Interview experience

1

Bad

Difficulty level

Hard

Process Duration

4-6 weeks

Result

No response

I applied via Referral and was interviewed in Jul 2024. There were 5 interview rounds.

Coding TestAssignmentTechnical - 1One-on-one Round+ 1 more

Round 1 - Coding Test 

Five MCQs, 2 moderate-level+1 hard level coding,2 SQL queries.

Round 2 - Assignment 

Hard-level 6 Data structure based questions to write the code.

Round 3 - Technical 

### (2 Questions)

- Q1. Flattenedarray code
- Ans. 
    
    Flattening an array involves converting a multi-dimensional array into a single-dimensional array.
    
    - Use recursion to handle nested arrays. Example: flatten([1, [2, [3, 4]], 5]) results in [1, 2, 3, 4, 5].
        
    - Utilize Java Streams for a more functional approach. Example: Arrays.stream(array).flatMap(Arrays::stream).toArray() for 2D arrays.
        
    - Consider edge cases like empty arrays or arrays with null values. Example: flatten([], [null]) should return [].
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q2. Nested array objects to single array.
- Ans. 
    
    Convert nested array objects to single array of strings.
    
    - Iterate through the nested arrays and flatten them into a single array.
        
    - Use recursion to handle multiple levels of nesting.
        
    - Example: [["a", "b"], ["c", "d"]] -> ["a", "b", "c", "d"]
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

Round 4 - One-on-one 

### (2 Questions)

- Q1. Reverse the list node with head node having multiple values.
- Ans. 
    
    Reverse a list node with multiple values in Java.
    
    - Create a new linked list to store the reversed nodes.
        
    - Traverse the original list and add each node to the front of the new list.
        
    - Update the head node of the original list to point to the new list.
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q2. Hard-level string question to write the code.

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

Round 5 - Technical 

### (2 Questions)

- Q1. Advanced Java and database

- Q2. Advanced Data structures

### Skills evaluated in this interview

Data Structures

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote-blue.svg)![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

## [Java Developer Interview Questions & Answers](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer "Wissen Technology Java Developer Interview Questions & Answers")

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions "interview questions")![user image](https://static.ambitionbox.com/static/icons/answered_by_anonymous.png "Anonymous")Anonymous

posted on 11 Jan 2025

Interview experience

5

Excellent

Difficulty level

-

Process Duration

-

Result

-

Technical

Round 1 - Technical 

### (1 Question)

- Q1. Remove duplicates from an array
- Ans. 
    
    Use a Set to remove duplicates from an array of strings.
    
    - Create a Set to store unique elements.
        
    - Iterate through the array and add each element to the Set.
        
    - Convert the Set back to an array to remove duplicates.
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote-blue.svg)![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

## [Java Developer Interview Questions & Answers](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer "Wissen Technology Java Developer Interview Questions & Answers")

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions "interview questions")![user image](https://lh3.googleusercontent.com/a/ACg8ocLR6RnDu_zn42VW73OLMVPQOcKhmA0G7P2gxJjq_HVDcxDRiw=s96-c "Gowthami Vs")Gowthami Vs

posted on 18 Jun 2025

Interview experience

1

Bad

Difficulty level

Hard

Process Duration

2-4 weeks

Result

Not Selected

- Q1. Multhteading program asking in output

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q2. Write the sql query second highest salary
- Ans. 
    
    To find the second highest salary, we can use SQL queries with subqueries or the DISTINCT keyword.
    
    - Use a subquery to select the maximum salary that is less than the highest salary.
        
    - Example: SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary) FROM employees);
        
    - Alternatively, use the DISTINCT keyword to filter unique salaries.
        
    - Example: SELECT DISTINCT salary FROM employees ORDER BY salary DESC LIMIT 1 OFFSET 1;
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

- Q3. What is the deadlock
- Ans. 
    
    Deadlock is a situation in concurrent programming where two or more threads are blocked forever, waiting for each other to release resources.
    
    - Occurs when two or more threads hold resources and wait for each other to release them.
        
    - Example: Thread A holds Resource 1 and waits for Resource 2, while Thread B holds Resource 2 and waits for Resource 1.
        
    - Deadlocks can lead to system unresponsiveness and require careful design to avoid.
        
    - Common strategies to prevent deadlocks include resource ordering and timeout mechanisms.
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote-blue.svg)![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

## [Java Developer Interview Questions & Answers](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions/java-developer "Wissen Technology Java Developer Interview Questions & Answers")

[](https://www.ambitionbox.com/interviews/wissen-technology-interview-questions "interview questions")![user image](https://lh3.googleusercontent.com/a/ACg8ocJVGZwRCTEUJ6mIkjlI6rqjMJ4m_zXqmzqkmwr2k4hHu04cww=s96-c "Satya Nikhil Matlakunta")Satya Nikhil Matlakunta

posted on 17 Dec 2024

Interview experience

3

Average

Difficulty level

-

Process Duration

-

Result

-

Coding TestOne-on-one Round

Round 1 - Coding Test 

Coding test time limit is 60 minutes it is moderate

Round 2 - One-on-one 

### (1 Question)

- Q1. Find the missing number in an given array
- Ans. 
    
    Identify the missing number in a sequence using mathematical formulas or iterative methods.
    
    - Use the formula for the sum of the first n natural numbers: n(n + 1)/2.
        
    - Calculate the expected sum and subtract the actual sum of the array.
        
    - Example: For array [1, 2, 4], expected sum for n=4 is 10, actual sum is 7, missing number is 10-7=3.
        
    - Alternatively, use a boolean array to track presence of numbers.
        
    

Answered by AI

Add your answer![](https://static.ambitionbox.com/static/blue_arrow_right.svg)

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote-blue.svg)![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

Read all interview experiences

Top trending discussions

View All

![](https://static.ambitionbox.com/assets/v2/images/rs:fit:100:100/aHR0cHM6Ly9zdGF0aWMuYW1iaXRpb25ib3guY29tL2FscGhhL2NvbW11bml0eS9sb2dvL2RheS10by1kYXktb2ZmaWNlLnBuZw==.webp)

Daily Office Life

1mo

·

ex -

Wissen Technology

![](https://static.ambitionbox.com/alpha/community/assets/dots.png)

[Is a 'bad culture' just a bad fit, or a universal red flag?](https://www.ambitionbox.com/p/is-a-bad-culture-just-a-bad-fit-or-a-universal-red-flag-id4l3eFU-8)

I recently left a company where the culture was just… awful. It felt like there was no respect among colleagues, and the way employees were treated was borderline harassment. It definitely wasn't the MNC work culture I expected. My mental peace and ...  read more

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/upvote.svg)5![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/downvote.svg)

![](https://static.ambitionbox.com/static/icons/recentWidgetIcons/comment.png)Comment

![](https://static.ambitionbox.com/static/icons/share-outline.svg)Share

Got a question about Wissen Technology?

Ask anonymously on communities.

Ask a Question

![](https://static.ambitionbox.com/static/feed/groups.png)

![](https://static.ambitionbox.com/static/feed/communities_interlinking_1.png)![](https://static.ambitionbox.com/static/feed/communities_interlinking_2.png)![](https://static.ambitionbox.com/static/feed/communities_interlinking_3.png)![](https://static.ambitionbox.com/static/feed/communities_interlinking_4.png)

Contribute & help others!

![anonymous](https://static.ambitionbox.com/static/rewards_revamp/anonymous-icon.svg)

You can choose to be anonymous

![review icon](https://static.ambitionbox.com/static/rewards_revamp/review.svg)Write a review

![salary icon](https://static.ambitionbox.com/static/rewards_revamp/salary.svg)Contribute salary

![interview icon](https://static.ambitionbox.com/static/rewards_revamp/interview.svg)Share interview

![photo icon](https://static.ambitionbox.com/static/rewards_revamp/photo.svg)Add office photos

[More about working at Wissen Technology](https://www.ambitionbox.com/overview/wissen-technology-overview?campaign=MoreAboutCompanyWidget) 

- [HQ - Bengaluru, Karnataka, India](https://www.ambitionbox.com/overview/wissen-technology-overview/locations/bengaluru-offices "HQ - Bengaluru, Karnataka, India companies in India")
- [IT Services & Consulting](https://www.ambitionbox.com/it-services-and-consulting-companies-in-india "IT Services & Consulting companies in India")
- [1k-5k Employees (India)](https://www.ambitionbox.com/list-of-companies?indianEmployeeCounts=1001-5000 "Companies with 1k-5k Employees  (India)")
- [Financial Services](https://www.ambitionbox.com/financial-services-companies-in-india "Financial Services companies in India")
- [Software Product](https://www.ambitionbox.com/software-product-companies-in-india "Software Product companies in India")

## 

Wissen Technology Interview FAQs

Go through your CV in detail and study all the technologies mentioned in your CV. Prepare at least two technologies or languages in depth if you are appearing for a technical interview at Wissen Technology. The most common topics and skills that interviewers at Wissen Technology expect are **Java, Data Structures and Algorithms**.

The most common HR questions asked in Wissen Technology Java Developer interview are -

1. Can you provide an introduction about yourself, including your current place of...[read more](https://www.ambitionbox.com/interviews/question/can-you-provide-an-introduction-about-yourself-including-your-current-place-of-residence-T5tJJhau)
2. Please tell us why you are leaving your compa...[read more](https://www.ambitionbox.com/interviews/question/please-tell-us-why-you-are-leaving-your-company-bkPJ8a8Hy)
3. How do you find the second highest salary from the employee tab...[read mo](https://www.ambitionbox.com/interviews/question/how-do-you-find-the-second-highest-salary-from-the-employee-table-bohq4NO6A)