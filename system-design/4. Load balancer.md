


                              Load-balancer
                                  /    \
                services:        A       B
    problem statements:
    1. design custom service for loadbalancer
    2. design a application from there is going to communicate with load-balancer
    3. design how will route on diff pods of specific service

# ⭐ **1. What is a Load Balancer?**

_A Load Balancer distributes incoming traffic across multiple backend servers to ensure reliability, fault tolerance, and high availability._

---

# ⭐ **2. Functional Requirements**

1. Distribute requests across servers.
2. Only route to healthy servers.
3. Support load balancing algorithms:
    - Round Robin
    - Weighted Round Robin
    - Least Connections
        
4. Auto-remove failed servers.
5. Add servers dynamically.
---

# ⭐ **3. Non-Functional Requirements**

- High availability
- Horizontal scalability
- Low latency
- Fault tolerance
- Stateless if possible

---

# ⭐ **4. Architecture**

```
Client → LoadBalancer → Server Pool (S1, S2, S3…)
                ↑
            Health Checker
```

---

# ⭐ **5. Key Interview Scenarios**

## **Scenario 1: Server Failure Detection**

Load Balancer performs health checks:

```
GET /health
200 → OK
Timeout/500 → Unhealthy
```

Unhealthy servers removed from routing.

---

## **Scenario 2: New Server Joins**

Load Balancer should update server list dynamically:

```
addServer(ip)
removeServer(ip)
```

---

## **Scenario 3: Consistent Load Distribution**

Algorithms:

### ✔ Round Robin
### ✔ Weighted Round Robin
### ✔ Least Connections
### ✔ IP Hash (sticky session)

---

## **Scenario 4: Sticky Sessions**

Used for:

- Shopping cart
- Login session

Method:

- IP Hash
- Cookie-based routing

---

## **Scenario 5: Scaling the Load Balancer Itself**

Load Balancer is a single point of failure → solve using:

### Active-Passive Load Balancer

```
Load Balancer 1 (Active)
Load Balancer 2 (Passive, via keepalived)
```

### Active-Active LB

Use **DNS Load Balancing** or **Anycast IP**.

---

## **Scenario 6: Retry on Failure**

If server fails while sending request:

```
Retry on next healthy server
```

---

## **Scenario 7: SSL Termination**

LB decrypts SSL → backend receives HTTP.

---

# ⭐ **6. LOW LEVEL DESIGN 

```java
public class Main {
    public static void main(String[] args) {

        LoadBalancer lb = new RoundRobinLoadBalancer();

        // adding your 2 services A & B
        lb.addServer("Service-A");
        lb.addServer("Service-B");

        RequestDispatcher dispatcher = new RequestDispatcher(lb);

        // simulate 5 client requests
        dispatcher.process("Request-1");
        dispatcher.process("Request-2");
        dispatcher.process("Request-3");
        dispatcher.process("Request-4");
        dispatcher.process("Request-5");
    }
}

```

---
👇 **Request Handler**  
```java
public class RequestDispatcher {

    private final LoadBalancer lb;

    public RequestDispatcher(LoadBalancer lb) {
        this.lb = lb;
    }

    public void process(String request) {
        String server = lb.getNextServer();
        System.out.println("Routing request: " + request + " → " + server);
    }
}

```
# ✔ **LLD: Interface Design**

```java
interface LoadBalancer {
    String getNextServer();
    void addServer(String server);
    void removeServer(String server);
}
```

---

# ✔ **Round Robin Implementation**

```java
class RoundRobinLoadBalancer implements LoadBalancer {
    private final List<String> servers;
    private int index;

    public RoundRobinLoadBalancer(List<String> servers) {
        this.servers = new ArrayList<>(servers);
        this.index = 0;
    }

    @Override
    public synchronized String getNextServer() {
        if (servers.isEmpty()) return null;

        String server = servers.get(index);
        index = (index + 1) % servers.size();
        return server;
    }

    @Override
    public synchronized void addServer(String server) {
        servers.add(server);
    }

    @Override
    public synchronized void removeServer(String server) {
        servers.remove(server);
    }
}
```

