Great — let’s dive into the most **important and commonly used design patterns** for backend systems like **supply chain, e-commerce, or high-traffic distributed services**, especially for your **Target Corporation system design interview**.

---

# 🎯 Production-Grade Java Design Patterns for System Design Interviews

---

## 🔷 Pattern Categories

| Category       | Key Patterns                                                   |
| -------------- | -------------------------------------------------------------- |
| **Creational** | Singleton, Factory, Builder                                    |
| **Structural** | Adapter, Proxy, Decorator                                      |
| **Behavioral** | Strategy, Observer, Template, Command, Chain of Responsibility |

---

## ✅ 1. **Singleton Pattern**

**Purpose**: Ensure a class has **only one instance** and provides a **global access point**.

---

### 📦 Real-World Use Case:

* **Config Loader** (load app settings once)
* **Database Connection Pool Manager**
* **RateLimiter (API Gateway)**

### 🔍 When to Use:

* You need a **shared instance across the app**
* You want to **avoid re-initializing costly resources**

---

### 🧑‍💻 Code (Thread-safe Production Singleton)

```java
public class AppConfig {

    // private static volatile instance (ensures visibility in multithreaded environment)
    private static volatile AppConfig instance;

    private Properties props;

    // private constructor (no outside instantiation)
    private AppConfig() {
        props = new Properties();
        props.setProperty("env", "PROD");
    }

    // thread-safe lazy initialization with double-checked locking
    public static AppConfig getInstance() {
        if (instance == null) {
            synchronized (AppConfig.class) {
                if (instance == null) {
                    instance = new AppConfig();
                }
            }
        }
        return instance;
    }

    public String getProperty(String key) {
        return props.getProperty(key);
    }
}
```

---

## ✅ 2. **Factory Pattern**

**Purpose**: **Create objects without exposing instantiation logic** to the client.

---

### 📦 Use Case:

* Payment processor (`UPI`, `Card`, `NetBanking`)
* Notification sender (`Email`, `SMS`, `Push`)
* Inventory strategy in a supply chain system

### 🔍 When to Use:

* You need to create objects based on **runtime input**
* You want to **abstract creation logic** from the caller

---

### 🧑‍💻 Code Example:

```java
public interface Payment {
    void pay(double amount);
}

public class UpiPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid via UPI");
    }
}

public class CardPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid via Card");
    }
}

public class PaymentFactory {
    public static Payment getPayment(String mode) {
        switch (mode.toLowerCase()) {
            case "upi": return new UpiPayment();
            case "card": return new CardPayment();
            default: throw new IllegalArgumentException("Unknown mode");
        }
    }
}

// Usage
Payment payment = PaymentFactory.getPayment("upi");
payment.pay(500.0);
```

---

## ✅ 3. **Strategy Pattern**

**Purpose**: **Encapsulate a group of interchangeable algorithms** and make them easily swappable.

---

### 📦 Use Case:

* **Dynamic inventory allocation** based on warehouse proximity
* **Payment gateway switching** based on region
* **Discount strategy** in pricing engine

### 🔍 When to Use:

* Multiple algorithms exist for one task
* Want to choose algorithm at runtime

---

### 🧑‍💻 Code Example:

```java
public interface DiscountStrategy {
    double applyDiscount(double amount);
}

public class FestiveDiscount implements DiscountStrategy {
    public double applyDiscount(double amount) {
        return amount * 0.9;  // 10% off
    }
}

public class LoyaltyDiscount implements DiscountStrategy {
    public double applyDiscount(double amount) {
        return amount * 0.85;  // 15% off
    }
}

// Context
public class OrderService {
    private DiscountStrategy strategy;

    public OrderService(DiscountStrategy strategy) {
        this.strategy = strategy;
    }

    public double checkout(double amount) {
        return strategy.applyDiscount(amount);
    }
}

// Usage
DiscountStrategy strategy = new FestiveDiscount();
OrderService service = new OrderService(strategy);
System.out.println(service.checkout(1000));
```

---

## ✅ 4. **Observer Pattern**

**Purpose**: **Notify multiple components** when one event occurs.

---

### 📦 Use Case:

* **Order placed → notify inventory, billing, shipment**
* **Cache invalidation**
* **Real-time stock update broadcast**

### 🔍 When to Use:

* One-to-many relationship needed
* Decoupled event propagation required

---

### 🧑‍💻 Code Example:

