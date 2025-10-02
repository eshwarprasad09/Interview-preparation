Perfect 👍 — you’re asking for revision notes on Wide-Column Stores (Cassandra) that also include:
•	DDL queries (table creation, insertion)
•	Rich example dataset (not just one or two rows, but multiple covering real scenarios)
•	Storage representation (how Cassandra organizes data internally)
•	Comparison with SQL & MongoDB

Let me build this like your study notes 📒.

⸻

📘 Revision Notes: Wide-Column Stores (Cassandra)

⸻

1. Type & Category
   •	Category: NoSQL → Wide-Column Store
   •	Similar To: Key-Value store (but with column families)
   •	Different From:
   •	SQL (row-oriented, fixed schema)
   •	MongoDB (document-oriented, flexible JSON)

⸻

2. Schema Design (DDL Example)

Suppose Netflix user activity tracking system: we want to log every user’s movie interactions (Play, Pause, Stop, Like, Rate).

-- Keyspace (like a DB in SQL)
CREATE KEYSPACE netflix
WITH replication = {
'class': 'SimpleStrategy',
'replication_factor': 3
};

-- Table for user activity
CREATE TABLE netflix.user_activity (
user_id      UUID,            -- Partition Key
activity_time TIMESTAMP,      -- Clustering Key
movie_id     INT,
action       TEXT,
device       TEXT,
rating       INT,
location     TEXT,
PRIMARY KEY (user_id, activity_time)
) WITH CLUSTERING ORDER BY (activity_time DESC);

🔑 Explanation
•	Partition Key = user_id → all activities of one user stored together.
•	Clustering Key = activity_time → activities ordered by time (latest first).
•	Columns = movie_id, action, device, rating, location → extra attributes.

⸻

3. Example Data Insert

INSERT INTO netflix.user_activity
(user_id, activity_time, movie_id, action, device, rating, location)
VALUES (1111, '2025-10-01 10:00:00', 101, 'Play', 'Mobile', NULL, 'Bangalore');

INSERT INTO netflix.user_activity
(user_id, activity_time, movie_id, action, device, rating, location)
VALUES (1111, '2025-10-01 10:10:00', 101, 'Pause', 'Mobile', NULL, 'Bangalore');

INSERT INTO netflix.user_activity
(user_id, activity_time, movie_id, action, device, rating, location)
VALUES (1111, '2025-10-01 10:15:00', 101, 'Rate', 'Laptop', 5, 'Bangalore');

INSERT INTO netflix.user_activity
(user_id, activity_time, movie_id, action, device, rating, location)
VALUES (2222, '2025-10-01 09:55:00', 102, 'Play', 'SmartTV', NULL, 'Delhi');

INSERT INTO netflix.user_activity
(user_id, activity_time, movie_id, action, device, rating, location)
VALUES (2222, '2025-10-01 10:20:00', 103, 'Like', 'Mobile', NULL, 'Delhi');


⸻

4. Internal Data Storage Representation

👤 Partition: user_id = 1111

user_id = 1111
2025-10-01 10:15:00 → { movie_id=101, action=Rate, device=Laptop, rating=5, location=Bangalore }
2025-10-01 10:10:00 → { movie_id=101, action=Pause, device=Mobile, location=Bangalore }
2025-10-01 10:00:00 → { movie_id=101, action=Play, device=Mobile, location=Bangalore }

👤 Partition: user_id = 2222

user_id = 2222
2025-10-01 10:20:00 → { movie_id=103, action=Like, device=Mobile, location=Delhi }
2025-10-01 09:55:00 → { movie_id=102, action=Play, device=SmartTV, location=Delhi }

🔑 Why efficient?
•	Single lookup by user_id → directly lands in one partition.
•	Activities already ordered by activity_time.

⸻

5. Query Examples

Find all actions of user 1111

SELECT * FROM netflix.user_activity
WHERE user_id = 1111;

👉 Cassandra jumps to partition of user 1111 and streams results.

⸻

Find user 1111’s activity on a specific date

SELECT * FROM netflix.user_activity
WHERE user_id = 1111
AND activity_time >= '2025-10-01 00:00:00'
AND activity_time < '2025-10-02 00:00:00';

👉 Uses clustering key (activity_time) to filter quickly within the partition.

⸻

Find latest action by user 2222

SELECT * FROM netflix.user_activity
WHERE user_id = 2222
LIMIT 1;

👉 Since clustering is DESC, the latest activity is fetched directly.

⸻

6. Comparison with SQL & MongoDB

Feature / DB	Cassandra (Wide-Column)	SQL (RDBMS)	MongoDB (Document)
Schema	Fixed table, but wide/flexible columns	Strict schema, normalized	Flexible JSON (nested docs)
Data Organization	Partition (user_id) → clustering (time) → values	Rows in table	JSON doc per user/activity
Query Model	Query by partition key → very fast	Uses indexes or scans	Query by doc fields
Example Storage	user_id=1111 → {time1: {movie, action}, time2: {…}}	Table rows: (user, movie, action, time)	Document: { user:1111, activities:[...] }
Best For	Write-heavy, time-series, scale-out	Strong joins, transactions	Semi-structured, flexible schema
Weakness	No joins, limited ad-hoc queries	Slower at scale, expensive joins	Can bloat documents, slower for analytics


⸻

7. Why Cassandra is Efficient Here
   •	Cassandra: Groups all activities of a user together → direct partition lookup → O(1) access pattern.
   •	SQL: Scans rows or uses indexes, but scattered rows can cause more I/O.
   •	MongoDB: Stores one user’s activities as an array in a document. Fast for user fetch, but updating nested arrays gets costly if very large.

⸻

8. Summary Points for Revision
   •	Type: Wide-column NoSQL DB.
   •	Schema Design: Partition key groups related data, clustering key orders it.
   •	Query Execution: Direct partition access (no table scans).
   •	Scaling: Horizontally via consistent hashing across nodes.
   •	Replication: Multiple replicas, eventual consistency.
   •	Why Better than SQL: No row scanning; optimized for write-heavy + time-series.
   •	Why Different from MongoDB: Stores data in partitions/columns vs JSON docs.
   •	Use Case: User activity tracking, IoT data, logs, recommendation events.

⸻

👉 Would you like me to also add MongoDB DDL + Example JSON documents for the same Netflix dataset so you can directly compare side by side how Cassandra vs MongoDB vs SQL would look in code form?