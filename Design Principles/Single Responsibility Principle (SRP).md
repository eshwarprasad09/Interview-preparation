Great choice, Eshwar! Let's deep-dive into the **Single Responsibility Principle (SRP)** — the **first and most foundational design principle** in SOLID.

SRP is **heavily used in supply chain systems**, microservices, and any **high-traffic enterprise-grade architecture**, especially in domains like **order processing, inventory, shipping**, and **notifications**.

---

# ✅ Single Responsibility Principle (SRP) – Complete Guide

---

## 💡 What is SRP?

> **A class should have only one reason to change.**

This means:

* A class should do **only one thing** (i.e., have one job).
* That job should be **encapsulated** fully in that class.
* **Changes to that responsibility** should affect only that class.

---

## 🧠 Why It Matters in Supply Chain Systems

* You avoid **God classes** like `OrderService` doing 20 things.
* You improve **maintainability** in fast-moving teams.
* You make your system **modular and testable**.
* Helps **scale** each responsibility as an independent service or module.

---

## 🔥 Real-World Problem (Anti-Pattern)

```java
public class OrderService {
    public void placeOrder(Order order) {
        validate(order);                   // ✅ Business rule
        calculateDiscount(order);         // 🧮 Pricing logic
        sendConfirmationEmail(order);     // ✉️ Communication
        updateInventory(order);           // 🏬 Inventory management
        saveToDatabase(order);            // 💾 Persistence logic
    }
}
```

### ❌ What's Wrong Here?

* **Too many responsibilities** in one class:

    * Order validation
    * Discount calculation
    * Email notification
    * Inventory update
    * Database operation
* Any change in one responsibility will **affect** others
* Makes code **hard to test**, **debug**, **maintain**, **scale**

---

## ✅ SRP Applied – Refactored Design

Break the `OrderService` into **dedicated components**, each having a **single responsibility**:

```java
public class OrderValidator {
    public void validate(Order order) { ... }
}

public class DiscountCalculator {
    public double calculate(Order order) { ... }
}

public class NotificationService {
    public void sendEmail(Order order) { ... }
}

public class InventoryService {
    public void update(Order order) { ... }
}

public class OrderRepository {
    public void save(Order order) { ... }
}
```

Now the `OrderService` becomes a **thin coordinator**:

```java
public class OrderService {
    private OrderValidator validator;
    private DiscountCalculator discountCalculator;
    private NotificationService notifier;
    private InventoryService inventoryService;
    private OrderRepository orderRepository;

    public void placeOrder(Order order) {
        validator.validate(order);
        double finalPrice = discountCalculator.calculate(order);
        inventoryService.update(order);
        orderRepository.save(order);
        notifier.sendEmail(order);
    }
}
```

✅ Now each class has **only one reason to change**
✅ Changes in one component **don’t affect others**
✅ Makes testing, scaling, and maintaining much easier

---

## 🛒 Real-World Supply Chain Use Cases of SRP

| Component             | Responsibility                         |
| --------------------- | -------------------------------------- |
| `OrderValidator`      | Validate payment, address, stock, etc. |
| `InventoryManager`    | Update warehouse levels                |
| `ShipmentSelector`    | Choose best shipping vendor            |
| `NotificationService` | Send updates via email/SMS             |
| `InvoiceGenerator`    | Generate invoices after order success  |
| `AuditLogger`         | Log user/system actions separately     |

---

## 🧩 LLD Problem: Apply SRP to a Shipment Processor

### ❓ Problem

You are asked to design a **ShipmentProcessor** that:

1. Verifies delivery address
2. Calculates delivery cost
3. Assigns courier
4. Logs the process
5. Persists shipment details

---

### ❌ Bad Code (Violation of SRP)

```java
public class ShipmentProcessor {
    public void process(Shipment s) {
        if (!verifyAddress(s)) throw new RuntimeException("Invalid address");
        double cost = calculateCost(s);
        String courier = assignCourier(s);
        log("Shipment ready for courier: " + courier);
        saveToDB(s);
    }
}
```

---

### ✅ SRP Refactored Version

```java
public class AddressVerifier {
    public boolean verify(Shipment s) { ... }
}

public class DeliveryCostCalculator {
    public double calculate(Shipment s) { ... }
}

public class CourierAssigner {
    public String assign(Shipment s) { ... }
}

public class ShipmentLogger {
    public void log(String msg) { ... }
}

public class ShipmentRepository {
    public void save(Shipment s) { ... }
}

public class ShipmentProcessor {
    private AddressVerifier verifier;
    private DeliveryCostCalculator costCalculator;
    private CourierAssigner assigner;
    private ShipmentLogger logger;
    private ShipmentRepository repository;

    public void process(Shipment s) {
        if (!verifier.verify(s)) throw new RuntimeException("Invalid address");
        double cost = costCalculator.calculate(s);
        String courier = assigner.assign(s);
        logger.log("Assigned to courier: " + courier);
        repository.save(s);
    }
}
```

✅ Now each class has **one role**
✅ You can write **unit tests** easily
✅ Makes each piece **replaceable, extensible**

---

## 📌 When to Apply SRP – Recognition Clues

| Symptom                                             | SRP Fix                            |
| --------------------------------------------------- | ---------------------------------- |
| Class has **many unrelated methods**                | Break into separate classes        |
| One method spans multiple **concerns/domains**      | Extract responsibilities           |
| Change in one logic affects unrelated functionality | Split and isolate logic            |
| Difficult to write unit tests or mock dependencies  | Introduce modular responsibilities |

---

## 🧾 Single Responsibility Principle – Interview Cheat Sheet

| Area                | Summary                                                       |
| ------------------- | ------------------------------------------------------------- |
| 🔧 Definition       | A class should have one reason to change                      |
| 💡 Why              | Maintainability, testability, scalability                     |
| 🧱 LLD Application  | Split OrderService, ShipmentProcessor, InvoiceService, etc.   |
| 🔥 Real Use Cases   | Order pipeline, shipping flow, stock sync, user signup, etc.  |
| 🧪 Recognition Clue | Class does too many things or changes too often               |
| ✅ Benefit           | Loose coupling, easy debugging, better separation of concerns |

---

## 💬 Common Interview Questions (and Answers)

| Question                                | Sample Answer                                                         |
| --------------------------------------- | --------------------------------------------------------------------- |
| How do you ensure SRP in your services? | I break services by responsibility: validators, persisters, notifiers |
| What if business logic overlaps?        | Shared logic goes into helpers or base services                       |
| Do SRP classes become too granular?     | Yes, but it’s easier to maintain/test them                            |
| How does SRP help in scaling a team?    | Different teams can own/modify different modules safely               |

---

## ✅ Final Summary: SRP in High-Traffic Systems

| Principle    | Single Responsibility Principle                                  |
| ------------ | ---------------------------------------------------------------- |
| What         | One class = One responsibility                                   |
| How It Helps | Smaller, testable, change-resistant classes                      |
| Where Used   | All domain services in supply chain (order, shipment, inventory) |
| Applies To   | Classes, microservices, functions, APIs                          |
| Tools        | IntelliJ, SonarLint, Clean Code practices                        |

---

Would you like to move to **Open/Closed Principle** next or go over a **live LLD problem to apply SRP + DIP together** (e.g., “Design a Notification Dispatcher”)?
