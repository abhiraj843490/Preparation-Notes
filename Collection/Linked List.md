**`LinkedList`** in Java is a class that implements the **Java Collections Framework** and represents a **doubly linked list** data structure.

It implements both:

- `List`
    
- `Deque`

So it can work as **List, Queue, or Stack**.

---

# 1️⃣ Structure of LinkedList

Each element (node) contains:

```
[ Prev Address | Data | Next Address ]
```

Example:

```
null <- [10] <-> [20] <-> [30] -> null
```

Unlike `ArrayList`, elements are **not stored in contiguous memory**.

---

# 2️⃣ Declaration

```java
import java.util.LinkedList;

LinkedList<Integer> list = new LinkedList<>();
```

Example:

```java
LinkedList<Integer> list = new LinkedList<>();
list.add(10);
list.add(20);
list.add(30);

System.out.println(list);
```

Output

```
[10, 20, 30]
```

---

# 3️⃣ Important Methods

### Add elements

```java
list.add(10);
list.addFirst(5);
list.addLast(40);
```

---

### Remove elements

```java
list.remove();
list.removeFirst();
list.removeLast();
```

---

### Access elements

```java
list.get(1);
list.getFirst();
list.getLast();
```

---

# 4️⃣ Time Complexity

|Operation|Time|
|---|---|
|Insert at beginning|O(1)|
|Insert at end|O(1)|
|Access by index|O(n)|
|Search|O(n)|

Because traversal is required.

---

# 5️⃣ LinkedList vs ArrayList

|Feature|LinkedList|ArrayList|
|---|---|---|
|Structure|Doubly linked list|Dynamic array|
|Access|Slow O(n)|Fast O(1)|
|Insert/Delete|Fast|Slower|
|Memory|More memory|Less memory|

Example:

```
ArrayList → [10,20,30,40]
LinkedList → 10 ↔ 20 ↔ 30 ↔ 40
```

---

# 6️⃣ When to Use LinkedList

Use when:

- Frequent **insertions**
    
- Frequent **deletions**
    
- **Queue/Deque implementation**

Do NOT use when:

- Frequent **random access**

---

# 7️⃣ Interview Question

### Reverse LinkedList

```java
Node prev = null;
Node current = head;

while(current != null){
    Node next = current.next;
    current.next = prev;
    prev = current;
    current = next;
}
head = prev;
```

---

# 8️⃣ Internal Node Structure

Internally Java LinkedList uses something like:

```java
class Node<E>{
    E item;
    Node<E> next;
    Node<E> prev;
}
```

---

# 9️⃣ Key Interview Points

- LinkedList is **doubly linked list**
    
- Implements **List and Deque**
    
- Better for **insert/delete**
    
- Slower for **random access**

---

✅ If you're preparing for **2–3 year Java interviews**, the interviewer may ask these **LinkedList coding questions**:

1️⃣ Reverse LinkedList  
2️⃣ Detect loop in LinkedList  
3️⃣ Find middle of LinkedList  
4️⃣ Merge two LinkedLists  
5️⃣ Remove Nth node from end
