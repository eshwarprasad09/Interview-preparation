Awesome! You’re in great shape for tomorrow’s interview at Target. Let’s get you battle-ready with:

---

# ✅ Design Pattern Cheat Sheet – Java & System Design Focus

*Quick revision for interviews (especially for Target’s high-scale backend systems)*

---

### 🔷 **Creational Patterns** – Object Creation Logic

| Pattern              | When to Use                                       | Example Use Case                                            |
| -------------------- | ------------------------------------------------- | ----------------------------------------------------------- |
| **Singleton**        | Single shared instance needed                     | Config loader, DB connection pool                           |
| **Factory**          | Decide which object to create at runtime          | Payment processor, Notification sender                      |
| **Builder**          | Complex object creation with many optional params | Building Order/Shipment objects                             |
| **Prototype**        | Clone existing objects                            | Creating copies of templates (e.g., predefined SLAs)        |
| **Abstract Factory** | Group of related factories                        | UI toolkit components (DarkThemeFactory, LightThemeFactory) |

---

### 🔷 **Structural Patterns** – Compose & Organize Classes

| Pattern       | When to Use                                       | Example Use Case                                        |
| ------------- | ------------------------------------------------- | ------------------------------------------------------- |
| **Adapter**   | Incompatible interfaces need to work together     | Convert 3rd-party shipping provider response            |
| **Decorator** | Add responsibilities dynamically                  | Add logging, metrics, or tracing to services            |
| **Proxy**     | Control access to real object                     | Rate limiter, Auth proxy, Lazy-load large data          |
| **Facade**    | Provide a simple interface to a complex subsystem | Unified API for Inventory + Pricing + Fulfillment       |
| **Composite** | Tree structure of objects                         | Product bundles or hierarchical permissions             |
| **Bridge**    | Separate abstraction from implementation          | Decouple report generator from export type (PDF, Excel) |

---

### 🔷 **Behavioral Patterns** – Control Communication / Logic

| Pattern                     | When to Use                                    | Example Use Case                                |
| --------------------------- | ---------------------------------------------- | ----------------------------------------------- |
| **Strategy**                | Runtime behavior selection                     | Discount strategies, shipping modes             |
| **Observer**                | Event-based notification                       | Order placed → notify email, inventory, billing |
| **Template Method**         | Fixed steps, variable implementation           | ETL pipelines, Order lifecycle process          |
| **Command**                 | Encapsulate action as object                   | Task queue, Audit logging, Undo/Retry           |
| **Chain of Responsibility** | Request passed through a pipeline              | Auth → Quota → Fraud Check in order processing  |
| **State**                   | Object behavior changes with internal state    | Order state (Placed → Shipped → Delivered)      |
| **Mediator**                | Central coordinator between many objects       | Chat room server, Workflow engine               |
| **Iterator**                | Traverse collection without exposing internals | Pagination logic                                |
| **Memento**                 | Save/restore state                             | Undo system in UI/IDE                           |

---

### 🎯 Pro Tips for Interview

* Most used in production: **Factory**, **Strategy**, **Observer**, **Builder**, **Singleton**, **Chain of Responsibility**, **Template**
* Know how to **compose patterns** (e.g., Factory + Strategy)
* Explain the **problem→why this pattern→how it solves it**
* Always connect patterns to **real-world system examples**

---

## ✅ Mock System Design: “Design Order Processing System” (Target-style)

Let’s apply multiple design patterns into a **high-level architecture** you can talk about.

---

### 🛒 Problem Statement:

Design a scalable **Order Processing System** for a large e-commerce or retail chain.

### 🎯 Key Features:

* Accept orders from website/app
* Validate and enrich order
* Route order to proper warehouse
* Notify customer and inventory
* Scale to handle **high load**

---

## 🔧 Design Layers and Patterns

### ✅ 1. **OrderService Layer**

* Uses **Strategy Pattern** to apply different **Discount or Validation rules** based on region or user type.
* Uses **Template Method Pattern** for common processing steps: validate → enrich → route.

```java
public abstract class OrderProcessor {
    public final void process(Order order) {
        validate(order);
        enrich(order);
        route(order);
    }

    protected abstract void validate(Order order);
    protected abstract void enrich(Order order);
    protected void route(Order order) {
        // default implementation
    }
}
```

---

### ✅ 2. **Warehouse Routing**

* Uses **Factory Pattern** to return the correct warehouse routing logic based on geography, capacity, item type.

```java
public class RoutingFactory {
    public static WarehouseRouter getRouter(Order order) {
        if (order.getRegion().equals("South")) return new SouthWarehouseRouter();
        else return new DefaultRouter();
    }
}
```

---

### ✅ 3. **Inventory Updater and Event Publisher**

* Uses **Observer Pattern** to notify:

    * Inventory Service
    * Email Service
    * Audit Logger
    * Shipment Scheduler

```java
publisher.subscribe(new InventoryService());
publisher.subscribe(new EmailService());
publisher.publish("ORDER_PLACED", orderId);
```

---

### ✅ 4. **Discount Rule Engine**

* Uses **Strategy Pattern**:

    * `LoyaltyDiscount`, `FestiveDiscount`, `FlashSaleDiscount`

```java
discountStrategy.apply(orderAmount);
```

---

### ✅ 5. **Handler Chain for Pre-Processing**

* Uses **Chain of Responsibility**:

    * AuthHandler → FraudCheckHandler → QuotaHandler → OrderSaver

```java
authHandler.setNext(fraudHandler).setNext(quotaHandler);
authHandler.handle(orderRequest);
```

---

### ✅ 6. **Builder Pattern** for Constructing Order Objects

```java
Order order = new Order.Builder()
    .userId("U123")
    .items(cartItems)
    .shippingAddress(addr)
    .paymentMethod("UPI")
    .build();
```

---

### ✅ 7. **Singleton** for Configuration or Shared Services

```java
Config config = Config.getInstance();
config.get("inventory.service.url");
```

---

## 🏗️ System Design Diagram (Mental Model)

```text
[API Gateway]
     |
[OrderController]
     |
[OrderService]
     |
[OrderProcessor (Template)]
     ├─ validate()    ← Strategy
     ├─ enrich()
     └─ route()       ← uses RoutingFactory
     |
     └─ [EventPublisher] → Email, Inventory, Shipment (Observer)
```

---

## 📚 What You Should Explain in Interview

1. **High-level components** and how requests flow
2. Which **patterns** you’ve applied and **why**
3. How it helps **scalability, flexibility, and decoupling**
4. How to plug-in new business logic **without code changes** (thanks to patterns)

---

## 📥 Want This as PDF?

I can export this whole content into a **printable, high-quality PDF** cheat sheet for you to carry into the interview.

Would you like that?
