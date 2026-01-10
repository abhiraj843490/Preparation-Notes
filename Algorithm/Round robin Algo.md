Round Robin is one of the **simplest and most popular load-balancing algorithms** used to distribute requests across multiple servers.

---

# ✅ **Round Robin Algorithm (Simple Explanation)**

Round Robin means:

👉 **Send the first request to Server 1**  
👉 **Next request to Server 2**  
👉 **Next request to Server 3**  
👉 …and after the last server, start again from Server 1

```
Request 1 → S1  
Request 2 → S2  
Request 3 → S3  
Request 4 → S1  
Request 5 → S2  
...
```

It is like dealing cards in a circle — everyone gets a turn equally.

---

# 🧠 **Why is it used?**

- Very simple
- Fast
- Equal traffic distribution
- No server preference

---

# ⚙️ **Where is it used?**

Used in:
- Load balancers (NGINX, HAProxy, AWS ELB)
- Distributed systems
- OS process scheduling

---

# ⭐ **Real Example:**

Suppose you have 3 backend servers:

```
S1 = server1.com  
S2 = server2.com  
S3 = server3.com
```

Incoming 7 requests will go as:

```
1 → S1  
2 → S2  
3 → S3  
4 → S1  
5 → S2  
6 → S3  
7 → S1  
```

---

# 🧩 **Where round robin fails?**

Round Robin **does NOT consider server load**.

Example:

- S1 has 100 requests already
- S2 has 2 requests only

Round Robin still sends the next request to **S1** → unfair.

That’s why we use:

- **Weighted Round Robin**
- **Least Connections**

---

# 🧑‍💻 **One-line Java implementation idea**

```java
int index = (index + 1) % servers.size();
return servers.get(index);
```

---
### ⭐ Weighted Round Robin

### ⭐ Least Connection Algorithm

### ⭐ LLD for Round Robin

### ⭐ When to use which algorithm
