Perfect, Eshwar! Let's now build a complete **Order Management System** as a full-scale mock **System Design Interview Question** using the following design patterns, **exactly how they’re used in production systems**:

---

# 🔧 Mock System Design: Order Management System (OMS)

**Design an enterprise-grade Order Management System** for a supply chain platform like Target, applying:

* ✅ Strategy
* ✅ Factory
* ✅ Chain of Responsibility
* ✅ Observer
* ✅ Builder
* ✅ Singleton
* ✅ Template Method

---

## 💡 Business Requirements

* Customers place orders (B2C, B2B)
* System validates orders, calculates discounts, selects shipping
* Notifies users (email/SMS), updates dashboards
* Can plug in future logic without downtime

---

## 🏗️ High-Level Components

| Component                 | Description                                         |
| ------------------------- | --------------------------------------------------- |
| OrderService              | Orchestrates creation, validation, persistence      |
| Validators                | Applies rule chain (payment, inventory, fraud)      |
| ShippingSelector          | Selects best shipping partner dynamically           |
| DiscountEngine            | Applies discounts based on strategy                 |
| NotificationService       | Pushes updates to observers                         |
| OrderBuilder              | Constructs complex Order object                     |
| OrderProcessor (Template) | Template for processing orders (B2C, B2B, Internal) |

---

## 📦 Design Patterns Applied

| Pattern   | Use in OMS                                       |
| --------- | ------------------------------------------------ |
| Strategy  | Dynamic discount/shipping selection              |
| Factory   | Creates shipping/payment/discount objects        |
| CoR       | Validation chain (payment → inventory → fraud)   |
| Observer  | Notify systems/services when order is created    |
| Builder   | Construct large immutable Order object           |
| Singleton | ConfigurationService, Logger, DB Connection      |
| Template  | Standardize order processing pipeline (B2C, B2B) |

---

---

## 🧱 Implementation Overview (Java-style Pseudocode)

---

### 🧰 1. `Builder`: Building the `Order` Object

```java
Order order = new Order.Builder()
    .withId("ORD123")
    .withCustomerId("CUS001")
    .withItems(List.of("item1", "item2"))
    .withShippingAddress("Bangalore")
    .withTotal(1200.0)
    .build();
```

✅ Clean, immutable, built in layers (data from API + DB + context)

---

### 🧾 2. `Chain of Responsibility`: Order Validation Pipeline

```java
OrderValidator payment = new PaymentValidator();
OrderValidator inventory = new InventoryValidator();
OrderValidator fraud = new FraudValidator();

payment.setNext(inventory);
inventory.setNext(fraud);

payment.validate(order); // Chain starts here
```

✅ Each validator focuses on single logic
✅ Extensible without modifying other handlers

---

### 📦 3. `Factory`: Shipping/Discount/Payment Provider

```java
ShippingProvider provider = ShippingFactory.getProvider("fedex");
provider.createShipment(order);

DiscountStrategy strategy = DiscountFactory.getStrategy(order.getCustomerType());
double finalPrice = strategy.applyDiscount(order.getTotal());
```

✅ Replaces massive switch-case logic
✅ Easily plug new types in future

---

### 🎯 4. `Strategy`: Apply Discount Dynamically

```java
public interface DiscountStrategy {
    double applyDiscount(double amount);
}

public class B2CDiscount implements DiscountStrategy {
    public double applyDiscount(double amount) {
        return amount * 0.90;
    }
}

public class B2BDiscount implements DiscountStrategy {
    public double applyDiscount(double amount) {
        return amount * 0.85;
    }
}
```

✅ Strategy chosen at runtime based on customer type
✅ Follows Open-Closed Principle

---

### 🔔 5. `Observer`: Notify Systems

```java
public interface OrderObserver {
    void onOrderCreated(Order order);
}

public class EmailService implements OrderObserver {
    public void onOrderCreated(Order order) {
        // send email
    }
}

public class InventoryNotifier implements OrderObserver {
    public void onOrderCreated(Order order) {
        // update inventory dashboards
    }
}
```

```java
orderService.addObserver(new EmailService());
orderService.addObserver(new InventoryNotifier());
orderService.notifyObservers(order);
```

✅ Loosely coupled services
✅ New observers added without touching OrderService

---

### 🧩 6. `Template Pattern`: OrderProcessor Template

```java
public abstract class OrderProcessor {
    public final void processOrder(Order order) {
        validate(order);
        enrich(order);
        persist(order);
        notify(order);
    }

    protected abstract void validate(Order order);
    protected abstract void enrich(Order order);
    protected abstract void notify(Order order);

    protected void persist(Order order) {
        // Common logic to save in DB
    }
}
```

```java
public class B2COrderProcessor extends OrderProcessor {
    protected void validate(Order order) {
        // Use CoR
    }
    protected void enrich(Order order) {
        // Set discounts, assign shipping
    }
    protected void notify(Order order) {
        // Send email/SMS
    }
}
```

✅ Allows defining a **skeleton algorithm**
✅ Subclasses plug in specific steps

---

### 🔒 7. `Singleton`: Config / Logger / DB Connection

```java
public class ConfigService {
    private static final ConfigService INSTANCE = new ConfigService();
    private ConfigService() {}

    public static ConfigService getInstance() {
        return INSTANCE;
    }

    public String get(String key) {
        return configMap.get(key);
    }
}
```

✅ Ensures only one instance used across system
✅ Used for shared state/services (DB, cache, logger, etc.)

---

## 🚦 Sequence Flow (All Patterns Together)

1. 🧰 Build Order using Builder
2. ✅ Validate Order using CoR
3. 🎯 Choose Discount Strategy using Factory + Strategy
4. 📦 Choose ShippingProvider using Factory
5. 📤 Notify observers (email/SMS/UI)
6. 🔁 Process using Template Method
7. 🔒 Use Singleton services (config/logger)

---

## 🧾 Interview Revision: Summary Table

| Pattern                 | What It Solves                   | Where Applied                    |
| ----------------------- | -------------------------------- | -------------------------------- |
| Builder                 | Step-by-step object construction | Order, Request, Shipment objects |
| Chain of Responsibility | Rule-based pipelines             | Validation, Filtering            |
| Strategy                | Algorithm choice at runtime      | Discounts, Shipping, Payment     |
| Factory                 | Object creation logic            | ShippingProvider, PaymentGateway |
| Observer                | Event notification               | Emails, dashboards, logs         |
| Template                | Process blueprint                | OrderProcessor (B2C/B2B/Guest)   |
| Singleton               | Shared service instance          | Config, Logger, DB Pool          |

---

## 💬 Mock System Design Questions You Can Expect

| Question                                                         | Tip for Answer              |
| ---------------------------------------------------------------- | --------------------------- |
| How will you validate a complex order request?                   | Use Chain of Responsibility |
| How to allow dynamic shipping carrier selection?                 | Strategy + Factory          |
| How to keep services loosely coupled when sending notifications? | Observer Pattern            |
| How do you ensure common steps in order processing?              | Template Pattern            |
| How do you inject configurations globally?                       | Singleton                   |

---

Let me know if you want:

* ✅ A **UML Class Diagram** of this system
* ✅ A **live mock interview** where you explain this design
* ✅ A real coding implementation in Java or Spring Boot
* ✅ Add caching, event queues (Kafka), or retry handling

Would you like to proceed with any of these?
