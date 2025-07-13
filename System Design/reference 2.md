Below is your **Comprehensive System Design Study Reference**—a single guide covering **HLD frameworks**, **core patterns**, **design principles**, **database concepts** (ACID, CAP, locking, CQRS), **messaging**, **caching**, and **trade-offs**, all illustrated with **real-world retail/supply-chain examples** (Target-style). Use this as your final revision before tomorrow’s interview.

---

# 📘 System Design Study Reference for Target Senior Engineer

---

## 1. 🧭 High-Level Design (HLD) Framework

1. **Clarify Requirements**

    * **Functional**: e.g., real-time inventory updates, order placement, shipping pipeline, analytics dashboards
    * **Non-Functional**: QPS (e.g., 5K writes/sec), latency (≤100 ms), availability (≥99.9%), consistency (strong vs eventual)
    * **Integrations**: Order Service, Warehouse Management, External Carriers, Fraud Analysis

2. **Define APIs & Data Models**

    * **Write**: `POST /inventory/update { warehouseId, sku, deltaQty, ts }`
    * **Read**: `GET /inventory/sku/{sku}` → returns cached or DB value
    * **Event Schema** (Kafka):

      ```json
      { "warehouseId":"W01", "sku":"SKU123", "delta":-2, "ts":"2025-06-23T10:00:00Z" }
      ```

3. **Component Breakdown**

    * **Edge**: Nginx → **API Gateway** (SSL termination, auth, rate-limit)
    * **Load Balancer**: Round-Robin or Session-Sticky
    * **Microservices**: InventoryService, OrderService, ShippingService
    * **Messaging**: Kafka topics (`inventory-updates`, `order-events`)
    * **Datastores**:

        * **Primary DB** (SQL or NoSQL, sharded per service)
        * **Read Replicas** for scale
        * **Cache** (Redis per service)
    * **Observability**: ELK (logs), Prometheus/Grafana (metrics), Jaeger (tracing)

4. **Data Flow**

    * **Write Path**:

        1. API → InventoryService.validate()
        2. DB write (optimistic lock)
        3. Publish Kafka event
        4. Cache update & analytics store
    * **Read Path**:

        1. API → Redis cache
        2. On miss → Read-Replica DB → populate cache

5. **Scaling & Availability**

    * **Horizontal** scaling for stateless services
    * **Sharding** by key (`warehouseId` or `sku % N`)
    * **Cache Clustering** (Redis Cluster)
    * **Multi-AZ** deployments, health checks, auto-failover

6. **Reliability & Consistency**

    * **Idempotency** (client-generated request IDs)
    * **Optimistic Locking** (`version` column) for concurrent stock updates
    * **Eventual Consistency** for read views, **strong** for write validations
    * **Retry** with exponential backoff on transient failures

7. **Monitoring & Alerts**

    * Track API latencies, error rates, Kafka consumer lag
    * Dashboards for inventory freshness, SLA breaches
    * Alerts on high error spikes, downstream slowdowns

---

## 2. ⚙️ Core Design Patterns

| Pattern                     | Purpose & Example                                             |
| --------------------------- | ------------------------------------------------------------- |
| **Singleton**               | Single instance: `ConfigService`, `KafkaProducer`             |
| **Factory**                 | Instantiate strategies: `ShippingFactory.get("fedex")`        |
| **Strategy**                | Runtime behavior: `DiscountStrategy`, `LoadBalancingStrategy` |
| **Observer**                | Event notifications: `OrderEventPublisher` → `Email/Kafka`    |
| **Chain of Responsibility** | Validation pipeline: payment → inventory → fraud → place      |
| **Builder**                 | Complex DTOs: `new Order.Builder().id(…)…build()`             |
| **Template Method**         | Ordered steps: `OrderProcessor.process()` with hook methods   |

---

## 3. 🔐 Design Principles (SOLID + Beyond)

| Principle                        | Key Idea & Retail Use Case                                                                   |
| -------------------------------- | -------------------------------------------------------------------------------------------- |
| **SRP**                          | One class = one reason to change (e.g., separate `InventoryValidator`, `OrderPersister`)     |
| **OCP**                          | Open to extension, closed to modification (add new `ShippingStrategy` without changing core) |
| **LSP**                          | Subtypes substitutable (mock and real `PaymentGateway` behave same)                          |
| **ISP**                          | Fine-grained interfaces (`EmailNotifier` vs `SMSNotifier`)                                   |
| **DIP**                          | Depend on abstractions (inject `Notifier`, not `EmailService`)                               |
| **Composition over Inheritance** | Combine `CourierService`, `Logger`, `Notifier` in `ShipmentService`                          |

