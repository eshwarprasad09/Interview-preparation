Absolutely, Eshwar! Let’s deep dive into the **Factory Design Pattern** — one of the **most asked patterns** in system design interviews (especially for **Senior Engineer roles** at companies like Target, Amazon, etc.).

---

# ✅ Factory Design Pattern – Complete Guide (For System Design Interviews)

---

## 🔍 1. What Is Factory Pattern?

> Factory Pattern is a **creational design pattern** that provides an interface for **creating objects**, but **lets subclasses decide** which class to instantiate.

📌 You delegate object creation to a method/factory, not to the client directly.

---

## 🎯 Why It’s Used in High-Traffic Supply Chain Systems

| Scenario                            | Why Factory Is Ideal                                              |
| ----------------------------------- | ----------------------------------------------------------------- |
| Order Processing by Type            | Create different processing strategies: COD, Credit, Wallet, etc. |
| Shipment Creation                   | Use different carriers: FedEx, UPS, DHL, Bluedart                 |
| Notification Systems                | Factory creates Email, SMS, Push services dynamically             |
| Payment Gateway Integrations        | Stripe, Razorpay, PayPal – plug-and-play via factories            |
| Kafka Consumer Handler Registration | Each event type maps to different processor                       |
| Report Generation                   | PDF, Excel, JSON reports via factory method                       |

---

## 🧠 When to Use Factory (Recognition Pattern)

✅ Object creation logic is **dynamic or complex**
✅ You want to **decouple creation logic** from client logic
✅ You need **runtime decision-making** to instantiate classes
✅ You expect **extensibility** (e.g., adding new order/payment types)

❌ Don’t use if:

* Only 1 concrete class exists
* Object creation is trivial

---

## 🧱 Implementation Breakdown

### 🎯 Problem: Shipment Service Factory

We need to create shipments dynamically for different carriers based on a string/enum input.

---

### 🔸 Step 1: Define the Product Interface

```java
public interface ShipmentService {
    void shipOrder(String orderId);
}
```

---

### 🔸 Step 2: Implement Concrete Products

```java
public class FedExShipment implements ShipmentService {
    public void shipOrder(String orderId) {
        System.out.println("Shipping with FedEx for Order: " + orderId);
    }
}

public class BluedartShipment implements ShipmentService {
    public void shipOrder(String orderId) {
        System.out.println("Shipping with Bluedart for Order: " + orderId);
    }
}
```

---

### 🔸 Step 3: Create the Factory

```java
public class ShipmentFactory {
    public static ShipmentService getShipmentService(String carrier) {
        return switch (carrier.toLowerCase()) {
            case "fedex" -> new FedExShipment();
            case "bluedart" -> new BluedartShipment();
            default -> throw new IllegalArgumentException("Unsupported carrier: " + carrier);
        };
    }
}
```

---

### 🔸 Step 4: Client Code

```java
public class OrderService {
    public void dispatchOrder(String orderId, String carrier) {
        ShipmentService shipment = ShipmentFactory.getShipmentService(carrier);
        shipment.shipOrder(orderId);
    }
}
```

✅ Now, adding a new carrier is as simple as:

* Creating a new class `DHLShipment` implementing `ShipmentService`
* Adding one line in the factory

---

## ✅ Real-World Use Case in Supply Chain

### Use Case: Dynamic Notification Service (Email/SMS/Push)

```java
// Interface
public interface NotificationService {
    void notifyUser(String message, String userId);
}

// Implementations
public class EmailNotification implements NotificationService { ... }
public class SmsNotification implements NotificationService { ... }
public class PushNotification implements NotificationService { ... }

// Factory
public class NotificationFactory {
    public static NotificationService getService(String type) {
        return switch (type.toLowerCase()) {
            case "email" -> new EmailNotification();
            case "sms" -> new SmsNotification();
            case "push" -> new PushNotification();
            default -> throw new RuntimeException("Unsupported type");
        };
    }
}
```

---

## ✅ Factory in Spring Boot Production Code

```java
@Component
public class PaymentServiceFactory {

    @Autowired
    private List<PaymentProcessor> processors;  // All beans implementing interface

    public PaymentProcessor getProcessor(String mode) {
        return processors.stream()
            .filter(p -> p.supports(mode))
            .findFirst()
            .orElseThrow(() -> new RuntimeException("No processor for mode " + mode));
    }
}
```

🧠 Use Spring’s `@Component` or `@Service` + `@Qualifier` for auto-wiring factories.

---

## 📦 Low-Level Design (LLD) Problem Using Factory

---

### 🧩 LLD Problem: Design Order Payment System for Multiple Payment Modes

#### Requirements:

* Accept payment via Razorpay, PayPal, or Stripe
* Support adding more gateways without changing client logic

---

### Step 1: Payment Processor Interface

```java
public interface PaymentProcessor {
    void pay(String orderId, double amount);
    boolean supports(String mode); // Optional for Spring use
}
```

---

### Step 2: Concrete Implementations

```java
public class RazorpayProcessor implements PaymentProcessor {
    public void pay(String orderId, double amount) {
        System.out.println("Paying via Razorpay for " + orderId);
    }
    public boolean supports(String mode) {
        return "razorpay".equalsIgnoreCase(mode);
    }
}

public class StripeProcessor implements PaymentProcessor {
    public void pay(String orderId, double amount) {
        System.out.println("Paying via Stripe for " + orderId);
    }
    public boolean supports(String mode) {
        return "stripe".equalsIgnoreCase(mode);
    }
}
```

---

### Step 3: Factory

```java
public class PaymentProcessorFactory {
    public static PaymentProcessor getProcessor(String mode) {
        return switch (mode.toLowerCase()) {
            case "razorpay" -> new RazorpayProcessor();
            case "stripe" -> new StripeProcessor();
            default -> throw new IllegalArgumentException("Unsupported payment mode");
        };
    }
}
```

---

### Step 4: Client Code

```java
public class OrderPaymentService {
    public void process(String orderId, double amount, String mode) {
        PaymentProcessor processor = PaymentProcessorFactory.getProcessor(mode);
        processor.pay(orderId, amount);
    }
}
```

---

## 🧾 Quick Revision Points

* **Factory = Object creation delegated to a central method**
* Promotes **OCP (Open-Closed Principle)** → Add new classes without touching old ones
* Prevents **if-else hell** in client code
* Makes codebase **pluggable and extensible**
* Used widely in **payment**, **shipping**, **notification**, **auth**, **event handling**

---

## 🧠 Interview Follow-Up Questions

| Question                                   | Sample Answer                                            |
| ------------------------------------------ | -------------------------------------------------------- |
| Why use Factory over direct instantiation? | Decouples creation logic, promotes maintainability       |
| How to make Factory extensible?            | Use reflection, configuration, or Spring context         |
| Is Factory pattern always static?          | No, can be instance-based (Spring uses this model)       |
| What are drawbacks?                        | Can grow large; improper design leads to rigid structure |
| How is this used in Spring Boot?           | Spring IoC Container is a Factory of Beans               |

---

Would you like to move to **Strategy**, **Builder**, or **Observer** next — or see a **system design use case (like Order Processing System)** where we use Factory + Singleton + Strategy together?
