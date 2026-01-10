## **@SpringBootApplication**

@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan

# @SpringBootConfiguration

**1️⃣ Marks the main class as the central configuration**
It tells Spring Boot:
- "This is the starting point for configuration."

 **2️⃣ Enables Spring IoC container configuration**
Because it includes `@Configuration`, it allows:
- Bean definitions
- Bean lifecycle handling
- Dependency injection

 **3️⃣ Used for Spring Boot auto-configuration detection**
- Spring Boot looks for this annotation to locate the main class during startup, especially in testing.

# @EnableAutoConfiguration
Auto configures the beans that are present in classpath. This simplifies the developers work by guessing the required beans from classpath and configuring it to run the application.

 I.e Spring Boot to automatically configure the application based on the classpath dependencies, reducing the need for manual configuration.

- Use this with @Configuration.


# @ComponentScan
- scan a package for beans   
- @ComponentScan(basePackages = "com.javatpoint")  
- scans all the beans and package declarations when the application initializes.


**@Conditional:**
Annotations to conditionally enable or disable themselves based on the presence or absence of certain conditions.

  **@Configuration**
A class defines one or more spring beans.

@Configuration to define and configure custom beans related to JPA components like EntityManagerFactory, JpaTransactionManager, and DataSource.

   **@Autowired**
commonly It is useful for injecting the dependency. 
- It allows you to inject beans into other beans without explicitly creating instances.

## **IOC:**
IoC Container stands for Inversion of Control container. It is a core concept in frameworks like Spring, 

where the container is responsible for managing the creation, configuration, and life cycle of objects (often referred to as beans). The IoC container handles the responsibility of managing dependencies by using Dependency Injection (DI).

