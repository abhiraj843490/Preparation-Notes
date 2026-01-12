
design db schema
classes
how microservices will be designed
write APIs

Q. queries based schema design
Q. which data structure will use to implement autocomplete search functionality if we need to search restaurant with location.



Q. DNS, API Gateway
Q. Trie DATA structure


# 🍽️ Restaurant Ordering System – HLD & LLD

---

## 1️⃣ Clarify Requirements (Always First in Interview)

### ✅ Functional Requirements

- Customers can browse menu
    
- Add items to cart
    
- Place order
    
- Restaurant accepts / rejects order
    
- Order status tracking
    
- Payment support (optional)
    
- Order cancellation (before preparation)
### ✅ Non-Functional Requirements

- High availability
    
- Low latency
    
- Consistency (no duplicate orders)
    
- Scalability
    
- Fault tolerance

📌 Interview line:

> “This is a read-heavy system with critical write consistency during order placement.”

---

# 🏗️ 2️⃣ High-Level Design (HLD)

---

## 🔹 Architecture

```
Client (Web / Mobile)
        ↓
Load Balancer
        ↓
API Gateway
        ↓
Order Management Service
        ↓
┌──────────────┬──────────────┬───────────────┐
│ Menu Service │ Payment Svc  │ Notification  │
└──────────────┴──────────────┴───────────────┘
        ↓
Database + Cache
```

---

## 🔹 Core Services

|Service|Responsibility|
|---|---|
|User Service|Customer authentication|
|Menu Service|Menu & item management|
|Order Service|Order lifecycle|
|Payment Service|Payment processing|
|Notification Service|Order updates|

---

# 🗂️ 3️⃣ Database Design (HLD Level)

---

### 🧾 Users

|Field|Type|
|---|---|
|user_id|UUID|
|name|String|

---

### 🧾 Menu Items

|Field|Type|
|---|---|
|item_id|UUID|
|name|String|
|price|Decimal|
|availability|Boolean|

---

### 🧾 Orders

|Field|Type|
|---|---|
|order_id|UUID|
|user_id|UUID|
|status|CREATED / CONFIRMED / PREPARING / DELIVERED / CANCELLED|
|total_price|Decimal|

---

### 🧾 Order Items

|Field|Type|
|---|---|
|order_item_id|UUID|
|order_id|UUID|
|item_id|UUID|
|quantity|Int|

---

# 🔁 4️⃣ Order Flow (HLD)

---

### 🟢 Place Order

1. User selects items
    
2. User places order
    
3. System validates menu availability
    
4. Creates order
    
5. Payment processed
    
6. Restaurant notified
    

---

### 🔄 Order Status Flow

```
CREATED → CONFIRMED → PREPARING → READY → DELIVERED
              ↓
          CANCELLED
```

---

# 🧠 Interview Tip

> “Order creation and payment should be atomic to avoid ghost orders.”

---

# ⚙️ 5️⃣ Low-Level Design (LLD)

Now let’s go **deep into classes, interfaces, and logic**.

---

## 🧩 Core Enums

```java
enum OrderStatus {
    CREATED,
    CONFIRMED,
    PREPARING,
    READY,
    DELIVERED,
    CANCELLED
}
```

---

## 🧩 Domain Models

```java
class MenuItem {
    String itemId;
    String name;
    double price;
    boolean available;
}
```

```java
class OrderItem {
    String itemId;
    int quantity;
}
```

```java
class Order {
    String orderId;
    String userId;
    List<OrderItem> items;
    OrderStatus status;
    double totalAmount;
}
```

---

## 🧩 Repository Interfaces

```java
public interface OrderRepository {
    void save(Order order);
    Optional<Order> findById(String orderId);
    void updateStatus(String orderId, OrderStatus status);
}
```

```java
public interface MenuRepository {
    MenuItem findItem(String itemId);
}
```

---

## 🧩 Service Interfaces

```java
public interface OrderService {
    Order placeOrder(String userId, List<OrderItem> items);
    void cancelOrder(String orderId);
    void updateOrderStatus(String orderId, OrderStatus status);
}
```

```java
public interface PaymentService {
    boolean processPayment(String orderId, double amount);
}
```

```java
public interface NotificationService {
    void notifyUser(String userId, String message);
    void notifyRestaurant(String orderId);
}
```

---

## 🧩 Order Service Implementation (CORE LOGIC)

```java
public class OrderServiceImpl implements OrderService {

    private OrderRepository orderRepo;
    private MenuRepository menuRepo;
    private PaymentService paymentService;
    private NotificationService notificationService;

    @Override
    public Order placeOrder(String userId, List<OrderItem> items) {

        double total = 0;

        for (OrderItem item : items) {
            MenuItem menuItem = menuRepo.findItem(item.getItemId());
            if (!menuItem.isAvailable()) {
                throw new RuntimeException("Item unavailable");
            }
            total += menuItem.getPrice() * item.getQuantity();
        }

        Order order = new Order();
        order.setUserId(userId);
        order.setItems(items);
        order.setTotalAmount(total);
        order.setStatus(OrderStatus.CREATED);

        orderRepo.save(order);

        boolean paymentSuccess = paymentService.processPayment(order.getOrderId(), total);

        if (!paymentSuccess) {
            orderRepo.updateStatus(order.getOrderId(), OrderStatus.CANCELLED);
            throw new RuntimeException("Payment failed");
        }

        orderRepo.updateStatus(order.getOrderId(), OrderStatus.CONFIRMED);
        notificationService.notifyRestaurant(order.getOrderId());

        return order;
    }
}
```

---

## 🛑 Cancellation Logic

```java
@Override
public void cancelOrder(String orderId) {
    Order order = orderRepo.findById(orderId)
            .orElseThrow(() -> new RuntimeException("Order not found"));

    if (order.getStatus() == OrderStatus.PREPARING) {
        throw new RuntimeException("Too late to cancel");
    }

    orderRepo.updateStatus(orderId, OrderStatus.CANCELLED);
    notificationService.notifyUser(order.getUserId(), "Order cancelled");
}
```

---

# 🔔 6️⃣ Notifications (LLD)

- Order placed
    
- Order accepted
    
- Order ready
    
- Order delivered
    

✔ Async via Kafka / SQS  
✔ Retries on failure

---

# ⚠️ 7️⃣ Failure & Edge Case Handling

|Scenario|Handling|
|---|---|
|Item goes out of stock|Reject order|
|Payment failure|Cancel order|
|Duplicate order|Idempotency key|
|Restaurant offline|Queue orders|

---

# 📈 8️⃣ Scalability & Performance

- Cache menu in Redis
    
- Partition orders by restaurantId
    
- Async notifications
    
- Circuit breaker on payment
    

---

# 🎯 Final Interview Summary (Say This)

> “The restaurant ordering system separates menu browsing, order placement, payment, and notifications. Orders follow a clear lifecycle, and consistency is ensured during payment and order creation. The system is scalable using stateless services, caching, and asynchronous messaging.”

---

## Want to go further?

I can next help with:

- 🔥 Delivery assignment system
    
- 🔥 Kitchen display system
    
- 🔥 Payment retry & refund
    
- 🔥 UML diagrams
    
- 🔥 Mock interview Q&A
    

Just say **next** 🚀