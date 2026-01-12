
Problem statement: Design core booking logic, availability management, notifications, and conflicts resolution.





**Appointment Booking System** is a **very common System Design interview question** (used by Google, Amazon, Flipkart, Practo, UrbanCompany, etc.).

I’ll explain it **step-by-step**, covering **HLD + LLD**, and I’ll also tell you **what to say in interview**.

---

# 🟢 **Appointment Booking System – HLD & LLD**

---

## 1️⃣ **Clarify Requirements (Always start here in interview)**

### ✅ Functional Requirements

- Users can **book appointments**
    
- Service providers (Doctor, Salon, Consultant) define **available slots**
    
- Prevent **double booking**
    
- Users can **cancel / reschedule**
    
- Appointment confirmation
    
- View appointments
    

### ✅ Non-Functional Requirements

- High availability
    
- Low latency
    
- Consistency is critical (no double booking)
    
- Scalable
    
- Fault tolerant

📌 _Important interview statement:_

> “This is a **write-heavy and consistency-sensitive** system.”

---

## 2️⃣ **High-Level Design (HLD)**

### 🏗 **Architecture**

```
Client (Web/Mobile)
        ↓
Load Balancer
        ↓
API Gateway
        ↓
Appointment Service
        ↓
Cache (Redis)
        ↓
Database (SQL)
        ↓
Notification Service
```

---

## 3️⃣ **Core Services**

|Service|Responsibility|
|---|---|
|User Service|User authentication|
|Provider Service|Doctors/Salons availability|
|Appointment Service|Booking logic|
|Notification Service|SMS/Email confirmations|

---

## 4️⃣ **Database Design (Very Important)**

### 🧾 **Users**

|Field|Type|
|---|---|
|user_id|UUID|
|name|String|

---

### 🧾 **Service Providers**

|Field|Type|
|---|---|
|provider_id|UUID|
|name|String|
|specialization|String|

---

### 🧾 **Time Slots**

|Field|Type|
|---|---|
|slot_id|UUID|
|provider_id|UUID|
|start_time|TIMESTAMP|
|end_time|TIMESTAMP|
|status|AVAILABLE / BOOKED|

🔒 **Unique constraint** on:

```
(provider_id, start_time)
```

---

### 🧾 **Appointments**

|Field|Type|
|---|---|
|appointment_id|UUID|
|user_id|UUID|
|provider_id|UUID|
|slot_id|UUID|
|status|CONFIRMED / CANCELLED|

---

## 5️⃣ **Booking Flow (Most Important)**

### 📌 Step-by-step:

1. User selects provider
    
2. System shows **available slots**
    
3. User selects slot
    
4. System checks availability
    
5. Locks slot (transaction)
    
6. Creates appointment
    
7. Marks slot as BOOKED
    
8. Sends notification
    

---

## 6️⃣ **Prevent Double Booking (Interview Favorite Question)**

### ✅ Option 1: Database Lock (Best Answer)

```sql
SELECT * FROM slots
WHERE slot_id = ?
FOR UPDATE;
```

✔ Only one transaction can book the slot

---

### ✅ Option 2: Optimistic Locking

- Version field
    
- Retry on conflict
    

---

### ✅ Option 3: Redis Lock (Distributed systems)

```
SET slotId userId NX EX 30
```

---

## 7️⃣ **High-Level API Design**

### 🔹 Get available slots

```
GET /providers/{id}/slots
```

### 🔹 Book appointment

```
POST /appointments
{
  "userId": "u1",
  "providerId": "p1",
  "slotId": "s1"
}
```

### 🔹 Cancel appointment

```
DELETE /appointments/{id}
```

---

## 8️⃣ **Low-Level Design (LLD)**

---
Perfect 👍  
Now I’ll give you **FULL LOW-LEVEL DESIGN (LLD)** for **ALL scenarios** of the **Appointment Booking System**, exactly in an **interview-ready**, **object-oriented**, **SOLID-aligned** way.

I’ll cover:

1. Core booking logic
2. Availability management
3. Conflict resolution
4. Cancellation
5. Rescheduling
6. Notifications
7. Failure handling
8. Class interactions

No HLD repetition — **pure LLD**.

---

# 1️⃣ Core Domain Models (Entities)

