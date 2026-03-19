
# 🔷 What is Normalization?

**Normalization = organizing data to** ensure **data integrity** and make data easier to maintain

- ❌ reduce **redundancy**
    
- ✅ ensure **data integrity**
    
- ❌ avoid **anomalies**
    
- ✅ make data easier to maintain


Think of it like:

> “One fact → stored in one place → updated in one place”

---

# 1️⃣ Redundancy (The Root Problem)

### 🔹 What is Redundancy?

**Same data stored multiple times unnecessarily**

### ❌ Real-Life Example (Before Normalization)

**Student_Course table**

| student_id | student_name | course_id | course_name | instructor |
| ---------- | ------------ | --------- | ----------- | ---------- |
| 1          | Abhi         | C1        | Java        | Raj        |
| 1          | Abhi         | C2        | Spring      | Raj        |
| 2          | Swati        | C1        | Java        | T          |

### ❌ Problems

- "Java" + "Raj" repeated many times
    
- If instructor changes → update everywhere
    
- More storage
    
- Higher chance of inconsistency
    

📌 This is **data duplication**

---

# 2️⃣ Data Integrity (Why Normalization Exists)

### 🔹 What is Data Integrity?

**Data must be accurate, consistent, and reliable**

Example:

- One student name → same everywhere
    
- One course instructor → same everywhere

### ❌ Without Integrity

```
Java → Raj
Java → Rajesh
Java → Raju
```

Which one is correct? 🤯

📌 **Normalization enforces integrity**

---

# 3️⃣ Anomalies (Real Pain in Production)

Anomalies are **side effects of redundancy**.

---

## 🔥 3 Types of Anomalies

---

## A️⃣ Insert Anomaly

### ❌ Problem

You **can’t insert data** without unrelated data.

### Example:

You want to add a **new course** but no student enrolled yet.

```
student_id = NULL ❌
```

📌 Course info is lost until a student exists.

---

## B️⃣ Update Anomaly (Most Dangerous)

### ❌ Problem

Updating same data in **multiple rows**

### Example:

Instructor for Java changes from `Raj → Amit`

You update:

- Row 1 ✅
    
- Forget row 3 ❌

Now DB says:

```
Java → Raj
Java → Amit
```

📌 **Data inconsistency**

---

## C️⃣ Delete Anomaly

### ❌ Problem

Deleting one record removes important data.

### Example:

Delete last student of Java course →  
Course itself disappears 😨

📌 **Loss of business data**

---

# 4️⃣ Normal Forms (Step-by-Step, Deep)

Normalization happens in **levels** called **Normal Forms**.

---

# 🔹 1NF – First Normal Form

### 📌 Rule

- Atomic values (no multi-valued columns)
    
- No repeating groups

---

### ❌ Not in 1NF

|student_id|name|phones|
|---|---|---|
|1|Abhi|9999,8888|

---

### ✅ In 1NF

|student_id|name|phone|
|---|---|---|
|1|Abhi|9999|
|1|Abhi|8888|

📌 Each cell = **single value**

---

# 🔹 2NF – Second Normal Form

### 📌 Rule

- Must be in 1NF
    
- **No partial dependency**

---

### 🔹 What is Partial Dependency?

When a column depends on **part of a composite key**

---

### ❌ Not in 2NF

**Order_Details**

|order_id|product_id|product_name|price|
|---|---|---|---|

**Composite PK** = (order_id, product_id)

Problem:

- product_name depends only on product_id ❌

---

### ✅ In 2NF

**Orders**  
| order_id | order_date |

**Products**  
| product_id | product_name | price |

**Order_Items**  
| order_id | product_id | qty |

📌 Each non-key column depends on **full key**

---

# 🔹 3NF – Third Normal Form (Very Important)

### 📌 Rule

- Must be in 2NF
    
- **No transitive dependency**

---

### 🔹 What is Transitive Dependency?

```
A → B → C
```

Non-key depends on another non-key.

---

### ❌ Not in 3NF

**Employee**

|emp_id|emp_name|dept_id|dept_name|
|---|---|---|---|

Dependency:

```
emp_id → dept_id
dept_id → dept_name
```

---

### ✅ In 3NF

**Employee**  
| emp_id | emp_name | dept_id |

**Department**  
| dept_id | dept_name |

📌 Change department name → update once

---

# 🔹 BCNF (Boyce–Codd Normal Form)

### 📌 Rule

- Stronger version of 3NF
    
- **Every determinant must be a candidate key**

---

### Example (Simplified)

|student|subject|professor|
|---|---|---|
|A|Math|P1|
|B|Math|P1|

Rule:

- Subject → Professor
    
- Student → Subject ❌

Violates BCNF

📌 Rare but asked in interviews

---

# 🔹 4NF (Multi-valued Dependency)

### ❌ Problem

One entity has **independent multi-values**

Example:

- Student → skills
    
- Student → hobbies

---

### ✅ Split tables

**Student_Skills**  
**Student_Hobbies**

---

# 🔹 5NF (Join Dependency)

- Mostly theoretical
    
- Rarely used in enterprise

---

# 5️⃣ Real Enterprise Example (Banking)

### ❌ Bad Design

|acc_no|acc_name|branch|branch_city|manager|
|---|---|---|---|---|

Problems:

- branch info repeated
    
- update anomaly
    
- delete anomaly

---

### ✅ Normalized Design

**Account**  
| acc_no | acc_name | branch_id |

**Branch**  
| branch_id | branch_city | manager |

📌 Clean, scalable, consistent

---

# 6️⃣ When NOT to Normalize Fully? (Important)

### ❗ Controlled Denormalization

Used when:

- Heavy read systems
    
- Reporting dashboards
    
- Performance critical paths

Example:

```sql
order_total_amount
```

(derived but stored)

📌 **Normalize for correctness → Denormalize for performance**

---

# 7️⃣ Interview Gold Summary 🏆

- **Redundancy causes anomalies**
    
- **Normalization removes redundancy**
    
- **1NF → atomic values**
    
- **2NF → no partial dependency**
    
- **3NF → no transitive dependency**
    
- **3NF is enough for 90% enterprise apps**
    
- **Denormalize only with reason**

---

If you want, next we can:

- 🔥 Solve **interview questions on normalization**
    
- 🔥 Convert a **bad schema → normalized schema**
    
- 🔥 Map normalization concepts to **Spring Boot JPA**
    
- 🔥 Compare **Normalization vs Denormalization**