---

# ✔ **LLD: Weighted Round Robin**

```java
class WeightedRoundRobin {
    private static class Server {
        String ip;
        int weight;

        Server(String ip, int weight) {
            this.ip = ip;
            this.weight = weight;
        }
    }

    private final List<Server> pool = new ArrayList<>();
    private int index = 0;

    public void addServer(String ip, int weight) {
        pool.add(new Server(ip, weight));
    }

    public String getNextServer() {
        if (pool.isEmpty()) return null;

        int totalWeight = pool.stream().mapToInt(s -> s.weight).sum();
        int pointer = index % totalWeight;
        index++;

        for (Server s : pool) {
            if (pointer < s.weight) return s.ip;
            pointer -= s.weight;
        }
        return null;
    }
}
```

---

# ✔ **LLD: Least Connections Algorithm**

```java
class LeastConnectionLB {
    private Map<String, Integer> serverConnections = new HashMap<>();

    public void addServer(String ip) {
        serverConnections.put(ip, 0);
    }

    public String getNextServer() {
        return serverConnections.entrySet()
            .stream()
            .min(Map.Entry.comparingByValue())
            .get().getKey();
    }

    public void onRequestStart(String server) {
        serverConnections.put(server, serverConnections.get(server) + 1);
    }

    public void onRequestEnd(String server) {
        serverConnections.put(server, serverConnections.get(server) - 1);
    }
}
```

---

# ✔ **LLD: Health Checker (Thread-based)**

```java
class HealthChecker implements Runnable {

    private final List<String> servers;
    private final LoadBalancer lb;

    public HealthChecker(List<String> servers, LoadBalancer lb) {
        this.servers = servers;
        this.lb = lb;
    }

    @Override
    public void run() {
        while (true) {
            for (String server : servers) {
                if (isHealthy(server)) {
                    lb.addServer(server);
                } else {
                    lb.removeServer(server);
                }
            }

            try { Thread.sleep(3000); } 
            catch (InterruptedException e) {}
        }
    }

    private boolean isHealthy(String server) {
        // ping server: GET /health
        return true; // simplify
    }
}
```

---

# ⭐ **7. End-to-End Flow (Explain to Interviewer)**

### Client Request →

### Load Balancer →

1. Choose server based on algorithm
2. Check if server healthy
3. Forward request
4. If server fails → retry on next server

---

# ⭐ **4. Real API Example with Code

### Service A API


```java 
@RestController public class UserController {      
	@GetMapping("/users")     
	public String getUsers() {
         return "Response from Service A";
	} }
```

### Service B API

```java 
@RestController public class UserController {
      @GetMapping("/users")     
      public String getUsers() {         
	      return "Response from Service B";     
    } }
```

Both services expose the same REST API.

---

# ⭐ **5. Load Balancer Logic**

```java 
public class ApiGateway {

    private LoadBalancer lb;

    public ApiGateway(LoadBalancer lb) {
        this.lb = lb;
    }

    public String callBackend(String apiPath) {
        String backendServer = lb.getNextServer();
        String url = backendServer + apiPath;

        // You would make HTTP call here
        return "Forwarding to: " + url;
    }
}
```

---

# ⭐ **6. How API Gateway Calls It**

```java 
public class Main {
    public static void main(String[] args) {

        LoadBalancer lb = new RoundRobinLoadBalancer();
        lb.addServer("http://10.0.0.1:8080");
        lb.addServer("http://10.0.0.2:8080");

        ApiGateway apiGateway = new ApiGateway(lb);

        // Simulate client API calls
        System.out.println(apiGateway.callBackend("/users"));
        System.out.println(apiGateway.callBackend("/users"));
        System.out.println(apiGateway.callBackend("/users"));
    }
}

```

---

If you want, we can continue to:

### 📌 API Gateway vs Load Balancer

### 📌 Rate Limiter design

### 📌 Cache design (Redis)

### 📌 URL Shortener system design

### 📌 Consistent hashing (very important)

Just tell me **“next topic”**.