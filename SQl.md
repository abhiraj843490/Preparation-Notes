## 🧩 **LEVEL 1 — EASY**

### 🧠 SQL

1️⃣ **Select all columns**

```sql
SELECT * FROM employees;
```

2️⃣ **Filter rows**

```sql
SELECT name, department FROM employees WHERE salary > 50000;
```

3️⃣ **Sorting**

```sql
SELECT name, salary FROM employees ORDER BY salary DESC;
```

4️⃣ **Count employees**

```sql
SELECT COUNT(*) FROM employees;
```

5️⃣ **Distinct departments**

```sql
SELECT DISTINCT department FROM employees;
```
## 🧩 **LEVEL 2 — MEDIUM**

### 🧠 SQL

1️⃣ **Aggregation (Group By)**

```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

2️⃣ **Having Clause**

```sql
SELECT department, COUNT(*) AS emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

3️⃣ **Inner Join**

```sql
SELECT e.name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.id;
```

4️⃣ **Left Join**

```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
```

5️⃣ **Find Nth highest salary**

```sql
SELECT DISTINCT salary 
FROM employees 
ORDER BY salary DESC
LIMIT 1 OFFSET 2;  -- 3rd highest salary
```
## 🧩 **🎯 Problem:**

Find the **3rd highest salary** (or in general, the N-th highest salary) from the `employees` collection/table.

---

## ✅ **SQL — Nested Query Approach**

### 📘 1️⃣ Using Subquery:

```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary)
    FROM employees
    WHERE salary < (
        SELECT MAX(salary)
        FROM employees
    )
);
```

➡️ **Explanation:**

- Inner-most query → finds **highest salary**
- Middle query → finds **2nd highest**
- Outer query → finds **3rd highest**

**Generalized version (N-th highest salary):**

```sql
SELECT salary
FROM employees e1
WHERE N-1 = (
  SELECT COUNT(DISTINCT e2.salary)
  FROM employees e2
  WHERE e2.salary > e1.salary
);
```

👉 For 3rd highest salary:

```sql
SELECT salary
FROM employees e1
WHERE 2 = (
  SELECT COUNT(DISTINCT e2.salary)
  FROM employees e2
  WHERE e2.salary > e1.salary
);
```

## 🧠 **Summary Table**

|Goal|SQL Query|MongoDB Query|
|---|---|---|
|3rd highest salary|Uses `MAX()` and nested subquery or count comparison|Uses `$sort`, `$skip`, `$limit` or `$group` + `$arrayElemAt`|
|Handles duplicates|Use `DISTINCT` in SQL|Use `$addToSet` in MongoDB|
|Generalize for N-th|Change `OFFSET` or subquery count|Change `$skip` or array index|


## 🧩 **LEVEL 3 — ADVANCED / HIGH LEVEL**

### 🧠 SQL

1️⃣ **Subquery Example**

```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

2️⃣ **CTE (Common Table Expression)**

```sql
WITH dept_avg AS (
  SELECT department_id, AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department_id
)
SELECT e.name, e.salary, d.avg_salary
FROM employees e
JOIN dept_avg d ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```

3️⃣ **Window Functions**

```sql
SELECT 
  name, department,
  salary,
  RANK() OVER(PARTITION BY department ORDER BY salary DESC) AS rank_in_dept
FROM employees;
```

4️⃣ **Find duplicate records**

```sql
SELECT name, COUNT(*)
FROM employees
GROUP BY name
HAVING COUNT(*) > 1;
```

5️⃣ **Self Join**

```sql
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
JOIN employees e2 ON e1.manager_id = e2.id;
```
## 🧠 Quick Recap — When to Use What

|Type|SQL Concept|MongoDB Equivalent|
|---|---|---|
|Join|`JOIN`|`$lookup`|
|Group|`GROUP BY`|`$group`|
|Filter|`WHERE`|`$match`|
|Sort|`ORDER BY`|`$sort`|
|Limit|`LIMIT`|`$limit`|
|Subquery|Nested `SELECT`|`$lookup` + `$expr`|
|Window Function|`RANK()`, `OVER()`|`$setWindowFields`|

## 🧩 **Interview Tip**

When asked SQL ↔ MongoDB conversions:

> ✅ Always explain that SQL is **table-based (relational)**, while MongoDB is **document-based**, and joins or aggregations are achieved through `$lookup`, `$group`, and `$match`.



WHERE vs HAVING

| Feature                        | WHERE                        | HAVING                              |
| ------------------------------ | ---------------------------- | ----------------------------------- |
| Filters applied on             | Rows                         | Groups                              |
| Works with aggregate functions | ❌ No                         | ✅ Yes                               |
| Executed                       | Before `GROUP BY`            | After `GROUP BY`                    |
| Used with                      | `SELECT`, `UPDATE`, `DELETE` | Only with `SELECT` (and `GROUP BY`) |
| Example usage                  | `WHERE salary > 50000`       | `HAVING COUNT(*) > 5`               |


JOINS
---

## 🔹 1️⃣ INNER JOIN

**Definition:** Returns only the rows that have matching values in both tables.

✅ **Syntax:**

```sql
SELECT *
		FROM employees e INNER JOIN departments d ON e.dept_id = d.id;
