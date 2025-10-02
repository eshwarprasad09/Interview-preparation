Great choice 🚀 Let’s go deep into **Key-Value Store Databases** with all the requested details.

---

# 📌 Key-Value Stores (NoSQL Database)

---

## 1. **Types & Category**

* **Category**: NoSQL
* **Type**: Key-Value Store
* **Definition**:

    * Simplest type of NoSQL database.
    * Data is stored as a collection of key-value pairs (like a giant hash map/dictionary).
    * Keys are unique identifiers, and values can be strings, JSON, binary objects, or documents.

👉 Examples: **Redis, Amazon DynamoDB, Riak, Memcached**

---

## 2. **Usage by Companies**

| Company     | Database Used                           | Purpose                                                                        |
| ----------- | --------------------------------------- | ------------------------------------------------------------------------------ |
| **Amazon**  | DynamoDB (built in-house)               | Shopping cart service, user sessions, product catalog.                         |
| **Netflix** | DynamoDB + EVCache (built on Memcached) | Store movie metadata, caching recommendations, low-latency streaming sessions. |
| **Uber**    | Redis                                   | Fast ride matching, surge pricing, caching driver/rider data.                  |
| **Twitter** | Redis & Memcached                       | Caching tweets, session data, timelines.                                       |
| **GitHub**  | Redis                                   | Job queues, caching pull requests, notifications.                              |
| **Twitch**  | DynamoDB                                | Chat messages, real-time streaming metadata.                                   |

---

## 3. **Data Storage Representation**

Data stored as **Key → Value** pair.

Example: **User Session Store**

```
Key: user:1001
Value: {
    "username": "Eshwar",
    "session_id": "xyz123",
    "cart_items": ["iPhone15", "MacBook"]
}
```

### Diagram (Simplified)

```
+--------------------+-----------------------------------+
| Key                | Value                             |
+--------------------+-----------------------------------+
| user:1001          | {username:"Eshwar", cart:[...]}   |
| product:2001       | {name:"iPhone", price:70000}      |
| order:3001         | {user:1001, items:["product:2001"]|
+--------------------+-----------------------------------+
```

---

## 4. **Query Execution**

* Queries are mostly **O(1)** lookups → like a **HashMap**.
* No joins or complex queries like SQL.
* Internals:

    * **Redis** → uses in-memory data structures + hash tables.
    * **DynamoDB** → uses **partitioning + consistent hashing** to distribute keys.
    * **LSM Trees** (for persistent stores) → used to handle writes efficiently.
* Query Types:

    * `GET key`
    * `PUT key value`
    * `DELETE key`

Example in Redis:

```bash
SET user:1001 "{name: 'Eshwar', balance: 1000}"
GET user:1001
```

---

## 5. **Scaling Techniques**

* **Vertical Scaling (Scale Up)**:

    * Increase server RAM/CPU to handle more data in memory (Redis).
    * Works for smaller workloads but limited by single machine.

* **Horizontal Scaling (Scale Out)**:

    * Data partitioned across nodes using **sharding**.
    * Consistent hashing ensures keys are evenly distributed.
    * DynamoDB auto-scales partitions as data grows.

---

## 6. **Replication & Failover**

* **Replication**:

    * Write node (Leader) replicates data to read replicas (Followers).
    * Example: Redis → *Primary-Replica replication*.

* **Failover (If write node fails)**:

    * Leader election happens → one replica promoted as the new leader.
    * Techniques used:

        * **Raft** (Redis Cluster, TiKV)
        * **Paxos** (Dynamo-style systems)
        * **Zookeeper** (used for coordination in some systems).

---

## 7. **Data Sync Mechanism**

* **Synchronous Replication**:

    * Write is acknowledged only after replicas confirm.
    * Safer but slower.

* **Asynchronous Replication**:

    * Write node acknowledges immediately and later sends data to replicas.
    * Faster but risk of data loss during failover.

👉 Redis offers both: **ASYNC replication by default**, but **WAIT command** can enforce sync.
👉 DynamoDB → uses *multi-AZ replication* for fault tolerance.

---

## 8. **Business Use Cases**

### How Tech Giants Use It:

* **Amazon**: DynamoDB for **shopping carts** – millions of users add/remove items concurrently.
* **Netflix**: Key-value caching for **movie metadata and recommendations** – ensures **<10ms latency**.
* **Uber**: Redis for **real-time ride matching** – must match rider/driver in milliseconds.
* **Twitter**: Caching tweets to avoid hitting relational DB millions of times per second.

### Business Advantages:

* **Low Latency**: Sub-millisecond lookups.
* **Scalability**: Handles millions of requests per second.
* **High Availability**: Auto-replication & failover keep services always running.
* **Cost-Effective**: Less complex schema, easy to scale horizontally.

---

## 9. **Revision (Bullet Points)**

* ✅ **Type**: NoSQL → Key-Value Store.
* ✅ **Examples**: Redis, DynamoDB, Memcached, Riak.
* ✅ **Companies**: Amazon (cart), Netflix (metadata), Uber (ride matching).
* ✅ **Data Model**: `Key → Value` (like HashMap).
* ✅ **Query**: `GET key`, `SET key value`.
* ✅ **Scaling**: Vertical (RAM up), Horizontal (Sharding).
* ✅ **Replication**: Primary → Replicas.
* ✅ **Failover**: Leader election (Raft, Paxos).
* ✅ **Sync**: Async (fast) vs Sync (safe).
* ✅ **Advantage**: Low latency, scalability, real-time performance.

---

⚡Would you like me to create the **same level of detail for Document Stores (e.g., MongoDB)** next, so we cover each NoSQL type one by one?
