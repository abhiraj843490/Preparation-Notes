1. Introduced in java 14 and stable in java 16
2. A **record** in Java is a **special type of class** introduced to **reduce boilerplate code** for classes that are mainly used to hold **immutable data** (like DTOs, value objects).
3. 

### ✅ **Syntax**

`public record Person(String name, int age) {}`

**Equivalent to :**
public final class Person {
    private final String name;
    private final int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public String name() { return name; }
    public int age() { return age; }
    
    @Override
    public boolean equals(Object o) { ... }
    
    @Override
    public int hashCode() { ... }

    @Override
    public String toString() { ... }
}


 **That single line automatically generates:**
- `private final` fields (`name`, `age`)
- A **canonical constructor**
- **Getters** (called _accessors_) and setter is not required because of immutable of fields(private and final)
- `equals()`, `hashCode()`, and `toString()`

