Perfect 👌 Let’s start with **Relational Databases (SQL / RDBMS)** and build a detailed **MD file** with all the points you asked:

---

# 📚 Relational Databases (SQL / RDBMS)

---

## 🔹 Overview

* Relational Databases store data in **tables (rows & columns)**.
* They use **SQL (Structured Query Language)** for queries.
* Ensure **ACID properties** (Atomicity, Consistency, Isolation, Durability).
* Widely used in **banking, finance, ERP, e-commerce, CRM, healthcare**.

---

## 🔹 Examples & Companies

* **MySQL** → Facebook (early days, user profiles & social graph).
* **PostgreSQL** → Netflix (billing, subscriptions).
* **Oracle** → Banks & Airlines (transactions, ticket booking).
* **MS SQL Server** → Walmart (inventory, sales tracking).

---

## 🔹 How Data is Stored

* Data is stored in **tables with rows and columns**.
* Example: `Users` and `Orders` tables.

```sql
-- Users Table
+---------+----------+-------------+
| user_id | name     | email       |
+---------+----------+-------------+
| 1       | Alice    | a@gmail.com |
| 2       | Bob      | b@gmail.com |

-- Orders Table
+---------+----------+------------+
| order_id| user_id  | product    |
+---------+----------+------------+
| 101     | 1        | iPhone     |
| 102     | 2        | Laptop     |
```

---

## 🔹 Querying the Data

To fetch **user details with their orders**:

```sql
SELECT u.name, u.email, o.product
FROM Users u
JOIN Orders o ON u.user_id = o.user_id
WHERE u.user_id = 1;
```

**Result:**

```
+-------+-------------+---------+
| name  | email       | product |
+-------+-------------+---------+
| Alice | a@gmail.com | iPhone  |
```

---

## 🔹 Python API Example

Using **SQLAlchemy**:

```python
from sqlalchemy import create_engine, text

# Connect to DB
engine = create_engine("postgresql://user:password@localhost:5432/mydb")

# Query user orders
with engine.connect() as conn:
    result = conn.execute(text("""
        SELECT u.name, u.email, o.product
        FROM Users u
        JOIN Orders o ON u.user_id = o.user_id
        WHERE u.user_id = :uid
    """), {"uid": 1})

    for row in result:
        print(dict(row))
```

**Output:**

```json
{"name": "Alice", "email": "a@gmail.com", "product": "iPhone"}
```

---

## 🔹 How Database Works Internally to Fetch Data

1. **Parser** → Parses SQL query into tokens.
2. **Query Optimizer** → Chooses best execution plan (indexes, joins, etc.).
3. **Executor** → Runs the plan, fetches rows.
4. **Storage Engine** → Reads data blocks from disk (with caching).

If an **index exists** (e.g., on `user_id`), query becomes **faster** since DB doesn’t scan full table.

---

## 🔹 Scalability

* **Vertical Scaling (Scale-Up):** Add more CPU/RAM to single server.
* **Horizontal Scaling (Scale-Out):** Use **read replicas** and **sharding**.

### 🔹 Read/Write Architecture

* **Write Node (Primary)** → Accepts writes.
* **Read Nodes (Replicas)** → Serve reads (replicated from primary).
* Replication can be:

    * **Synchronous** → Strong consistency, slower.
    * **Asynchronous** → Eventual consistency, faster.

---

## 🔹 Failover (When Primary Fails)

* **Technique:** **Leader Election** using consensus algorithms (e.g., Raft, Paxos, or DB’s own HA manager).
* One of the read replicas is promoted to be the **new primary**.
* Other replicas sync from the new primary.

**Example:**

* In **PostgreSQL** → `Patroni` or `PgPool` manages failover.
* In **MySQL** → `Group Replication` or `ProxySQL` handles failover.

---

## 🔹 Summary

* ✅ Best for **structured, relational data**.
* ✅ Excellent for **transactions (ACID)**.
* ✅ Supports **joins and complex queries**.
* ❌ Harder to scale horizontally compared to NoSQL.
* ⚡ Uses **replication + failover** for HA, but large-scale writes are challenging.

---

👉 Next, do you want me to create a **similar MD file for Cassandra (Column Store)**, showing how data is stored, queried, and scaled?
