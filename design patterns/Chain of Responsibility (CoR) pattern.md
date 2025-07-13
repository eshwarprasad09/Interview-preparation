Perfect, Eshwar. The **Chain of Responsibility (CoR)** pattern is widely used in **enterprise-grade systems**, especially in **middleware**, **request processing pipelines**, **event handling**, and **rule engines**.

It’s a **must-know behavioral design pattern** — commonly asked in **LLD/system design interviews** for senior engineering roles like the one you're targeting at **Target Corporation**.

---

# ✅ Chain of Responsibility (CoR) – Complete Guide (for System Design Interviews)

---

## 🔍 1. What Is Chain of Responsibility?

> **Chain of Responsibility** allows a **request** to be **passed through a chain of handlers**.
> Each handler decides either to process it or pass it to the next handler in the chain.

📌 It helps **decouple sender from receiver**, allowing **flexible request pipelines**.

---

## 🎯 Why CoR in High-Traffic, Supply Chain Systems?

| Use Case                     | Why Chain Works                                                   |
| ---------------------------- | ----------------------------------------------------------------- |
| 🛒 Order Validation Pipeline | Validate order data, payment status, inventory, fraud check       |
| 📦 Shipment Rules Engine     | Check carrier availability, packaging constraints, serviceability |
| 🔐 Authorization Chain       | Role-based, team-based, hierarchy-based access checks             |
| 📄 Invoice Processing        | Tax calculation → Discount application → Final price update       |
| 📩 Notification System       | Email → SMS → Push fallback chain                                 |
| ⚙️ API Gateway Filters       | Rate limit → Auth → Transform → Log → Forward                     |

---

## 🧠 When to Use CoR

✅ You want to **process requests in a pipeline**
✅ You want each handler to decide **whether to handle or pass**
✅ You want to **decouple processing logic**
✅ You want to support **dynamic or configurable chains**

---

## ❌ When NOT to Use

* You only have **one fixed handler**
* Every request must go through **every processor** (use Decorator instead)

---

## 🧱 Step-by-Step Implementation: Order Validation Pipeline

---

### 🔸 Step 1: Define Handler Interface

```java
public interface OrderValidator {
    void setNext(OrderValidator next);
    void validate(Order order);
}
```

---

### 🔸 Step 2: Abstract Base Class (optional)

```java
public abstract class BaseValidator implements OrderValidator {
    protected OrderValidator next;

    public void setNext(OrderValidator next) {
        this.next = next;
    }

    protected void next(Order order) {
        if (next != null) next.validate(order);
    }
}
```

---

### 🔸 Step 3: Concrete Validators

```java
public class PaymentValidator extends BaseValidator {
    public void validate(Order order) {
        if (!order.isPaymentComplete()) {
            throw new RuntimeException("Payment incomplete!");
        }
        System.out.println("✅ Payment validated.");
        next(order);  // delegate to next
    }
}

public class InventoryValidator extends BaseValidator {
    public void validate(Order order) {
        if (!order.hasStock()) {
            throw new RuntimeException("Inventory not available!");
        }
        System.out.println("✅ Inventory validated.");
        next(order);
    }
}

public class FraudValidator extends BaseValidator {
    public void validate(Order order) {
        if (order.getIp().startsWith("192.168")) {
            throw new RuntimeException("Fraudulent IP detected!");
        }
        System.out.println("✅ Fraud check passed.");
        next(order);
    }
}
```

---

### 🔸 Step 4: Client Code

```java
OrderValidator payment = new PaymentValidator();
OrderValidator inventory = new InventoryValidator();
OrderValidator fraud = new FraudValidator();

payment.setNext(inventory);
inventory.setNext(fraud);

Order order = new Order("ORD123", true, true, "103.45.67.1");
payment.validate(order);  // starts the chain
```

✅ Order is passed through a **chain of validators**
✅ Each handler **focuses on one responsibility**
✅ Easy to plug in new handlers or skip some based on config

---

## ✅ Real-World Use Case: Shipment Preparation Pipeline

### Shipment passes through:

1. Packaging Check
2. Carrier Assignment
3. Insurance Eligibility
4. Hazardous Item Check
5. Final Dispatch Trigger

