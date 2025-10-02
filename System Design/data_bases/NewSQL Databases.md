Got it 👍 Let’s dive deep into NewSQL Databases (with Google Spanner and CockroachDB as examples).

⸻

📌 NewSQL Databases

1. Types & Category
   •	Category: Relational + Distributed (Hybrid)
   •	Type: NewSQL database
   •	Combines:
   •	SQL (Relational model + ACID transactions) from traditional RDBMS
   •	Horizontal scalability + fault tolerance from NoSQL

👉 In short: They give SQL’s consistency + NoSQL’s scalability.

⸻

2. Usage by Companies

🔹 Google Spanner
•	Company: Google
•	Use case: Runs mission-critical systems like:
•	Google Ads (billions of ad transactions/day)
•	Google Play Store purchases
•	Google Cloud Spanner (offered as a service)

It ensures global consistency with low latency using Google’s TrueTime API (atomic clocks + GPS clocks).

🔹 CockroachDB
•	Company: Cockroach Labs (used by companies like DoorDash, Comcast, and Baidu)
•	Use case:
•	DoorDash: Handles real-time order placements and deliveries across cities with high availability.
•	Comcast: Customer data + streaming usage analytics with always-on performance.

⸻

3. Schema / Data Storage Representation
   •	Similar to relational DB (tables, rows, columns).
   •	Schema is relational → you define tables with columns & constraints (just like SQL).
   •	But internally:
   •	Data is stored in key-value ranges (like NoSQL).
   •	Partitioned automatically based on primary key ranges (sharding).
   •	Distributed across multiple nodes for availability & scaling.

Example (Table Creation in Spanner / CockroachDB):

CREATE TABLE Users (
user_id INT PRIMARY KEY,
name STRING(100),
email STRING(255) UNIQUE,
created_at TIMESTAMP
);

CREATE TABLE Orders (
order_id INT PRIMARY KEY,
user_id INT,
amount DECIMAL(10,2),
created_at TIMESTAMP,
FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

✅ Looks like a SQL schema, but runs distributed & globally consistent.

⸻

4. Complex Data Example

Imagine a global e-commerce platform (like Amazon).
•	Users: millions worldwide
•	Orders: billions/year
•	Inventory: across multiple warehouses

Storage in NewSQL:
•	Users Table (partitioned by user_id → spreads across regions).
•	Orders Table (partitioned by order_id → distributed).
•	Inventory Table (partitioned by product_id).

📌 Why? → So that no single node is overloaded. Data is automatically sharded + replicated globally.

Retrieval:
•	Query: SELECT * FROM Orders WHERE user_id = 123;
•	DB looks at partitioned ranges → finds the right shard → retrieves in milliseconds.

⸻

5. Comparison with Other Databases

Feature	SQL (MySQL, Postgres)	NoSQL (MongoDB, Cassandra)	NewSQL (Spanner, CockroachDB)
Data Model	Relational (tables)	Document/Key-Value/Wide-Column	Relational (tables)
Consistency (ACID)	✅ Strong	❌ Weak/Eventual	✅ Strong (Distributed ACID)
Scalability	❌ Vertical only	✅ Horizontal	✅ Horizontal
Global Distribution	❌ Limited	✅ Good	✅ Best (automatic & consistent)
Query Language	SQL	Custom APIs / JSON Query	SQL
Best For	Small-medium apps	Big data, analytics, IoT	Global, real-time, transactional apps


⸻

6. Query Execution Internals
   •	Uses MVCC (Multi-Version Concurrency Control) for transactions.
   •	Relies on consensus protocols (Paxos, Raft) to maintain global consistency.
   •	Google Spanner → TrueTime API: Combines atomic clocks + GPS satellites to guarantee global ordering of transactions.

⸻

7. Scaling Techniques
   •	Horizontal scaling: Nodes are added dynamically.
   •	Data is partitioned automatically based on primary keys.
   •	Transactions are distributed across nodes with consensus.

⸻

8. Replication & Failover
   •	Replication: Multi-region replication (synchronous or near-synchronous).
   •	Failover: If a leader node fails, Raft/Paxos elects a new leader automatically.
   •	Ensures zero downtime.

⸻

9. Data Sync Mechanism
   •	Synchronous replication for strong consistency (writes confirmed across majority before commit).
   •	Asynchronous replication for read scalability in some cases.

⸻

10. Business Use Cases
    •	Google Spanner: Ads, Play Store, global services (billions of transactions/day).
    •	DoorDash (CockroachDB): Low-latency global delivery system.
    •	Comcast: Always-on customer data services.

👉 Business Advantage:
•	Can serve millions of global users with:
•	Strong consistency
•	High availability
•	Low latency

⸻

11. Python Code Example

import psycopg2

# Connect to CockroachDB (Postgres driver works since CockroachDB uses SQL)
conn = psycopg2.connect(
dbname="bank",
user="root",
password="",
host="localhost",
port=26257
)
cur = conn.cursor()

# Create table
cur.execute("""
CREATE TABLE IF NOT EXISTS Accounts (
id SERIAL PRIMARY KEY,
name STRING,
balance DECIMAL
);
""")

# Insert data
cur.execute("INSERT INTO Accounts (name, balance) VALUES ('Alice', 1000.00)")
cur.execute("INSERT INTO Accounts (name, balance) VALUES ('Bob', 500.00)")

# Query data
cur.execute("SELECT * FROM Accounts")
rows = cur.fetchall()
for row in rows:
print(row)

conn.commit()
cur.close()
conn.close()


⸻

12. Revision Summary
    •	Type: NewSQL (Relational + Distributed).
    •	Storage Model: Tables (like SQL) but distributed across nodes.
    •	Query Execution: Uses MVCC, Raft/Paxos, TrueTime (Spanner).
    •	Scaling: Horizontal (automatic sharding).
    •	Replication: Multi-region, synchronous for strong consistency.
    •	Data Sync: Consensus protocols ensure sync.
    •	Business Use: Global-scale apps (Ads, e-commerce, delivery systems).
    •	Comparison: Strong consistency (better than NoSQL), scalability (better than SQL).
    •	Best For: Mission-critical, globally distributed applications needing ACID + scale.

⸻

👉 Would you like me to make a real-world LinkedIn-style case study (like we did for Graph DB) to explain how LinkedIn posts + reactions + comments could be modeled in NewSQL (Spanner/CockroachDB) for clarity?