```java
public interface EventListener {
    void update(String eventType, String data);
}

public class EmailNotifier implements EventListener {
    public void update(String eventType, String data) {
        System.out.println("Email sent for " + eventType + ": " + data);
    }
}

public class InventoryUpdater implements EventListener {
    public void update(String eventType, String data) {
        System.out.println("Inventory updated for " + eventType + ": " + data);
    }
}

public class EventPublisher {
    private List<EventListener> listeners = new ArrayList<>();

    public void subscribe(EventListener listener) {
        listeners.add(listener);
    }

    public void publish(String eventType, String data) {
        for (EventListener l : listeners) {
            l.update(eventType, data);
        }
    }
}

// Usage
EventPublisher publisher = new EventPublisher();
publisher.subscribe(new EmailNotifier());
publisher.subscribe(new InventoryUpdater());
publisher.publish("ORDER_PLACED", "Order#123");
```

---

## ✅ 5. **Builder Pattern**

**Purpose**: Build complex objects **step-by-step** using a fluent interface.

---

### 📦 Use Case:

* **Constructing large domain objects** (Product, Shipment, Order)
* **Immutable config/DTO creation**
* **Complex nested JSON construction**

---

### 🧑‍💻 Code Example:

```java
public class Shipment {

    private final String orderId;
    private final String destination;
    private final boolean fragile;

    private Shipment(Builder builder) {
        this.orderId = builder.orderId;
        this.destination = builder.destination;
        this.fragile = builder.fragile;
    }

    public static class Builder {
        private String orderId;
        private String destination;
        private boolean fragile;

        public Builder orderId(String orderId) {
            this.orderId = orderId;
            return this;
        }

        public Builder destination(String destination) {
            this.destination = destination;
            return this;
        }

        public Builder fragile(boolean fragile) {
            this.fragile = fragile;
            return this;
        }

        public Shipment build() {
            return new Shipment(this);
        }
    }
}

// Usage
Shipment shipment = new Shipment.Builder()
    .orderId("ORD123")
    .destination("Bangalore")
    .fragile(true)
    .build();
```

---

## ✅ 6. **Template Method Pattern**

**Purpose**: Define a **fixed algorithm structure** in a base class and **allow child classes to customize steps**.

---

### 📦 Use Case:

* **Order processing pipeline** (validate, reserve stock, deduct inventory)
* **ETL Jobs** (Extract → Transform → Load)
* **Audit-enabled service calls**

---

### 🧑‍💻 Code Example:

```java
public abstract class OrderProcessor {

    // Template method
    public final void processOrder() {
        validate();
        reserveStock();
        confirmOrder();
    }

    // Steps to be implemented by subclasses
    protected abstract void validate();
    protected abstract void reserveStock();

    // Common step
    protected void confirmOrder() {
        System.out.println("Order confirmed");
    }
}

public class B2BOrderProcessor extends OrderProcessor {
    protected void validate() {
        System.out.println("Validating B2B order");
    }

    protected void reserveStock() {
        System.out.println("Stock reserved for B2B");
    }
}
```

---

## ✅ 7. **Chain of Responsibility Pattern**

**Purpose**: Create a **pipeline of handlers** where each one processes or passes the request further.

---

### 📦 Use Case:

* **Request validation pipeline** (auth, quota check, fraud check)
* **Shipment rules (fragile, perishable, bulk)**
* **Middleware pipelines (filters, interceptors)**

---

### 🧑‍💻 Code Example:

```java
public abstract class Handler {
    protected Handler next;

    public Handler setNext(Handler next) {
        this.next = next;
        return next;
    }

    public abstract void handle(String request);
}

public class AuthHandler extends Handler {
    public void handle(String request) {
        if (request.contains("auth")) {
            System.out.println("Auth passed");
            if (next != null) next.handle(request);
        } else {
            System.out.println("Auth failed");
        }
    }
}

public class QuotaHandler extends Handler {
    public void handle(String request) {
        if (request.contains("quota")) {
            System.out.println("Quota ok");
            if (next != null) next.handle(request);
        } else {
            System.out.println("Quota exceeded");
        }
    }
}

// Usage
Handler pipeline = new AuthHandler().setNext(new QuotaHandler());
pipeline.handle("auth,quota");
```

---

## 📚 Summary – How to Recognize and Use Patterns

| Pattern                     | Use When...                                       |
| --------------------------- | ------------------------------------------------- |
| **Singleton**               | One shared instance needed (config, logger, pool) |
| **Factory**                 | Need to abstract object creation                  |
| **Strategy**                | Need dynamic algorithm switching                  |
| **Observer**                | One-to-many event notifications                   |
| **Builder**                 | Complex object construction (step-by-step)        |
| **Template Method**         | Fixed workflow with variable steps                |
| **Chain of Responsibility** | Pipeline of tasks with fail/pass logic            |

---

## 🔜 Next

Would you like:

* ✅ A printable **Design Pattern Cheat Sheet** for last-minute revision?
* ✅ A mock system design question (e.g., "Design Order Processing System") where we apply these patterns?

Let me know, and I’ll generate it before your interview tomorrow!
