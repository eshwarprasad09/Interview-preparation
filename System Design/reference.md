Below is your **Target Corporation System Design Study Reference Guide**, organized as a structured walkthrough—from high-level design (HLD) frameworks to low-level design (LLD) patterns, principles, and trade-offs. Use this as your one-stop revision before tomorrow’s interview.

---

# 📘 Target System Design Study Reference

---

## 1. 🧭 HLD Preparation Framework

1. **Clarify Requirements**

    * Functional: e.g., real-time inventory updates, order placement, shipping pipeline
    * Non-Functional: QPS, latency (SLA), consistency, availability, data volume
    * Integrations: OMS, WMS, external carriers, fraud service, analytics

2. **Define APIs & Data Models**

    * Write API (POST /inventory/update)
    * Read API (GET /inventory/sku/{sku})
    * Event schema (Kafka: {warehouseId, sku, delta, ts})

3. **Component Breakdown**

    * Edge: Nginx → API Gateway (SSL, auth, rate-limit)
    * Load Balancer (Round-Robin or Sticky by Session)
    * Service Layer: InventoryService, OrderService, ShippingService
    * Messaging: Kafka topics (inventory-updates, order-events)
    * Data Stores:

        * Write DB (NoSQL/SQL, sharded per microservice)
        * Read replicas for scaling reads
        * Cache (Redis per service)
    * Observability: ELK, Prometheus/Grafana, Jaeger

4. **Data Flow**

    * **Write Path**: API → Service → DB write → Kafka publish → Cache & Analytics update
    * **Read Path**: API → Cache lookup → Replica/DB fallback

5. **Scaling & Availability**

    * Horizontal scaling for stateless services
    * Sharding by key (warehouseId/sku)
    * Read replicas & cache clustering
    * Multi-AZ deployment for HA
    * Health checks & failover

6. **Reliability & Consistency**

    * **Idempotency** keys for API retries
    * **Optimistic locking** (version column) for concurrent writes
    * Eventual consistency acceptable for read views
    * Synchronous strong consistency for validations

7. **Monitoring & Alerts**

    * Track request latencies, error rates, queue lags
    * Dashboards for inventory freshness, consumer lag
    * Alerts on SLA breaches, high error spikes

---

## 2. ⚙️ Core Design Patterns & Where to Apply

| Pattern                     | Use Case                                                       |
| --------------------------- | -------------------------------------------------------------- |
| **Singleton**               | ConfigService, Logger, KafkaProducer                           |
| **Factory**                 | Instantiate ShippingProvider, PaymentGateway, DiscountStrategy |
| **Strategy**                | Load-balancing, Pricing/Discount logic, Shipping selection     |
| **Observer**                | Event notifications (EmailService, KafkaPublisher)             |
| **Chain of Responsibility** | Validation pipeline (payment → inventory → fraud → hold)       |
| **Builder**                 | Complex DTOs: OrderRequest, ShipmentRequest                    |
| **Template Method**         | Order processing flow (validate → enrich → persist → notify)   |

---

## 3. 🔬 Design Principles (SOLID + Beyond)

| Principle                        | Description & Target Use                                         |
| -------------------------------- | ---------------------------------------------------------------- |
| **SRP**                          | One class/Service = one reason to change (split OrderService)    |
| **OCP**                          | Open to extension, closed to modification (Strategy + Factory)   |
| **LSP**                          | Subtypes substitutable (mock providers implement same interface) |
| **ISP**                          | Fine-grained interfaces (EmailNotifier vs SMSNotifier)           |
| **DIP**                          | Depend on abstractions (inject Notifier, PaymentGateway)         |
| **Composition over Inheritance** | Compose CourierService, Logger, Notifier into ShipmentService    |

---

## 4. 🗺️ System Design Concepts & Trade-Offs

| Concept                            | Trade-Off & When to Use                                  |
| ---------------------------------- | -------------------------------------------------------- |
| **API Gateway**                    | +Centralized SSL/auth; –Latency, SPOF unless HA          |
| **Load Balancing**                 | Round-Robin (stateless); Sticky (session state)          |
| **SSL Termination**                | At API Gateway for simplicity; use mTLS internally       |
| **Cache (Redis)**                  | +Low latency; –Staleness (use TTL or invalidation)       |
| **DB Sharding**                    | +Write scale; –Cross-service joins                       |
| **Read Replicas**                  | +Scale reads; –Eventual consistency                      |
| **Optimistic vs Pessimistic Lock** | Optimistic for low contention; Pessimistic for hotspots  |
| **Sync API vs Async MQ**           | Sync for user actions; Async (Kafka) for downstream      |
| **SQL vs NoSQL**                   | SQL for ACID transactions; NoSQL for high throughput     |
| **Monolith vs Microservices**      | Monolith for simplicity; Microservices for team autonomy |

---

## 5. 🏗️ LLD Examples & Code Snippets

### 5.1 **Validation Pipeline (CoR)**

```java
OrderValidator payment = new PaymentValidator();
OrderValidator inventory = new InventoryValidator();
OrderValidator fraud    = new FraudValidator();
payment.setNext(inventory).setNext(fraud);
payment.validate(order);
```

### 5.2 **Shipping Factory + Strategy**

```java
public interface ShippingStrategy { void ship(Order o); }
public class FedEx  implements ShippingStrategy { /*...*/ }
public class UPS    implements ShippingStrategy { /*...*/ }
public class ShippingFactory {
  public static ShippingStrategy get(String carrier) { /*switch*/ }
}
// Runtime use:
ShippingStrategy strat = ShippingFactory.get("fedex");
strat.ship(order);
```

### 5.3 **Event Notification (Observer)**

```java
public interface OrderObserver { void onOrderCreated(Order o); }
subject.register(new EmailNotifier());
subject.register(new KafkaNotifier());
subject.notify(order);
```

### 5.4 **Object Construction (Builder)**

```java
Order order = new Order.Builder()
    .id("ORD1").items(list).shipping("ADDR").build();
```

### 5.5 **Process Template**

```java
abstract class OrderProcessor {
  public final void process(Order o) {
    validate(o); enrich(o); persist(o); notify(o);
  }
  protected abstract void validate(Order o);
  // ...
}
class B2CProcessor extends OrderProcessor { /*...*/ }
```

### 5.6 **Optimistic Locking**

* Add `version` column
* On update: `WHERE id=? AND version=?` → increment version
* If update count=0, retry or abort

---

## 6. 🗂️ Final Revision Checklist

1. **HLD Framework**: Requirements → APIs → Components → DataFlow
2. **Patterns**: Know intent, code sketch, real-world use
3. **Principles**: SOLID + Composition, DRY, YAGNI, KISS
4. **Concepts & Trade-Offs**: Be ready to explain “why X vs Y”
5. **LLD**: Drill into at least two pipelines (validation, notification)
6. **Mock Scenario**: Walk through Inventory System example end-to-end
7. **Diagrams**: Practice drawing on paper/whiteboard
8. **Clarifying Questions**: SLA, QPS, data volumes, failure modes

---

> 🔔 **Tip**: For every design choice, state:
> “I choose **X** because it gives me **A**, and I accept the trade-off **B**, given our constraints.”

Good luck, Eshwar—this guide encapsulates everything you need to ace your HLD and LLD round at Target!