These are your **core business objects**.

```java
enum SlotStatus {
    AVAILABLE,
    LOCKED,
    BOOKED
}
```

```java
enum AppointmentStatus {
    CONFIRMED,
    CANCELLED,
    FAILED
}
```

```java
class Slot {
    String slotId;
    String providerId;
    LocalDateTime startTime;
    LocalDateTime endTime;
    SlotStatus status;
    int version; // for optimistic locking
}
```

```java
class Appointment {
    String appointmentId;
    String userId;
    String providerId;
    String slotId;
    AppointmentStatus status;
    LocalDateTime createdAt;
}
```

---

# 2️⃣ Repository Layer (Persistence Abstraction)

👉 Keeps business logic **DB-agnostic**

```java
public interface SlotRepository {
    Optional<Slot> findById(String slotId);
    List<Slot> findAvailableSlots(String providerId, LocalDate date);
    boolean lockSlot(String slotId);
    void markBooked(String slotId);
    void markAvailable(String slotId);
}
```

```java
public interface AppointmentRepository {
    void save(Appointment appointment);
    Optional<Appointment> findById(String appointmentId);
    void updateStatus(String appointmentId, AppointmentStatus status);
}
```

---

# 3️⃣ Locking Abstraction (Conflict Resolution)

This is **VERY IMPORTANT** for interviews.

```java
public interface SlotLockManager {
    boolean acquireLock(String slotId);
    void releaseLock(String slotId);
}
```

### Redis-based implementation (conceptual)

```java
public class RedisSlotLockManager implements SlotLockManager {

    @Override
    public boolean acquireLock(String slotId) {
        // SET slotId NX EX 30
        return true;
    }

    @Override
    public void releaseLock(String slotId) {
        // DEL slotId
    }
}
```

📌 **Why interface?**

> “So we can switch DB lock / Redis lock without touching business logic.”

---

# 4️⃣ Availability Management (LLD)

```java
public interface AvailabilityService {
    List<Slot> getAvailableSlots(String providerId, LocalDate date);
}
```

```java
public class AvailabilityServiceImpl implements AvailabilityService {

    private SlotRepository slotRepository;

    @Override
    public List<Slot> getAvailableSlots(String providerId, LocalDate date) {
        return slotRepository.findAvailableSlots(providerId, date);
    }
}
```

✔ Slots are **pre-generated**  
✔ Only `AVAILABLE` slots returned  
✔ Cache sits **outside** this layer

---

# 5️⃣ Core Booking Logic (MOST IMPORTANT CLASS)

```java
public interface AppointmentService {
    Appointment book(String userId, String providerId, String slotId);
    void cancel(String appointmentId);
    Appointment reschedule(String appointmentId, String newSlotId);
}
```

---

## Booking Implementation

```java
public class AppointmentServiceImpl implements AppointmentService {

    private SlotRepository slotRepo;
    private AppointmentRepository appointmentRepo;
    private SlotLockManager lockManager;
    private NotificationService notificationService;

    @Override
    public Appointment book(String userId, String providerId, String slotId) {

        // 1. Acquire distributed lock
        if (!lockManager.acquireLock(slotId)) {
            throw new RuntimeException("Slot already locked");
        }

        try {
            Slot slot = slotRepo.findById(slotId)
                    .orElseThrow(() -> new RuntimeException("Slot not found"));

            if (slot.getStatus() != SlotStatus.AVAILABLE) {
                throw new RuntimeException("Slot not available");
            }

            // 2. Mark slot as BOOKED
            slotRepo.markBooked(slotId);

            // 3. Create appointment
            Appointment appt = new Appointment();
            appt.setUserId(userId);
            appt.setProviderId(providerId);
            appt.setSlotId(slotId);
            appt.setStatus(AppointmentStatus.CONFIRMED);
            appt.setCreatedAt(LocalDateTime.now());

            appointmentRepo.save(appt);

            // 4. Async notification
            notificationService.sendBookingConfirmation(appt);

            return appt;

        } finally {
            // 5. Always release lock
            lockManager.releaseLock(slotId);
        }
    }
}
```

📌 **Interview gold line**:

> “Booking is atomic: lock → validate → book → notify → unlock.”

---

# 6️⃣ Cancellation (LLD)

