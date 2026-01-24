## 🔹 What is Serialization?

**Serialization** is the process of **converting a Java object into a byte stream** so that it can be:

- Stored on disk
- Sent over a network
- Cached in memory
- Shared between JVMs

📌 Java uses `ObjectOutputStream` for serialization.

---

## 🔹 What is Deserialization?

**Deserialization** is the **reverse process**:

> Converting the byte stream back into a Java object.

📌 Java uses `ObjectInputStream` for deserialization.

---

## 🎯 Why Do We Use Serialization?

|Use Case|Reason|
|---|---|
|Network communication|Send objects over sockets|
|Caching|Store objects in Redis, Ehcache|
|Persistence|Save object state to file|
|Distributed systems|Transfer objects between JVMs|
|Session management|Store HTTP session data|
|Messaging|Kafka, JMS payloads|

---

## ✅ Simple Example

### Serializable Class

```java
import java.io.Serializable;

class User implements Serializable {
    private static final long serialVersionUID = 1L;

    int id;
    String name;

    User(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

---

### Serialization

```java
ObjectOutputStream oos =
    new ObjectOutputStream(new FileOutputStream("user.ser"));
oos.writeObject(new User(1, "Abhiraj"));
oos.close();
```

---

### Deserialization

```java
ObjectInputStream ois =
    new ObjectInputStream(new FileInputStream("user.ser"));
User user = (User) ois.readObject();
ois.close();
```

---

## 🧠 How Java Serialization Works (Internal)

1. JVM checks if class implements `Serializable`
    
2. Generates a byte stream
    
3. Stores:
    
    - Object data
        
    - Class metadata
        
    - `serialVersionUID`
    
4. On deserialization:
    
    - JVM recreates object **without calling constructor**
        
    - Populates fields directly
    

📌 **Constructor is NOT called during deserialization**

---

## 🆔 serialVersionUID

```java
private static final long serialVersionUID = 1L;
```

## What Exactly Happens During Deserialization?

When Java deserializes an object:

1. JVM reads the **`serialVersionUID`** stored in the serialized byte stream
    
2. JVM compares it with the **`serialVersionUID` of the current class**
    
3. If they match → **Deserialization succeeds**
    
4. If they don’t match → ❌ **`InvalidClassException`**

### Interview Line:

> serialVersionUID is a version control mechanism for Serializable classes.

---

## ❌ What is NOT Serialized?

|Item|Reason|
|---|---|
|`static` fields|Belong to class|
|`transient` fields|Explicitly ignored|
|Non-Serializable objects|Throws exception|

---

### transient Example

```java
transient String password;
```

✔ Prevents sensitive data serialization

---

## 🚫 What Happens if a Class Doesn’t Implement Serializable?

```java
java.io.NotSerializableException
```

---

## 🔐 Security Risk in Serialization

❗ Deserialization can execute malicious code.

### Interview Answer:

> Java deserialization is vulnerable to remote code execution if untrusted input is deserialized.

---

## 🔄 Custom Serialization

Use:

```java
private void writeObject(ObjectOutputStream oos)
private void readObject(ObjectInputStream ois)
```

Example:

```java
private void writeObject(ObjectOutputStream oos) throws IOException {
    oos.defaultWriteObject();
}
```

---

## ⚡ Externalizable (Advanced)

|Serializable|Externalizable|
|---|---|
|Automatic|Manual control|
|Less code|More control|
|Slower|Faster|
|Uses reflection|Uses explicit logic|

---

## 🌍 Real-World Usage

|System|Usage|
|---|---|
|Spring Session|Serialize session|
|Redis cache|Serialize objects|
|Kafka|Message payload|
|RMI|Remote calls|
|JMS|ObjectMessage|

---

## 🧪 Serialization in Spring Boot

Spring prefers:

- JSON (Jackson)
    
- ProtoBuf
    
- Avro


❌ Native Java serialization is avoided in microservices.

---

## 🧠 Interview FAQs

### Q1: Why constructors are not called?

✔ JVM uses reflection to create object

---

### Q2: Difference between Serialization & Marshalling?

- Serialization → Java-specific
    
- Marshalling → Language-neutral (JSON/XML)
    

---

### Q3: Can final fields be serialized?

✔ Yes, but not changeable after deserialization

---

### Q4: How to prevent serialization?

```java
private void writeObject(...) throws NotSerializableException
```

---

## 🗣 Interview-Ready Summary

> “Serialization is the process of converting an object into a byte stream for storage or transmission, while deserialization recreates the object from that stream. It is used in networking, caching, and distributed systems. Classes must implement Serializable, and serialVersionUID ensures compatibility.”

---