```

✅ **Result:**  
Only rows where `dept_id` from `employees` matches `id` from `departments`.

🧩 **Venn Diagram:**

```
     [A] ●∩● [B]   → Only intersection (common part)
```

---

## 🔹 2️⃣ LEFT JOIN (LEFT OUTER JOIN)

**Definition:** Returns **all rows from the left table**, and the **matching rows** from the right table.  
If no match, the right-side columns become **NULL**.

✅ **Syntax:**

```sql
SELECT *
FROM employees e
LEFT JOIN departments d
ON e.dept_id = d.id;
```

✅ **Result:**  
All employees are shown. If some employee has no department, department fields will be `NULL`.

🧩 **Venn Diagram:**

```
     [A●]───●[B]   → All of A + matching part from B
```

---

## 🔹 3️⃣ RIGHT JOIN (RIGHT OUTER JOIN)

**Definition:** Opposite of LEFT JOIN.  
Returns **all rows from the right table**, and the **matching rows** from the left table.  
If no match, left-side columns become **NULL**.

✅ **Syntax:**

```sql
SELECT *
FROM employees e
RIGHT JOIN departments d
ON e.dept_id = d.id;
```

✅ **Result:**  
All departments are shown, even if no employee belongs to them.

🧩 **Venn Diagram:**

```
     [A]●───●[B●]   → All of B + matching part from A
```

---

## 🔹 4️⃣ FULL JOIN (FULL OUTER JOIN)

**Definition:** Returns **all rows from both tables**, with `NULL` where there’s no match.

✅ **Syntax:**

```sql
SELECT *
FROM employees e
FULL OUTER JOIN departments d
ON e.dept_id = d.id;
```

✅ **Result:**  
All employees and all departments — even if some have no match.

🧩 **Venn Diagram:**

```
     [A●]───●[B●]   → Both sides + intersection
```

---

## 🔹 5️⃣ CROSS JOIN

**Definition:** Returns the **Cartesian product** — every row of the first table is joined with every row of the second table.

✅ **Syntax:**

```sql
SELECT *
FROM employees e
CROSS JOIN departments d;
```

✅ **Result:**  
If `employees` has 5 rows and `departments` has 3 rows → result = 15 rows.

🧩 **Venn Diagram:**

```
     Every combination of A × B
```

---

## 🔹 6️⃣ SELF JOIN

**Definition:** A table joined with itself — usually to compare rows within the same table.

✅ **Syntax:**

```sql
SELECT e1.name AS Employee, e2.name AS Manager
FROM employees e1
JOIN employees e2
ON e1.manager_id = e2.id;
```

✅ **Result:**  
Shows employee and their corresponding manager names.

---

## 🔹 7️⃣ NATURAL JOIN

**Definition:** Automatically joins tables **on columns with the same name and datatype** (avoid using in production — not explicit).

✅ **Syntax:**

```sql
SELECT *
FROM employees
NATURAL JOIN departments;
```

---

## 🔹 8️⃣ Summary Table

|Type|Returns|Nulls for No Match|Common Use|
|---|---|---|---|
|**INNER JOIN**|Only matching rows|❌|Most common join|
|**LEFT JOIN**|All left + matching right|✅ on right|To keep all from left|
|**RIGHT JOIN**|All right + matching left|✅ on left|To keep all from right|
|**FULL JOIN**|All rows from both|✅ both sides|Combine two datasets|
|**CROSS JOIN**|Cartesian product|❌|All combinations|
|**SELF JOIN**|Same table twice|❌|Hierarchies (manager → employee)|
|**NATURAL JOIN**|Auto match same columns|✅|Quick but not recommended|

---

Would you like me to show this with **sample data and actual outputs** (so you can visualize each join’s result clearly)?