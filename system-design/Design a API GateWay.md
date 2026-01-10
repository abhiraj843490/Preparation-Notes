## 🔹 What is an API Gateway?

> An **API Gateway** is a single entry point for all client requests that routes traffic to backend microservices and applies cross-cutting concerns.
---

# 🧱 FINAL ARCHITECTURE

```
Client
 ↓
API Gateway
  ├─ Rate Limiter (IP based)
  ├─ Router
  ├─ Load Balancer
 ↓
Order Service (8081 / 8082)
```

---

# 📁 Project Structure

```
api-gateway/
 └── src/main/java/com/example/gateway
     ├── ApiGatewayApplication.java   ✅ MAIN
     ├── filter/ApiGatewayFilter.java
     ├── ratelimit/SlidingWindowRateLimiter.java
     ├── router/Router.java
     ├── lb/RoundRobinLoadBalancer.java
     ├── registry/ServiceRegistry.java
     └── model/ServiceInstance.java
```

---

# 🚀 1️⃣ MAIN CLASS

```java
package com.example.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ApiGatewayApplication {

    public static void main(String[] args) {
        SpringApplication.run(ApiGatewayApplication.class, args);
    }
}
```

**application.properties**

```properties
server.port=8080
```

---

# 🧩 2️⃣ ServiceInstance

```java
package com.example.gateway.model;

public class ServiceInstance {

    private String host;
    private int port;

    public ServiceInstance(String host, int port) {
        this.host = host;
        this.port = port;
    }

    public String getUrl() {
        return "http://" + host + ":" + port;
    }
}
```

---

# 📒 3️⃣ ServiceRegistry (In-Memory)

```java
package com.example.gateway.registry;

import com.example.gateway.model.ServiceInstance;
import jakarta.annotation.PostConstruct;
import org.springframework.stereotype.Component;

import java.util.*;
import java.util.concurrent.ConcurrentHashMap;

@Component
public class ServiceRegistry {

    private final Map<String, List<ServiceInstance>> services =
            new ConcurrentHashMap<>();

    @PostConstruct
    public void init() {
        register("order-service", new ServiceInstance("localhost", 8081));
        register("order-service", new ServiceInstance("localhost", 8082));
    }

    public void register(String serviceName, ServiceInstance instance) {
        services.computeIfAbsent(serviceName, k -> new ArrayList<>())
                .add(instance);
    }

    public List<ServiceInstance> getInstances(String serviceName) {
        return services.get(serviceName);
    }
}
```

🧠 _Production → Eureka / Kubernetes DNS_

---

# 🔁 4️⃣ Load Balancer (Round Robin)

```java
package com.example.gateway.lb;

import com.example.gateway.model.ServiceInstance;
import org.springframework.stereotype.Component;

import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

@Component
public class RoundRobinLoadBalancer {

    private final AtomicInteger index = new AtomicInteger(0);

    public ServiceInstance choose(List<ServiceInstance> instances) {
        int i = Math.abs(index.getAndIncrement() % instances.size());
        return instances.get(i);
    }
}
```

---

# 🧭 5️⃣ Router

```java
package com.example.gateway.router;

import com.example.gateway.lb.RoundRobinLoadBalancer;
import com.example.gateway.model.ServiceInstance;
import com.example.gateway.registry.ServiceRegistry;
import org.springframework.stereotype.Component;

@Component
public class Router {

    private final ServiceRegistry registry;
    private final RoundRobinLoadBalancer lb;

    public Router(ServiceRegistry registry, RoundRobinLoadBalancer lb) {
        this.registry = registry;
        this.lb = lb;
    }

    public ServiceInstance route(String path) {
        if (path.startsWith("/orders")) {
            return lb.choose(registry.getInstances("order-service"));
        }
        throw new RuntimeException("No route found");
    }
}
```

---

# ⏱️ 6️⃣ Sliding Window Rate Limiter (IP-based)

```java
package com.example.gateway.ratelimit;

import org.springframework.stereotype.Component;

import java.util.*;
import java.util.concurrent.ConcurrentHashMap;

@Component
public class SlidingWindowRateLimiter {

    private final int LIMIT = 5;          // 5 requests
    private final long WINDOW = 60_000;   // per minute

// Rate Limiting date we are storing
    private final Map<String, Deque<Long>> store =
            new ConcurrentHashMap<>();
            
            /*store = {
			  "192.168.1.10" → [1000, 2000, 3000]
			  "192.168.1.20" → [1500, 2500]
			}
			*/

    public synchronized boolean allow(String ip) {
        long now = System.currentTimeMillis();
        store.putIfAbsent(ip, new ArrayDeque<>());

        Deque<Long> q = store.get(ip);

        while (!q.isEmpty() && q.peekFirst() <= now - WINDOW) {
            q.pollFirst();
        }

        if (q.size() < LIMIT) {
            q.addLast(now);
            return true;
        }
        return false;
    }
}
```

---

# 🚦 7️⃣ API Gateway Filter (CORE ENTRY POINT)

```java
package com.example.gateway.filter;

import com.example.gateway.model.ServiceInstance;
import com.example.gateway.ratelimit.SlidingWindowRateLimiter;
import com.example.gateway.router.Router;
import jakarta.servlet.*;
import jakarta.servlet.http.*;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import java.io.IOException;

@Component
public class ApiGatewayFilter extends OncePerRequestFilter {

    private final SlidingWindowRateLimiter limiter;
    private final Router router;

    public ApiGatewayFilter(SlidingWindowRateLimiter limiter,
                            Router router) {
        this.limiter = limiter;
        this.router = router;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain chain)
            throws ServletException, IOException {

        String clientIp = request.getRemoteAddr();

        // Rate limiting
        if (!limiter.allow(clientIp)) {
            response.setStatus(429);
            response.getWriter().write("Too Many Requests");
            return;
        }

        // Routing
        ServiceInstance instance = router.route(request.getRequestURI());

        response.getWriter().write(
                "Request routed to → " + instance.getUrl()
        );
    }
}
```

---

# 🧪 8️⃣ Backend Order Service (Simple)

Create **separate Spring Boot app**:

```java
@SpringBootApplication
@RestController
public class OrderServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }

    @GetMapping("/orders")
    public String orders() {
        return "Order Service running on port " +
                System.getProperty("server.port");
    }
}
```

Run twice:

```properties
server.port=8081
server.port=8082
```

---

# ▶️ TEST

```bash
curl http://localhost:8080/orders
```

Output:

```
Request routed to → http://localhost:8081
Request routed to → http://localhost:8082
```

✔ Round Robin  
✔ IP-based rate limiting

---

# 🎯 INTERVIEW SUMMARY LINE

> “I implemented an API Gateway without authentication for public APIs. It uses IP-based sliding window rate limiting, path-based routing, and round-robin load balancing. The gateway is stateless and horizontally scalable.”

---

# 🔥 Interview Follow-up Questions (Be Ready)

1. Where do you store rate limit data?: concurrent HashMap
2. How do you handle retries? based on the window of time
3. Gateway vs Service Mesh?
4. Gateway bottleneck?
5. Auth at gateway or service?

---
## 🔹 Data Structures Used (Must Remember)

|Feature|Data Structure|
|---|---|
|Routing|HashMap<Path, Service>|
|Load Balancer|List + AtomicInteger|
|Rate Limiting|Deque / Redis ZSET|
|Service Registry|ConcurrentHashMap|
|Caching|LRU (LinkedHashMap)|