This process involves the container injecting the required dependencies into objects, rather than the objects creating or finding those dependencies themselves. This approach promotes loose coupling between components, making the application easier to manage, test, and scale.
- Manage beans creation, and its lifecycle, inject dependency into them.
- Transfer the control of objects of a program to a container.
- [https://www.javatpoint.com/ioc-container](https://www.javatpoint.com/ioc-container) 
    
 **Dependency injection:**
Spring container injects objects into other objects.
It manages and provides dependencies to objects or components.

## Scheduling Annotations

**@EnableAsync**
Enable asynchronous functionality.
Useful in time - consuming or non-blocking tasks to separate threads to improve the responsiveness of application.
Note:

@SpringBootApplication / @Configuration
@EnableAsync 
public class MyApp{}

## @Async 

Make a specific method asynchronous.
Allow separate thread for separate execution. 
## @EnableScheduling

Enable scheduled task execution in a spring application.
useful for automating and scheduling routine tasks in your application.
Ex. sending emails at specific times. And more.

## @Scheduled(cron = "0 0 1 * * ?")

Cron is an expression to specify the schedule.
Cron expression defines the timing and frequency of the task execution.
@Scheduled should have a void return type.
  

0 0 0 * * ?  ⇒  0 sec(0-59)  0 min(0-59) 0 hrs(0-59) * day(1-31) * month(1-12) * week(0-7)

@Schedules({ 
@Scheduled(fixedRate = 10000), @Scheduled(cron = "0 * * * * MON-FRI") 
})


# Data Annotations

@Transactional

# @Scopes

1. Singleton
2. prototype
3. Session
4. Request
5. Application 
6. Web socket

## **Singleton** : 
by default scopes is singleton, when we request for the object spring container gives Singleton scope .

**Prototype:** when we want to create-different object for each request.

Always use with @Component annotation

@Scope(“prototype”) i.e it said to the spring container create new object  
## @OneToOne

- One-To-One association can be either unidirectional or bidirectional.
- Unidirectional: Source entity’s table contains the foreign key. 
- Bidirectional: Target entity’s table contains the foreign key
- Source entity must use the mappedBy attribute to define the bidirectional one-to-one mapping.
    
@OneToOne(cascade = CascadeType.ALL)
@JoinColumn(name = "address_id")
private CusAddress address;

## Cascading:

Cascading is the way to achieve this. ‘When we perform some action on the target entity, the same action will be applied to the associated entity’.
**Jpa cascade type:**
All JPA-specific cascade operations are represented by the javax.persistence.CascadeType enum containing entries:

- ALL
- PERSIST
- MERGE
- REMOVE
- REFRESH
- DETACH

example:

@OneToOne(cascade = CascadeType.ALL)
@OneToOne(cascade = CascadeType.PERSIST)
@OneToOne(cascade = CascadeType.MERGE)
@OneToOne(cascade = CascadeType.REMOVE)
@OneToOne(cascade = CascadeType.REFRESH)

ALL: 
This cascade type includes all the cascade operations. If an entity has the CascadeType.ALL option set, it means that any operation performed on the owning entity (e.g., persist, merge, remove, refresh, or detach) will also be cascaded to the associated entity.

PERSIST:
 With CascadeType.PERSIST, when you persist (i.e., save) an entity, it will also cascade the persist operation to its associated entities. This means that new associated entities will also be saved.

MERGE:
 CascadeType.MERGE means that when you merge (i.e., update) an entity, it will also cascade the merge operation to its associated entities. This ensures that changes made to associated entities are also merged.

REMOVE:
 CascadeType.REMOVE is used when you remove (i.e., delete) an entity. If this cascade type is set, it will also cascade the remove operation to associated entities, effectively deleting them as well.

REFRESH:
 The CascadeType.REFRESH option is used when you refresh an entity. Refreshing an entity means reloading its state from the database. If this cascade type is set, refreshing the owning entity will also cascade the refresh operation to associated entities.

DETACH:
CascadeType.DETACH, the child entity will also get removed from the persistent context.

Detaching an entity means that it is no longer managed by the persistence context. If this cascade type is set, detaching the owning entity will also cascade the detach operation to associated entities.

Fetch
To Use to control the loading behavior of entity associations.

types :
1. Eager Loading
2. Lazy Loading

1.**Eager Loading:**
you have an entity called "Author" and another entity called "Books," and there's a one-to-many relationship between them (one author can have many books).

With eager loading, when you retrieve an author from the database, all of their associated books are also loaded immediately. It's like grabbing the author and all their books at once.

2.**Lazy Loading:**
Going back to our "Author" and "Books" example, if you use lazy loading, when you retrieve an author, only the author's information is fetched initially. 

If you want to access their books, a separate query is made to load the books from the database when you actually try to access them.

  Lazy loading is useful when you have a large amount of associated data, and you don't always need it, as it reduces the initial database load and can improve performance.

NOTE:
@ManyToMany associations use the FetchType.LAZY strategy.
@OneToOne and @ManyToOne use the FetchType.EAGER strategy.
@OneToMany(cascade = CascadeType.ALL, fetch = FetchType.LAZY)
@JoinColumn(name = "order_id")

  
## **Junit testing:**

Testing done by programmer on their own piece of code is called unit testing.
- Unit testing is defined as a type of software testing where individual components of a software are tested .
## Peer testing:

Unit testing done by 1 programmer's code/task on their colleague programmer is called peer testing.

  

Development phase we perform Unit testing.
Note:
Testing means matching expected results with actual results. 

JUnit: 
Junit is a popular unit testing framework for java programming language 
Latest version is Junit5 ( minimum Java ver 8)

  

Note: 
  Combination of 3 things

Junit5 =  Junit platform + Junit jupiter engine + Junit vintage 

<dependency>

<groupId>org.junit.jupiter</groupId>

<artifactId>junit-jupiter-engine</artifactId>

<version>5.7.0</version>

<scope>test</scope>

</dependency>

## Junit5 Annotations

File location

src/main/java: .java files

src/main/resource: Non java files, .txt, .properties, image etc, .xml

src/test/java: java test classes

—-----------------------------------------------------------------------------------

## Annotations: Introduced Java 1.5

: Always Begin @ 

1. @Test : to indicates a method as test method
2. @DisplayName(value = “contains test methods related to employee”)

It is a class and method level annotation, customize description about test class or test method

3. @BeforeEach: when a method is annotated with @BeforeEach annotation then that method will be executed before each and every test method.
    
4. @AfterEach: when a method is annotated with @AfterEach annotation then that method will be executed after each and every test method.
    
5. @BeforeAll: when a method is annotated with @BeforeAll annotation then that method will be executed only once before all test methods           * method should be static.
    
6. @AfterAll: When a method is annotated with @AfterAll annotation then that method will be executed only once after all test methods. *method should be static.
    
7. Disabled:  to disable either test class or test method.
    

Example:
	
@DisplayName(value = “contains test methods related to employee”)
Class EmployeeTests{
@AfterAll
Public static void beforeEach(){
	System.out.println(“After All method”);
}

@BeforeAll
Public static void beforeEach(){
	System.out.println(“before All method”);
}

@BeforeEach
Public void beforeEach(){
	System.out.println(“before each method”);
}

@Test
@Disabled
@DisplayName(value = “contains test methods related to employee”)
Public void testCreateEmployee() {
	System.out.println(“created employee method this will be executed”);
}

@AfterEach
Public void beforeEach(){
	System.out.println(“After each method”);
}
}

  

Conditional Test Execution Annotation:

1. Condition on OS [ @EnabledOnOs]
2. Condition on JRE [ @EnabledOnJre]
3. Condition on JRE [ @EnabledForJreRange]
4. Condition on system properties[ @EnabledIfSystemProperty ]
    

Class DemoTests{

@Test
@EnabledOnOs(os.windows)
Public void testOnWindow(){
	System.out.println(“execute on windows os only”);
}
}

  

@Order annotation
@Order annotation is useful for set the priority of test-method to it’s execution.

Example: 
@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
Class DemoTests{
@Test @Order(2)
Public void test1(){
	System.out.println(“welcome in test1 method”);
}

@Test @Order(1)
Public void test2(){
	System.out.println(“welcome in test2 method”);
}
}

@RepeatedTest(int)

Numbers of times-test will be executed.

@Test 
@RepeatedTest(3)
public void test2(){
	System.out.println(“welcome in test2 method”);
} 

@RepeatedTest(value=3)
public void test2(RepetitionInfo info){
	System.out.println(“current repetition:”+info.getCurrentRepetition());
	System.out.println(“current repetition:”+info.getTotalRepetition());
} 

Note : test2 executed 4 times. One for @Test and other for @RepeatedTest

## @Assertions api

- Test case pass or fail it’s depends on the actual and expected results matching or not.
    
- All Junit assertions are static methods.
    
@Getter @Setter @NoArg @AllArg
Public class Employee{
	private int id;
	Private String name;
}

Public class EmployeeService{
// i want to test the particular method
Public Employee getEmployee(){
	Employee e = new Employee(1, “rathor”, “Gujarat”);
	return e;
}

Note: 

- src/test/java
- Package name same as service’s package name

@DisplayName  @Order  @Disabled
@BeforeEach @AfterEach @AfterAll @BeforeAll

Public class EmployeeServiceTests{
EmployeeService es = null;
Employee expecedEmployee = null;

@BeforeEach 
Public void beforeEach(){
	Es = new  EmployeeService();
	expecedEmployee = new Employee(1,”prudhvi”, “Goa”);  
}

// this test check, actual is not equal to expected 

@Test 
Public void testGetNotEmployee(){
	Employee actualEmployee = es.getEmployee();
	Assertions.assertNotEquals(expecedEmployee , actualEmployee ,”expecedEmployee object and actualEmployee object are not equal”);
}

// this test check, actual is not null

@Test 
Public void testGetNotReturningNull(){
	Employee actualEmployee = es.getEmployee();
	Assertions.assertNotNull( actualEmployee );
}

@Test 
public void testArrayEquals(){
Int actual[] = {1,2,3,4};
Int expected[] = {1,2,3};
Assertions.assertArrayEquals(expected , actual); } // not pass

@AfterEach
Public void afterEach(){
	es  = null;
	expecedEmployee = null;
}

If expected results are matching with actual results then test case should pass.
If expected results are not matching with actual results then test case should fail

  

All methods in Assertions api are static
- assertNull
- assertNotNull
- assertTrue
- assertFalse
- assertSame (check for reference of object pointing to same obj or not)
- assertNotSame
- assertThat
- assertIterableEquals
- assertEquals
    

@Test 

Public void testAssertNotNull(){
	Object obj = new Object();
	Assertions.assertNotNull( obj );
}

## Timeout

- @Timeout(seconds)
    

@Test
@Timeout(4)
Public void testTimeOut(){
	Thread.sleep(6000); 
}
## Exception

Assertions.assertThrows()
Assertions.assertDoesNotThrow()
@Test
Public void testException(){
	Assertions.asserThrows(ArithmeticException.class, ()->divide(10, 0));
}

Public void divide(int fnum, int snum){
	Int result = fnum/snum;
}

## @Primary Annotation
Marks a bean as the default choice when multiple beans are available for autowiring

public interface PaymentService {
    void pay();
}

@Service
public class CreditCardPayment implements PaymentService {
    public void pay() {
        System.out.println("Paid using Credit Card");
    }
}

@Service
@Primary
public class DebitCardPayment implements PaymentService {
    public void pay() {
        System.out.println("Paid using Debit Card");
    }
}

@RestController
public class PaymentController {
    private final PaymentService paymentService;

    @Autowired
    public PaymentController(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}


## @Qualifier Annotation
specifies exactly which bean to inject when multiple beans of the same type exist

@RestController
public class PaymentController {
    private final PaymentService paymentService;

@Autowired
public PaymentController(@Qualifier("creditCardPayment") PaymentService paymentService) {
    this.paymentService = paymentService;
    }
}




## Q. what is the difference b/w component and service annotation, and what will happen when in Dao class going to use Service Annotation?