Each of these is a handler. Based on item and destination, some handlers may pass without doing anything.

---

## 🧩 LLD Problem: Implement Discount Engine Using CoR

---

### 🧠 Problem Statement

Design a system that applies multiple discount strategies to a shopping cart:

1. Seasonal discount
2. Coupon discount
3. Loyalty discount

Each rule should be optional and can be **enabled/disabled** independently.

---

### Step 1: Define DiscountHandler Interface

```java
public interface DiscountHandler {
    void setNext(DiscountHandler next);
    double applyDiscount(double total);
}
```

---

### Step 2: Abstract Base Class

```java
public abstract class BaseDiscountHandler implements DiscountHandler {
    protected DiscountHandler next;

    public void setNext(DiscountHandler next) {
        this.next = next;
    }

    protected double next(double amount) {
        return (next != null) ? next.applyDiscount(amount) : amount;
    }
}
```

---

### Step 3: Concrete Handlers

```java
public class SeasonalDiscount extends BaseDiscountHandler {
    public double applyDiscount(double total) {
        total = total * 0.9; // 10% off
        System.out.println("Seasonal discount applied: " + total);
        return next(total);
    }
}

public class CouponDiscount extends BaseDiscountHandler {
    public double applyDiscount(double total) {
        total = total - 100; // ₹100 off
        System.out.println("Coupon discount applied: " + total);
        return next(total);
    }
}

public class LoyaltyDiscount extends BaseDiscountHandler {
    public double applyDiscount(double total) {
        total = total * 0.95; // 5% off
        System.out.println("Loyalty discount applied: " + total);
        return next(total);
    }
}
```

---

### Step 4: Client Code

```java
DiscountHandler seasonal = new SeasonalDiscount();
DiscountHandler coupon = new CouponDiscount();
DiscountHandler loyalty = new LoyaltyDiscount();

seasonal.setNext(coupon);
coupon.setNext(loyalty);

double finalAmount = seasonal.applyDiscount(1000); // Start the chain
System.out.println("Final amount: ₹" + finalAmount);
```

---

## ✅ CoR in Spring Boot / Real Systems

| Real Use Case           | Pattern in Action                                         |
| ----------------------- | --------------------------------------------------------- |
| Spring Security Filters | Authentication → Authorization → CSRF → Logging           |
| Kafka Middleware        | Pre-processors chained using CoR                          |
| Logging Pipelines       | Different logging levels or channels (info, debug, error) |
| Request Filters         | Rate limiting → Auth → Logging → Forwarding               |
| Rule Engine             | Apply configurable business rules one after another       |

---

## 🧾 Quick Revision Points

* Chain of Responsibility = **Pipeline of handlers**
* Each handler processes or **delegates** to the next
* Great for **rule engines**, **filter chains**, **approval flows**
* Promotes **SRP** and **Open-Closed Principle**
* You can **dynamically modify chain** (configurable)

---

## 💬 Common Interview Questions

| Question                               | Example Answer                                             |
| -------------------------------------- | ---------------------------------------------------------- |
| Difference between Chain and Strategy? | Strategy selects one behavior; Chain applies multiple      |
| What if one handler fails?             | Use try-catch or fail-fast design                          |
| How to make dynamic chains?            | Use config, DI container, or reflection to build the chain |
| How is this used in real systems?      | Spring Security filters, Kafka processors, API gateways    |
| Can handlers be async?                 | Yes — especially in reactive chains (WebFlux, Kafka, etc.) |

---

## ✅ Summary for System Design Interviews

| Pattern       | Chain of Responsibility                           |
| ------------- | ------------------------------------------------- |
| What          | Pass request through multiple handlers            |
| Best For      | Filters, validation, approval flows, rule engines |
| Real Use Case | Order validation, discount engine, shipment prep  |
| Benefits      | Extensible, decoupled, reconfigurable, testable   |

---

✅ Would you like to now combine these into a **mock system design problem (like "Design an Order Management System")** applying:

* 🔁 Strategy
* 🧱 Factory
* 🚦 Chain of Responsibility
* 📣 Observer
* 🧰 Builder
* 🔒 Singleton

Let me know and I’ll prepare that full mock system walkthrough for you.