```java
@Override
public void cancel(String appointmentId) {

    Appointment appt = appointmentRepo.findById(appointmentId)
            .orElseThrow(() -> new RuntimeException("Not found"));

    if (appt.getStatus() != AppointmentStatus.CONFIRMED) {
        throw new RuntimeException("Invalid state");
    }

    appointmentRepo.updateStatus(appointmentId, AppointmentStatus.CANCELLED);

    slotRepo.markAvailable(appt.getSlotId());

    notificationService.sendCancellation(appt);
}
```

✔ Slot is reusable  
✔ Cache invalidated  
✔ Notification async

---

# 7️⃣ Rescheduling (LLD)

### Strategy: **Cancel + Book (simpler & safer)**

```java
@Override
public Appointment reschedule(String appointmentId, String newSlotId) {

    Appointment old = appointmentRepo.findById(appointmentId)
            .orElseThrow(() -> new RuntimeException("Not found"));

    cancel(appointmentId);

    return book(old.getUserId(), old.getProviderId(), newSlotId);
}
```

📌 **Why interviewer likes this**:

- Less lock complexity
- Easier rollback
- Clear flow

---

# 8️⃣ Notification System (LLD)

```java
public interface NotificationService {
    void sendBookingConfirmation(Appointment appt);
    void sendCancellation(Appointment appt);
    void sendReminder(Appointment appt);
}
```

```java
public class NotificationServiceImpl implements NotificationService {

    @Override
    public void sendBookingConfirmation(Appointment appt) {
        // Publish to Kafka / SQS
    }

    @Override
    public void sendCancellation(Appointment appt) {
        // Publish event
    }

    @Override
    public void sendReminder(Appointment appt) {
        // Scheduled job
    }
}
```

✔ Async  
✔ Failure-tolerant  
✔ Non-blocking

---

# 9️⃣ Failure Scenarios (LLD Handling)

|Failure|Handling|
|---|---|
|Lock not acquired|Reject booking|
|DB failure|Rollback transaction|
|Service crash|Lock expires (TTL)|
|Notification fails|Retry via queue|
|Slot conflict|Retry / error|

---

# 🔟 Class Interaction Flow (Booking)

```
Controller
   ↓
AppointmentService
   ↓
SlotLockManager
   ↓
SlotRepository
   ↓
AppointmentRepository
   ↓
NotificationService (async)
```

---

# ✅ SOLID Principles Applied

|Principle|Where|
|---|---|
|SRP|Slot, Appointment, Notification|
|OCP|New lock strategies|
|LSP|Interface-based services|
|ISP|Small focused interfaces|
|DIP|Business logic depends on abstractions|

---

# 🎯 FINAL INTERVIEW SUMMARY (LLD)

> “The low-level design separates booking, availability, locking, and notification concerns. Slot locking ensures consistency, repositories abstract persistence, notifications are async, and rescheduling is handled via cancel-and-book. The design follows SOLID and supports scalability and fault tolerance.”

---

If you want next:

- ✅ UML Class Diagram
    
- ✅ Sequence Diagram (booking & conflict)
    
- ✅ Waitlist LLD
    
- ✅ Payment + refund LLD
    
- ✅ Mock interview cross-questions
    

Just say **next** 🚀
## Want Next?

I can now help you with:  
1️⃣ **Sequence Diagram explanation**  
2️⃣ **Concurrency deep dive**  
3️⃣ **Doctor booking vs Salon booking difference**  
4️⃣ **Mock interview Q&A**

Just tell me 👍




---
Appointment Booking

user (request for appointment)                   --            provider (they will accept the appointment)

user.java class
userId
userName
userEmail

Provider.java class
providerId
providerName (doctor)
ProviderEmail

Appointment.java class
appointmentId
status
userid
providerid
slotid
createdAt


Slot.java class
slotid
startTime
endTime
slotStatus


UserService.java class
addUser(user)
deleteUser(userId)
updateUser(user)

AppointmentService.java class
submitAppointment(Appointment)
cancelAppointment(appointmentid) // optional
updateAppointmentStatus(appointmentid, status)

SlotService.java class
asseginslot(appointmentid, userid)
updateSLot(appointmentid)
cancelslot(slotid)






