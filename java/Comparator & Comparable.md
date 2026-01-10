Both are used for **sorting objects**, but their purpose and usage are slightly different.

---
## **1. Comparable Interface**

**Package:** `java.lang`  
**Purpose:** Used to define the **natural sorting** of objects.

### ➤ Syntax:

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
```

### ➤ Example:

```java
class Employee implements Comparable<Employee> {
    int id;
    String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public int compareTo(Employee e) {
        return this.id - e.id; // sort by ID in ascending order
    }
}

public class Main {
    public static void main(String[] args) {
        List<Employee> list = new ArrayList<>();
        list.add(new Employee(3, "John"));
        list.add(new Employee(1, "Abhi"));
        list.add(new Employee(2, "Riya"));

        Collections.sort(list); // Uses compareTo
        for (Employee e : list) {
            System.out.println(e.id + " " + e.name);
        }
    }
}
```

### 🧾 **Output:**

```
1 Abhi
2 Riya
3 John
```

### ✅ **Key Points:**

- Defines **default (natural)** sorting.
- The class itself must **implement** `Comparable`.
- Only **one** sorting logic per class.
---

## ⚙️ **2. Comparator Interface**

**Package:** `java.util`  
**Purpose:** Used to define **custom sorting logic** outside the class.

### ➤ Syntax:

```java
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```

### ➤ Example:

```java
import java.util.*;

class Employee {
    String name;
    int age;
    Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class Main implements Comparator<Employee> {

    @Override
    public int compare(Employee e1, Employee e2) {
        return e1.name.compareTo(e2.name);
    }

    public static void main(String[] args) {
        List<Employee> list = Arrays.asList(
            new Employee("Kumar", 25),
            new Employee("Abhi", 30)
        );

        Collections.sort(list, new Main()); // Main is comparator here
        //Collections.sort(list, new NameComparator());

        for (Employee e : list) {
            System.out.println(e.name + " - " + e.age);
        }
    }
}

//for understanding: instead of implementing on same class we can use diff also
class NameComparator implements Comparator<Employee> {
    @Override
    public int compare(Employee e1, Employee e2) {
        return e1.name.compareTo(e2.name);
    }
}
```

### 🧾 **Output:**

```
1 Abhi
3 John
2 Riya
```

### ✅ **Key Points:**

- Defines **multiple** sorting logics.
- Sorting logic is **external** to the class.
- Can be used even if the class is **not modifiable**.
- Often used with **lambda expressions** in Java 8+.
---

## ⚖️ **Comparison Table**

|Feature|Comparable|Comparator|
|---|---|---|
|Package|java.lang|java.util|
|Method|`compareTo(Object o)`|`compare(Object o1, Object o2)`|
|Sorting logic|Natural (default)|Custom (external)|
|Number of sort orders|One|Multiple|
|Modification|Class must implement it|No need to modify class|
|Example|`Collections.sort(list)`|`Collections.sort(list, comparator)`|

---

## 💡 Java 8 Shortcut Example

You can use Comparator chaining easily:

```java
list.sort(Comparator.comparing(Employee::getName).thenComparing(Employee::getId));
```