Absolutely! Let’s deep-dive into **Document Stores** in the same structured format as the Key-Value section.

---

# 📌 Document Stores (NoSQL Database)

---

## 1. **Types & Category**

* **Category**: NoSQL
* **Type**: Document Store
* **Definition**:

    * Stores data as **documents**, typically in **JSON, BSON, or XML** format.
    * Each document is **self-contained**, can have **nested structures**, arrays, or varied fields.
    * Ideal for **semi-structured data** where schema can evolve over time.

👉 Examples: **MongoDB, CouchDB, RavenDB, ArangoDB (multi-model)**

---

## 2. **Usage by Companies**

| Company      | Database Used                        | Purpose                                                     |
| ------------ | ------------------------------------ | ----------------------------------------------------------- |
| **eBay**     | MongoDB                              | Stores product catalog with varying attributes per product. |
| **Uber**     | Couchbase                            | Stores user profiles, ride requests, and metadata.          |
| **LinkedIn** | Document Store (Voldemort/Couchbase) | Caches user feeds and messaging data.                       |
| **Adobe**    | MongoDB                              | Stores configuration and asset metadata for creative apps.  |
| **Expedia**  | MongoDB                              | Stores hotel, flight, and travel package details.           |

---

## 3. **Data Storage Representation**

Documents store data as key-value pairs **inside a document**, often with nested structures.

### Example: User Profile in MongoDB

```json
{
  "_id": "user123",
  "name": "Eshwar",
  "email": "eshwar@example.com",
  "orders": [
    { "orderId": 101, "item": "iPhone 15", "price": 1200 },
    { "orderId": 102, "item": "MacBook", "price": 2500 }
  ],
  "preferences": {
    "theme": "dark",
    "language": "en"
  }
}
```

### Diagram (Simplified)

```
+-----------+------------------------------------------------+
| Key       | Value                                          |
+-----------+------------------------------------------------+
| _id       | user123                                        |
| name      | Eshwar                                         |
| email     | eshwar@example.com                             |
| orders    | [{orderId:101, item:iPhone 15,...}, ...]      |
| prefs     | {theme:dark, language:en}                     |
+-----------+------------------------------------------------+
```

---

## 4. **Query Execution**

* Queries are **document-centric**, not table-centric.
* Can filter on nested fields and arrays.
* Indexing:

    * **B-Trees** for normal fields
    * **Compound indexes** for multiple fields
    * **TTL indexes** for expiring data
* Partitioning/Sharding:

    * Documents are partitioned by a **shard key** (e.g., `_id` or `userId`) to distribute data across nodes.

### Example Query (MongoDB)

```js
// Find orders of user123 with price > 1000
db.users.find({
  "_id": "user123",
  "orders.price": { $gt: 1000 }
})
```

---

## 5. **Scaling Techniques**

* **Vertical Scaling**:

    * Increase RAM/CPU of a single MongoDB instance for more memory & caching.
* **Horizontal Scaling (Preferred)**:

    * **Sharding**: Data partitioned across multiple nodes by shard key.
    * **Replica Sets**: Each shard can have multiple nodes for replication & fault tolerance.

---

## 6. **Replication & Failover**

* **Replication**:

    * MongoDB uses **Replica Sets**: 1 primary (write node) + multiple secondaries (read nodes).
    * Primary handles writes; secondaries replicate changes.
* **Failover**:

    * If primary fails → automatic **leader election** among secondaries.
    * Election algorithm: **Raft consensus protocol**.
    * Once new primary is elected, write operations continue seamlessly.

---

## 7. **Data Sync Mechanism**

* **Asynchronous Replication**:

    * Writes are applied to primary first, then asynchronously pushed to secondaries.
    * Slight delay possible → eventual consistency for reads from secondaries.
* **Synchronous Replication** (optional in some setups):

    * Write is acknowledged after at least **N secondaries** confirm.
    * Ensures strong consistency but slower writes.

---

## 8. **Business Use Cases**

### Real-World Examples:

* **eBay**: Product catalogs have **varying attributes**; Document DB allows schema flexibility.
* **Uber**: Stores **ride metadata** like location, driver info, and nested trip details.
* **Expedia**: Travel packages with optional fields per hotel/flight; flexible structure needed.
* **Adobe**: Stores user assets and preferences with evolving schemas for apps like Photoshop.

### Advantages:

* Flexible schema → evolve data without downtime
* Supports nested & complex objects
* Easy horizontal scaling for **millions of users**
* Fast reads/writes for **semi-structured data**

---

## 9. **Revision (Bullet Points)**

* ✅ **Type**: NoSQL → Document Store
* ✅ **Examples**: MongoDB, CouchDB, Couchbase, RavenDB
* ✅ **Companies**: eBay, Uber, Expedia, Adobe
* ✅ **Data Model**: Self-contained documents (JSON/BSON)
* ✅ **Query**: Field queries, nested queries, array queries
* ✅ **Scaling**: Vertical (RAM/CPU up), Horizontal (Sharding + Replica Sets)
* ✅ **Replication**: Primary → Secondaries
* ✅ **Failover**: Leader election (Raft)
* ✅ **Sync**: Async by default, optional sync for strong consistency
* ✅ **Business Advantage**: Flexible schema, fast reads/writes, easy scaling

---

### Diagram: Replica Set + Sharding

```
Shard 1             Shard 2
+---------+         +---------+
| Primary |         | Primary |
| Secondary |       | Secondary|
| Secondary |       | Secondary|
+---------+         +---------+
        \               /
        Cross-shard query router
```

---

⚡ Next step: I can create **Wide-Column Stores (Cassandra/HBase)** with the same depth and diagrams, including **scaling, replication, and query execution**.

Do you want me to do that next?
