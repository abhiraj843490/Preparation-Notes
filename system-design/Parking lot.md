Fantastic — we’ll start **from the very beginning of Phase 2 (LLD)** and work through real interview-style design tasks, patterns, UML, trade-offs, and lots of cross-questions. I’ll lead with a clear plan and we’ll immediately jump into our **first LLD exercise** (classic, practical, pattern-rich): **Design a Parking Lot**.

# Plan for Phase 2 (Low-Level Design)

1. Quick LLD checklist & interview approach.
    
2. Solve 1 problem at a time—start with **Parking Lot**.
    
3. For each problem we’ll cover:
    
    - Requirements & assumptions (explicitly stated)
    - Core classes & interfaces (UML-like text)
    - Key methods & attributes
    - Which design patterns to use and why
    - Sequence of operations (textual sequence diagram)
    - Edge cases, testing ideas, extensions
    - Interview-style cross-questions and answers
4. Repeat practice problems (e.g., ATM, Coffee Machine, Logger) after parking lot.
    

If that plan looks good (I’ll assume yes), we’ll start right away.

# LLD Interview Checklist (how to approach in 2–4 min)

- Ask / state **requirements** (must-haves vs optional). If interviewer silent, state your assumptions.
    
- Identify **main actors** (user, system).
- Identify **core entities** (classes).
- Sketch **interfaces** and relationships (composition, aggregation, inheritance).
- Pick relevant **design patterns** and justify them.
- Show **one flow** with a sequence of calls.
- Discuss **edge cases**, **scalability**, and **how to extend**.

---

# Problem 1 — Design a Parking Lot (Interview-style)

## 1) Problem Statement (minimal)

Design a parking lot system that supports:

- Parking vehicles of different sizes (motorbike, car, truck).
- Multiple levels, each with multiple rows & spots.
- Assign the nearest suitable parking spot when a vehicle arrives.
- Release spot when vehicle leaves.
- Get status: free spots, occupied spots, locations.
- Track ticket with entry time to compute cost.

## 2) Assumptions (state explicitly)

- Spots types: `Motorbike`, `Compact`, `Large`.
- Vehicles: `Motorbike`, `Car`, `Truck` — each needs specific spot type or larger.
- One entrance/exit for simplicity (we can extend later).
- Pricing: simple per-hour rate by vehicle type.
- Single-process in-memory model (no distributed concerns). Later we can discuss scaling, persistence.
- Concurrency: basic locking assumed for spot assignment; we’ll mention thread-safety.
    

## 3) High-level classes / interfaces (UML-like text)

- `ParkingLot` — top-level manager (Singleton)
    
    - attributes: `List<Level> levels`
    - methods: `parkVehicle(Vehicle)`, `unparkVehicle(Ticket)`, `getStatus()`
        
- `Level`
    
    - attributes: `int levelNumber`, `List<ParkingSpot> spots`
        
    - methods: `parkVehicle(Vehicle)`, `freeSpot(spotId)`, `getAvailableSpots()`
        
- `ParkingSpot` (abstract or base)
    
    - attributes: `String id`, `SpotType type`, `boolean isOccupied`, `Vehicle currentVehicle`
        
    - methods: `canFitVehicle(Vehicle)`, `park(Vehicle)`, `leave()`
        
- `Vehicle` (abstract)
    
    - attributes: `String licensePlate`, `VehicleSize size`
        
    - subclasses: `Motorbike`, `Car`, `Truck`
        
- `Ticket`
    
    - attributes: `String ticketId`, `String spotId`, `LocalDateTime entryTime`, `Vehicle vehicle`
    
- `PaymentService` (interface)
    
    - method: `double calculateFee(Ticket ticket, LocalDateTime exitTime)`
        
- `ParkingStrategy` (interface) — strategy to choose spot (nearest, compact-first, etc.)
    
    - method: `ParkingSpot selectSpot(Level level, Vehicle vehicle)`
        
- `ParkingSpotFactory` (Factory) — create spots by type (optional for initialization)
    

## 4) Key design patterns & rationale

- **Singleton** for `ParkingLot` (single point-of-control). Could also avoid explicit singleton and use DI.
    
- **Factory** for creating `ParkingSpot` objects during initialization (keeps creation logic centralized).
    
- **Strategy** for spot-selection algorithms (nearest, fill-level-first) — allows changing allocation policy at runtime.
    
