Great choice, Eshwar! Let's learn **“Composition Over Inheritance”** — a **foundational object-oriented design principle** and a **core concept behind SOLID, microservices, and scalable architecture**.

This principle is often tested **indirectly** in system design interviews like the one you’ll face at **Target Corporation**.

---

# ✅ Composition Over Inheritance – Complete Guide with Examples, Cheat Sheet & LLD

---

## 🔥 What is "Composition Over Inheritance"?

> **Prefer composing behavior using objects (has-a) rather than inheriting it from classes (is-a).**

### ✅ Inheritance:

* You get behavior from a **parent class**.
* Tight coupling and fragile hierarchy.

### ✅ Composition:

* You create classes by **combining reusable components**.
* Promotes flexibility and extensibility.

---

### ⚠️ Problem with Inheritance (Overused):

```java
class Vehicle {
    void start() {}
    void stop() {}
}

class ElectricCar extends Vehicle {
    void chargeBattery() {}
}
```

👎 Issues:

* If business logic in `Vehicle` changes, it affects all subclasses.
* Can't combine behaviors from multiple hierarchies (no multiple inheritance in Java).
* Inflexible and tightly coupled.

---

### ✅ Composition Example

```java
class Engine {
    void start() {}
    void stop() {}
}

class ElectricBattery {
    void charge() {}
}

class ElectricCar {
    private final Engine engine;
    private final ElectricBattery battery;

    ElectricCar(Engine engine, ElectricBattery battery) {
        this.engine = engine;
        this.battery = battery;
    }

    void startCar() {
        engine.start();
    }

    void chargeCar() {
        battery.charge();
    }
}
```

✔ `ElectricCar` **uses (has-a)** components: Engine and Battery
✔ Can easily swap or upgrade Engine or Battery
✔ Follows **DIP, SRP, and OCP** naturally
✔ Works great with **Dependency Injection**

---

## 📦 Why It Matters in Supply Chain Systems

| Use Case            | Why Composition Helps                                      |
| ------------------- | ---------------------------------------------------------- |
| Notification System | Compose different channel notifiers (Email, SMS, Slack)    |
| Inventory Engine    | Compose data from internal + vendor sources                |
| Pricing Rules       | Combine multiple rule engines (Loyalty, Discount, Tax)     |
| Payment System      | Compose behavior from different gateways, fraud checkers   |
| Order Fulfillment   | Compose steps: inventory check, fraud validation, shipment |

---

## 🚫 Inheritance Anti-Pattern in Real Systems

```java
class OrderService {
    void placeOrder() {}
}

class DiscountedOrderService extends OrderService {
    void applyDiscount() {}
}
```

### ❌ Problems:

* Cannot apply multiple variations (e.g., loyalty, coupons) without deep hierarchies.
* Breaks **OCP and LSP** if behavior changes.

---

## ✅ Composition-Based Solution

```java
class OrderService {
    private final DiscountStrategy discountStrategy;
    private final PaymentGateway paymentGateway;

    OrderService(DiscountStrategy discountStrategy, PaymentGateway paymentGateway) {
        this.discountStrategy = discountStrategy;
        this.paymentGateway = paymentGateway;
    }

    void placeOrder(Order order) {
        double discountedAmount = discountStrategy.apply(order.getTotalAmount());
        paymentGateway.pay(order, discountedAmount);
    }
}
```

🎯 Now:

* Easily swap different discount strategies or gateways
* No inheritance chain
* Better for testing and extension
* Perfect for microservice-based design

---

## 🧩 LLD Problem – Composition in Shipment Flow

### ❓ Problem:

You want to design a shipment service that:

* Sends shipment through a courier
* Logs the shipment in a DB
* Notifies customer after shipping

---

### ❌ Inheritance-Based Approach (Rigid)

```java
class ShipmentService {
    void ship(Order o) {}
}

class LoggedShipmentService extends ShipmentService {
    void log() {}
}

class NotifiedShipmentService extends LoggedShipmentService {
    void notifyCustomer() {}
}
```

👎 Very rigid. Can't reuse log/notify separately.

---

### ✅ Composition-Based Approach

```java
interface CourierService {
    void ship(Order order);
}

interface Logger {
    void log(Order order);
}

interface Notifier {
    void notify(Order order);
}

class ShipmentService {
    private final CourierService courier;
    private final Logger logger;
    private final Notifier notifier;

    ShipmentService(CourierService courier, Logger logger, Notifier notifier) {
        this.courier = courier;
        this.logger = logger;
        this.notifier = notifier;
    }

    void ship(Order order) {
        courier.ship(order);
        logger.log(order);
        notifier.notify(order);
    }
}
```

🎯 Clean, composable, testable
🔁 Swap `Notifier` with SMS/Slack
🔁 Add `RetryCourier` decorator without changing `ShipmentService`

---

## 📌 When to Use Composition

| Symptom                           | Suggests Composition             |
| --------------------------------- | -------------------------------- |
| Rigid class hierarchy             | Hard to extend                   |
| Inability to reuse behavior       | Can’t mix behaviors              |
| Complex test setup                | Need mocks/stubs                 |
| Need runtime behavior changes     | Strategy pattern or plugin       |
| Multiple combinations of features | Class explosion with inheritance |

---

## 🧾 Interview Cheat Sheet – Composition over Inheritance

| Topic                   | Summary                                                             |
| ----------------------- | ------------------------------------------------------------------- |
| 📖 Definition           | Prefer object composition (`has-a`) over class inheritance (`is-a`) |
| 🎯 Goal                 | Flexibility, reuse, extensibility                                   |
| 🚫 Avoid                | Deep inheritance chains, fragile base classes                       |
| ✅ Prefer                | Interfaces + injected behaviors                                     |
| ☂️ Patterns that use it | Strategy, Decorator, Adapter, Bridge, Composite                     |
| ✅ Use cases             | Shipping pipelines, Pricing Engines, Notification Systems           |
| 🚩 Recognition          | Class hierarchy explosion, rigid code, hard testing                 |
| 💬 Java Tools           | Interfaces, Functional interfaces, Lambdas, Spring Beans            |

---

## 💬 Common Interview Questions

| Question                                  | Ideal Answer                                                    |
| ----------------------------------------- | --------------------------------------------------------------- |
| What is composition over inheritance?     | Prefer combining behaviors using objects instead of subclassing |
| When would you not use inheritance?       | When behavior combinations are dynamic or vary per runtime      |
| Have you used this in real-world systems? | Yes, in pricing engines and shipping logic via interfaces       |
| What are the benefits?                    | Decoupling, reusability, extensibility, testability             |
| What patterns favor composition?          | Strategy, Adapter, Bridge, Decorator                            |

---

## 🧠 Final Summary

| Principle       | Composition Over Inheritance                                 |
| --------------- | ------------------------------------------------------------ |
| Core Idea       | Use objects to compose behavior instead of subclassing       |
| Benefits        | Modular, testable, extensible code                           |
| Real Use        | Shipping pipelines, Order Services, Notification engines     |
| Best With       | DIP, Strategy, OCP, SRP                                      |
| Anti-Patterns   | Large abstract base classes, overridden behaviors everywhere |
| Interview Value | **High** — indirectly tested via design rounds               |

---

Would you like next:

1. 🔄 A **flow diagram** showing when to use Inheritance vs. Composition?
2. 🧪 A **mock LLD problem combining SRP + DIP + Composition** (e.g., Notification System with SMS, Email, Slack)?
3. 📥 A printable **Design Principles & Patterns Cheat Sheet** for last-minute revision?

Let me know how you want to wrap up your system design prep!
