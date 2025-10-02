Perfect — let’s break down **Non-Relational Databases (NoSQL)** in a structured way following your requested format.

---

# 📚 Non-Relational Databases (NoSQL) – Complete Guide

---

## 1. **Types & Category**

Non-relational databases are databases that do **not follow strict relational tables (rows & columns)** like SQL databases. They are designed for **flexibility, scalability, and handling massive amounts of unstructured or semi-structured data**.

### Main Categories of NoSQL:

1. **Key-Value Stores** – Data stored as simple key → value pairs.

    * Example: **Redis, DynamoDB**
2. **Document Stores** – Data stored in JSON/BSON-like documents.

    * Example: **MongoDB, CouchDB**
3. **Wide-Column Stores** – Data stored in tables, but with flexible columns (optimized for fast writes).

    * Example: **Cassandra, HBase**
4. **Graph Databases** – Data stored as nodes & edges to represent relationships.

    * Example: **Neo4j, Amazon Neptune**

---

## 2. **Usage by Companies**

* **Amazon DynamoDB** → Used by **Amazon & Alexa** for low-latency shopping carts, product catalogs, and voice assistant interactions.
* **Netflix (Cassandra + DynamoDB)** → Manages **real-time viewing history, recommendations, and personalization** across millions of users.
* **Uber (Cassandra)** → Handles **trip data, surge pricing, geolocation services** with high availability.
* **Google (Bigtable)** → Powers **search indexing, Google Maps, YouTube analytics**.
* **Facebook (HBase + Cassandra)** → Stores **messaging data, social feeds**.
* **LinkedIn (Voldemort, Espresso, RocksDB)** → Manages **user profiles, feed ranking**.

---

## 3. **Data Storage Representation**

### Example:

### 🔑 Key-Value Store (Redis/DynamoDB)

```
Key: "user123"
Value: { "name": "Alice", "cart": ["item1", "item2"] }
```

### 📄 Document Store (MongoDB)

```json
{
  "userId": "user123",
  "name": "Alice",
  "orders": [
    { "id": 1, "item": "Phone", "price": 500 },
    { "id": 2, "item": "Shoes", "price": 100 }
  ]
}
```

### 📊 Wide-Column Store (Cassandra)

```
Row Key: user123
Columns:
    name → Alice
    email → alice@example.com
    lastLogin → 2025-09-30
```

### 🌐 Graph DB (Neo4j)

```
(Alice) -[FRIENDS_WITH]-> (Bob)
(Alice) -[PURCHASED]-> (Phone)
```

---

## 4. **Query Execution**

* **Key-Value Stores** – O(1) lookups using **hashing**.
* **Document Stores** – Querying via **indexes (B-Trees)** or **sharded collections**.
* **Wide-Column Stores** – Use **LSM Trees** for fast writes and **partition keys** for distributed queries.
* **Graph Databases** – Use **graph traversal algorithms** (e.g., BFS/DFS) for queries like "friends of friends".

👉 Example in MongoDB:

```js
db.users.find({ "orders.item": "Phone" })
```

---

## 5. **Scaling Techniques**

* **Vertical Scaling**: Increase RAM, CPU, disk on one server. (Limited)
* **Horizontal Scaling**: Distribute data across multiple servers (sharding/partitioning). (Preferred in NoSQL)

👉 **NoSQL databases are built for horizontal scaling**:

* Cassandra → Consistent hashing for sharding.
* MongoDB → Sharded clusters with routing.
* DynamoDB → Automatic partitioning based on partition keys.

---

## 6. **Replication & Failover**

* **Leader-Follower Replication**:

    * One write node (leader), multiple read replicas (followers).
    * If leader fails → election algorithm promotes a replica.

* **Techniques used**:

    * **Raft (etcd, MongoDB Atlas, CockroachDB)**
    * **Paxos (Cassandra, Google Spanner)**
    * **Zookeeper** for coordination (HBase, Kafka).

👉 Example:

* In **Cassandra**, every node can accept writes → no single leader (peer-to-peer model).
* In **MongoDB**, primary node = write node. If it fails → automatic election among secondaries.

---

## 7. **Data Sync Mechanism**

* **Synchronous Replication** → Writes committed only after replicas confirm. (Strong consistency, slower).
* **Asynchronous Replication** → Leader writes immediately, replicas update later. (Eventual consistency, faster).

👉 Example:

* DynamoDB → **Eventually consistent** reads by default, but can request **strongly consistent**.
* Cassandra → Uses **tunable consistency levels** (e.g., QUORUM, ONE, ALL).

---

## 8. **Business Use Cases & Advantages**

* **Amazon DynamoDB** → Shopping carts, recommendation engines. Advantage: **Single-digit millisecond latency** at any scale.
* **Netflix Cassandra** → Stores **real-time watch history & recommendations**. Advantage: **Global availability, fault tolerance**.
* **Uber Cassandra** → Handles **live trip status, location tracking**. Advantage: **Massive scale + always available**.
* **Google Bigtable** → Indexes **billions of web pages**. Advantage: **High throughput for analytics**.
* **Facebook HBase** → Stores **chat & feed data**. Advantage: **Handles petabytes of data**.

---

## 📊 Quick Revision (Bullet Points)

* **NoSQL Types**: Key-Value, Document, Wide-Column, Graph.
* **Scaling**: Horizontal scaling via sharding/partitioning.
* **Replication**: Leader-follower or peer-to-peer. Failover with Raft/Paxos.
* **Consistency**: Eventual vs Strong (tunable in Cassandra).
* **Used by Tech Giants**:

    * Amazon → DynamoDB (e-commerce, Alexa)
    * Netflix → Cassandra (recommendations)
    * Uber → Cassandra (real-time trips)
    * Google → Bigtable (search, maps)
    * Facebook → HBase (feeds, chat).

---

📌 Diagrammatic Representation (simplified):

```
          ┌───────────────┐
          │   Write Node  │
          └───────┬───────┘
                  │
      ┌───────────┼───────────┐
      │           │           │
┌─────▼─────┐┌────▼─────┐┌────▼─────┐
│ Read Node ││ Read Node ││ Read Node │
└───────────┘└───────────┘└───────────┘
      (Replicas sync via async/sync replication)
```

---

✅ This is the **complete structured view of Non-Relational (NoSQL) Databases**.

Would you like me to now create **side-by-side comparison with Relational Databases (SQL vs NoSQL)** for interview prep?