- **Observer** (optional) for notifying subscribers when levels fill up (e.g., UI/dashboard).
    
- **Template Method** (optional) for parking fee calculation variations per vehicle type (or use Strategy for pricing).
    
- **Interface Segregation** — expose minimal public APIs.
    

## 5) Core method behaviors (pseudologic)

`parkVehicle(Vehicle v)`:

- For each level (or based on strategy):
    
    - Use `ParkingStrategy.selectSpot(level, v)` to find a spot.
        
    - If found:
        
        - mark `spot.isOccupied = true`, `spot.currentVehicle = v`
            
        - create `Ticket` with `entryTime = now`
            
        - return ticket
            
- If no spot -> return "ParkingFull" or null.
    

`unparkVehicle(Ticket t)`:

- Lookup spot by `t.spotId`
    
- Compute fee via `PaymentService.calculateFee(t, now)`
    
- Free spot: `spot.leave()` -> clear occupant
    
- Return fee and receipt
    

## 6) Sequence for parking a car (textual)

1. `User` arrives with `Vehicle v`.
    
2. `ParkingLot.parkVehicle(v)` called.
    
3. `ParkingLot` picks a level (maybe first with availability).
    
4. `ParkingStrategy.selectSpot(level, v)` returns `ParkingSpot s`.
    
5. `s.park(v)` sets occupied.
    
6. `Ticket t = TicketService.generateTicket(s.id, v, now)`
    
7. Return `t` to user.
    

## 7) Thread-safety and concurrency

- Use synchronized blocks or locks when marking a spot occupied (atomic check-and-set).
    
- At level or spot granularity: `ReentrantLock` per spot or `Concurrent` data structures for free-spot lists to avoid contention.
    

## 8) Extensions & scalability (interviewer will ask)

- Persist tickets & events to DB for recovery. Use ACID or transactional writes when marking spot occupied.
    
- For multiple entrances, maintain a shared consistent view (use DB or distributed cache like Redis).
    
- For very large lots, maintain free spot min-heaps or indexed sets per spot type to quickly find nearest spot — O(log N).
    
- Add sensors: use events pushed to system to update state (event-driven).
    
- Expose REST APIs, add monitoring, auth, metrics.
    

## 9) Example Java interfaces/classes (short)

```java
public enum SpotType { MOTORBIKE, COMPACT, LARGE }
public enum VehicleSize { SMALL, MEDIUM, LARGE }

public abstract class Vehicle {
    private String plate;
    abstract VehicleSize getSize();
}

public class ParkingSpot {
    private String id;
    private SpotType type;
    private volatile boolean occupied;
    private Vehicle vehicle;
    public synchronized boolean park(Vehicle v) {
        if (occupied || !canFit(v)) return false;
        occupied = true; vehicle = v; return true;
    }
    public synchronized void leave() { occupied = false; vehicle = null; }
    public boolean canFit(Vehicle v) { /* map vehicle size to spot types */ }
}

public interface ParkingStrategy {
    ParkingSpot selectSpot(Level level, Vehicle v);
}

public class NearestFirstStrategy implements ParkingStrategy {
    public ParkingSpot selectSpot(Level level, Vehicle v) { /* scan or use a priority queue */ }
}
```

## 10) Interview-style cross-questions (with short answers)

Q: Why use Strategy for spot selection?  
A: To decouple allocation policy from parking logic — allows swapping algorithms (nearest, first-fit) without changing core code.

Q: Why not store a boolean array of spots?  
A: You can, but for quick lookup by type and nearest position, maintain separate structures (e.g., `TreeSet` or min-heap keyed by proximity) per spot type. Simpler arrays are OK for small lot.

Q: How do you ensure uniqueness of ticket IDs?  
A: Use UUIDs or a DB sequence; for distributed system, use GUIDs or time+node ID scheme.

Q: How to compute fee for partial hours?  
A: Use `Duration` between entry and exit, apply rounding rules or minimum charge via `PaymentService`.

Q: How to handle lost tickets?  
A: Business rule: charge max fee or lookup entry via camera/license-plate recognition (requires storage of plate->ticket map).

Q: How to scale this design to multiple parking lots (cities)?  
A: Each `ParkingLot` instance is independent; maintain service registry; use centralized DB or distributed cache for aggregated queries.

---


