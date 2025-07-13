Great, Eshwar! The **Strategy Design Pattern** is a **must-know** for interviews—especially for **system design rounds** and **LLD problems** in **supply chain systems** where runtime decision-making (like pricing, shipping, inventory, or routing) is critical.

---

# ✅ Strategy Design Pattern – Complete Guide (For System Design Interviews)

---

## 🔍 1. What Is Strategy Pattern?

> The **Strategy Pattern** defines a family of algorithms (strategies), encapsulates each one, and makes them interchangeable at runtime.

📌 It lets the **algorithm vary independently from the client** that uses it.

---

## 🎯 Why Strategy in High-Traffic Supply Chain Systems?

| Use Case                  | Strategy Applied                               |
| ------------------------- | ---------------------------------------------- |
| 🚚 Shipping Strategy      | FedEx, BlueDart, Delhivery – chosen at runtime |
| 💰 Payment Strategy       | Razorpay, Stripe, COD – selected per order     |
| 🎯 Inventory Allocation   | FIFO, LIFO, Zone-Based, Dynamic prioritization |
| 🧾 Pricing Logic          | Different pricing for B2B, B2C, partners       |
| ⚡ Retry Logic             | Exponential, FixedDelay, NoRetry               |
| 📊 Recommendation Engines | Personalized, Trending, Sponsored              |

---

## 🧠 When to Use Strategy

✅ You need **runtime selection** of algorithms
✅ You want to **replace if-else/switch** logic
✅ You have **interchangeable behaviors** with a common interface
✅ You want to **extend new strategies without modifying old code**

---

## ❌ When NOT to Use

* If you have **only one implementation** that never changes
* If strategies don’t share a common method signature

---

## 🧱 Real-World Implementation: Shipping Strategy Example

---

### 🔸 Step 1: Strategy Interface

```java
public interface ShippingStrategy {
    double calculateShippingCost(Order order);
}
```

---

### 🔸 Step 2: Concrete Strategies

```java
public class FedExShipping implements ShippingStrategy {
    public double calculateShippingCost(Order order) {
        return 50 + order.getWeight() * 1.2; // fixed + per kg
    }
}

public class BlueDartShipping implements ShippingStrategy {
    public double calculateShippingCost(Order order) {
        return 30 + order.getWeight() * 1.5; // fixed + per kg
    }
}
```

---

### 🔸 Step 3: Context Class (e.g., Shipping Service)

```java
public class ShippingService {
    private ShippingStrategy strategy;

    // Set the strategy dynamically at runtime
    public void setShippingStrategy(ShippingStrategy strategy) {
        this.strategy = strategy;
    }

    public double calculateShipping(Order order) {
        return strategy.calculateShippingCost(order);
    }
}
```

---

### 🔸 Step 4: Client Code

```java
ShippingService service = new ShippingService();

Order order = new Order("ORD123", 10); // 10kg

service.setShippingStrategy(new FedExShipping());
System.out.println("FedEx Cost: ₹" + service.calculateShipping(order));

service.setShippingStrategy(new BlueDartShipping());
System.out.println("BlueDart Cost: ₹" + service.calculateShipping(order));
```

✅ Shipping strategy is interchangeable at runtime
✅ Adding new carriers doesn’t affect existing code

---

## ✅ Real-World Use Case: Inventory Allocation Strategy

In Target’s supply chain:

* For fast-moving items, use **zone-based allocation**
* For perishable goods, use **FIFO**
* For rare items, use **closest warehouse with stock**

```java
public interface InventoryAllocationStrategy {
    void allocate(String sku, int quantity);
}
```

---

### Different Implementations:

* `ZoneBasedAllocationStrategy`
* `FifoAllocationStrategy`
* `WarehousePriorityAllocationStrategy`

Used in:

```java
InventoryService.setStrategy(new FifoAllocationStrategy());
InventoryService.allocate("SKU123", 5);
```

---

## 🧩 LLD Problem Using Strategy Pattern

---

### 🧠 Problem: Design a Payment Processing System with Dynamic Payment Modes

---

### Step 1: Strategy Interface

```java
public interface PaymentStrategy {
    void pay(String orderId, double amount);
}
```

---

### Step 2: Concrete Strategies

```java
public class RazorpayStrategy implements PaymentStrategy {
    public void pay(String orderId, double amount) {
        System.out.println("Paid ₹" + amount + " via Razorpay for " + orderId);
    }
}

public class StripeStrategy implements PaymentStrategy {
    public void pay(String orderId, double amount) {
        System.out.println("Paid ₹" + amount + " via Stripe for " + orderId);
    }
}
```

---

### Step 3: Context Class

```java
public class PaymentContext {
    private PaymentStrategy strategy;

    public PaymentContext(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void pay(String orderId, double amount) {
        strategy.pay(orderId, amount);
    }
}
```

---

### Step 4: Usage

```java
PaymentContext context = new PaymentContext(new RazorpayStrategy());
context.pay("ORD101", 1000.0);

context = new PaymentContext(new StripeStrategy());
context.pay("ORD102", 1500.0);
```

---

## 🧾 Quick Revision Points

* Strategy = runtime interchangeable algorithms
* Used when behavior (logic) changes frequently
* Avoids tight coupling and `if-else` chains
* Follow **OCP**: add new strategies without changing old ones
* Ideal for pricing, shipping, payment, retry, filtering, etc.

---

## ⚙️ Strategy Pattern in Spring Boot

Let’s say you want to inject all pricing strategies at startup:

```java
@Service
public class PricingStrategyFactory {
    private final Map<String, PricingStrategy> strategyMap;

    @Autowired
    public PricingStrategyFactory(List<PricingStrategy> strategies) {
        this.strategyMap = strategies.stream()
            .collect(Collectors.toMap(PricingStrategy::getType, Function.identity()));
    }

    public PricingStrategy getStrategy(String type) {
        return strategyMap.getOrDefault(type, new DefaultPricingStrategy());
    }
}
```

✅ Injects all strategies via Spring
✅ Selects strategy dynamically via type

---

## 💬 Common Interview Follow-Up Questions

| Question                             | Sample Answer                                                 |
| ------------------------------------ | ------------------------------------------------------------- |
| Why use Strategy instead of if-else? | Better maintainability, clean extensibility, adheres to SOLID |
| Can strategies hold state?           | Yes, but be careful – stateless is safer and preferred        |
| How is this different from Factory?  | Factory creates objects; Strategy encapsulates **behavior**   |
| Where is this used in Spring?        | RetryTemplate, LoadBalancing, Cache eviction policies         |
| How would you test strategies?       | Unit-test each class independently, mock context if needed    |

---

## ✅ Summary for System Design

| Concept          | Example                                                     |
| ---------------- | ----------------------------------------------------------- |
| Strategy         | ShippingCostCalculator, PaymentHandler, InventoryAllocator  |
| Encapsulation    | Strategy interface hides implementation from client         |
| Runtime decision | Based on customer location, SKU type, or payment preference |
| Scalable         | Add new behaviors without touching old code                 |

---

Would you like to now proceed to **Observer**, **Decorator**, or do a **mock design (e.g., "Design Order Processing System")** where we combine Strategy + Factory + Builder + Singleton?
