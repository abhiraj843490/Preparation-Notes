

Spring beans describes the java object 
- It is a fundamental building blocks of a spring app and are created, configured and managed by the spring container.
    
- @Component, @Service, @Repository, and @Controller to mark classes as beans. 
- 
Life cycle:
1. create 
2. init
3. service 
4. destroy

Bean Scope:
- Defines the lifecycle of a bean, i.e., how many instances of the bean will be created and how they will be shared:
1. Singleton: One instance per Spring IoC container (default).
2. Prototype: A new instance each time a bean is requested.
3. Request: One instance per HTTP request (web applications).
4. Session: One instance per HTTP session (web applications).
5. GlobalSession: One instance per global HTTP session (web applications).
6. Application: One instance per Spring ApplicationContext.