
# 🔷 What is a Heap?

A **Heap** is a **special tree-based data structure** that satisfies the **Heap Property**.

It is **NOT just any tree** — it is a:

✅ **Complete Binary Tree**  
+  
✅ Follows **Heap Order Property**

---

## 🏗 1. Complete Binary Tree (Important)

A binary tree where:

- All levels are completely filled
    
- Last level filled **from left to right**
    

👉 This is why heaps are stored in **arrays**, not pointers.

Example shape:

```
        50
       /  \
     30    40
    /  \   /
   10  20 35
```

---

# 🔷 Types of Heap

There are **2 main types**

## 1️⃣ Max Heap

**Rule:** Parent ≥ Children

Top element = **largest**

```
        90
       /  \
     70    60
    /  \
   40  50
```

Used in:

- Priority queues (highest priority first)
    
- Heap Sort (descending)
    

---

## 2️⃣ Min Heap

**Rule:** Parent ≤ Children

Top element = **smallest**

```
        10
       /  \
     20    15
    /  \
   30  40
```

Used in:

- Dijkstra’s algorithm
    
- Prim’s algorithm
    
- Task scheduling
    
- Median of stream
    

---

# 🔷 How Heap is Stored Internally

Heaps use **ARRAY**, not nodes.

If index = `i`

|Relation|Formula|
|---|---|
|Left Child|`2*i + 1`|
|Right Child|`2*i + 2`|
|Parent|`(i-1)/2`|

Example:

Heap:

```
        50
       /  \
     30    40
    /  \
   10  20
```

Array representation:

```
Index: 0  1  2  3  4
Value: 50 30 40 10 20
```

---

# 🔷 Core Heap Operations

|Operation|Time Complexity|
|---|---|
|Insert|O(log n)|
|Delete Root|O(log n)|
|Heapify|O(log n)|
|Peek|O(1)|
|Build Heap|O(n)|

---

# 🔷 Important Algorithms

---

## 🟢 1. Heapify (Heart of Heap)

Used to maintain heap property.

### Max Heapify (Fix downward)

If a node violates heap rule, push it down.

### Steps:

1. Compare node with left & right child
    
2. Swap with largest
    
3. Repeat
    

```java
void maxHeapify(int arr[], int n, int i) {
    int largest = i;
    int left = 2*i + 1;
    int right = 2*i + 2;

    if(left < n && arr[left] > arr[largest])
        largest = left;

    if(right < n && arr[right] > arr[largest])
        largest = right;

    if(largest != i) {
        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;

        maxHeapify(arr, n, largest);
    }
}
```

---

## 🟢 2. Insert into Heap (Heapify Up)

Steps:

1. Add element at end
    
2. Compare with parent
    
3. Swap if needed
    
4. Repeat until valid
    

```java
void insert(int arr[], int n, int key) {
    n++;
    int i = n - 1;
    arr[i] = key;

    while(i > 0 && arr[(i-1)/2] < arr[i]) {
        swap(arr[i], arr[(i-1)/2]);
        i = (i-1)/2;
    }
}
```

---

## 🟢 3. Delete Root

Steps:

1. Replace root with last element
    
2. Reduce heap size
    
3. Heapify from root
    

---

## 🟢 4. Build Heap (O(n) trick)

Start from **last non-leaf node**

```
Last non-leaf = (n/2) - 1
```

```java
void buildHeap(int arr[], int n) {
    for(int i = n/2 - 1; i >= 0; i--) {
        maxHeapify(arr, n, i);
    }
}
```

---

## 🟢 5. Heap Sort

1. Build Max Heap
    
2. Swap root with last
    
3. Reduce heap size
    
4. Heapify
    
5. Repeat
    

Time: **O(n log n)**

---

# 🔥 Why Build Heap is O(n) (INTERVIEW GOLD)

Because:

- Lower levels have more nodes but small height
    
- Upper levels fewer nodes but bigger height  
    Total work = linear
    

---

# 🔷 Summary

|Feature|Heap|
|---|---|
|Structure|Complete Binary Tree|
|Storage|Array|
|Root|Min/Max|
|Insert|Heapify Up|
|Delete|Heapify Down|
|Build Heap|Bottom-Up|
|Use Cases|Priority Queue, Graph algorithms, Sorting|