---

## 4. 🛡️ Database Concepts & Locking

### 4.1 ACID Transactions

* **Atomicity**: All or none (e.g., order + payment)
* **Consistency**: DB rules hold (e.g., stock ≥ 0)
* **Isolation**: Levels (see 4.3)
* **Durability**: Committed data survives crashes

### 4.2 CAP Theorem

* **Consistency**: All readers see latest write
* **Availability**: Every request gets a response
* **Partition Tolerance**: System continues under network split

> **Pick two**:
>
> * **CP**: Bank transactions (prioritize consistency)
> * **AP**: Chat systems (prioritize availability)

### 4.3 Isolation Levels & MVCC

| Level            | Dirty Read | Non-Repeatable Read | Phantom Read | Use Case                             |
| ---------------- | ---------- | ------------------- | ------------ | ------------------------------------ |
| Read Uncommitted | Yes        | Yes                 | Yes          | Rarely used                          |
| Read Committed   | No         | Yes                 | Yes          | Default (Postgres, MySQL)            |
| Repeatable Read  | No         | No                  | Yes          | Moderate isolation                   |
| Serializable     | No         | No                  | No           | Strict consistency, lower throughput |

MVCC enables non-blocking reads by maintaining multiple versions.

### 4.4 Locking Mechanisms

| Type                     | How It Works                                                 | When to Use                            |
| ------------------------ | ------------------------------------------------------------ | -------------------------------------- |
| **Optimistic Lock**      | `UPDATE … WHERE version=?`; fail & retry on mismatch         | Low-contention updates (stock refills) |
| **Pessimistic Lock**     | `SELECT … FOR UPDATE` (row-level lock)                       | High-contention, critical resources    |
| **Advisory Locks**       | App-level named locks (`pg_advisory_lock`)                   | Distributed coordination (batch jobs)  |
| **Table/Row/Page Locks** | DB-internal; page locks group rows, table locks entire table | Rare; coarse access control            |

---

## 5. 🔄 CQRS & Event-Driven Architecture

### 5.1 CQRS (Command Query Responsibility Segregation)

* **Commands** (writes) go to a **command model** (normalized, transactional)
* **Queries** (reads) go to a **query model** (denormalized, optimized)
* Example:

    * **Write**: `POST /order` updates relational DB + publishes `OrderCreatedEvent`
    * **Read**: `GET /order/{id}` reads from a fast, denormalized view (Elasticsearch or Redis)

### 5.2 Event-Driven Patterns

* **Event Bus** (Kafka) for decoupling producers/consumers
* **Sagas** for multi-step distributed transactions (e.g., order → payment → shipment)
* **Idempotency** and **retry** policies ensure resilience

---

## 6. ⚡ Caching & Data Stores

* **Redis** per microservice for hot data (inventory, pricing)
* **Cache Patterns**:

    * **Cache Aside**: App manages cache population/invalidation
    * **Read-Through/Write-Through**: DB proxy into cache
* **TTL** or **Event-Based Invalidation** for staleness control
* **NoSQL** (DynamoDB, Cassandra) for high write QPS, flexible schema
* **SQL** (Postgres, MySQL) for transactional consistency (orders, payments)

---

## 7. 🗂️ Stored Procedures & Triggers

* **Stored Procedures** for bulk operations (e.g., nightly inventory adjustments)
* **Triggers** for audit logs or maintaining denormalized tables
* **Trade-Off**: Performance vs. maintainability/version control

---

## 8. 📋 Final Revision Checklist

1. **HLD Flow**: Requirements → APIs → Components → DataFlow → Scaling
2. **Patterns**: Know intent, code sketch, real use cases
3. **Principles**: SOLID + Composition, DRY, YAGNI, KISS
4. **DB Concepts**: ACID, CAP, Isolation, Locking, Sharding
5. **CQRS & Messaging**: Commands vs Queries, Kafka/Saga patterns
6. **Caching**: Patterns, invalidation, TTL
7. **Stored Logic**: When to use procs/triggers
8. **Trade-Off Language**: Always justify “why X and accept Y”

> 🔔 **Interview Tip**: For every decision, state:
> “I choose **X** because it provides **A**, and I accept the trade-off **B** given our constraints.”

---

Use this guide to **walk through your whiteboard** and **verbal explanations**, and you’ll be ready to impress in your **Target Corporation system design round**. Good luck, Eshwar!
