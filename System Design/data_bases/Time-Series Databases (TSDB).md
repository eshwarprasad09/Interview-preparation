Perfect 👍 Let’s take a real-world application and walk through how a Time-Series Database (TSDB) is used, how data is stored, and why it’s better than SQL/Mongo/Cassandra in that scenario.

⸻

🌍 Real-World Application Example: IoT Smart Home Energy Monitoring

Imagine a company like Nest (Google) or Sense that provides smart meters for homes.
Each smart meter sends readings every second: electricity usage, voltage, current, and device states (AC, fridge, washing machine, etc.).

⸻

1️⃣ Problem Definition
•	Thousands of homes → each sends readings every second.
•	That’s millions of data points per day.
•	Users want:
•	To see real-time energy usage (last 5 minutes).
•	To see daily/weekly/monthly trends.
•	To set alerts (if usage spikes above threshold).

A normal SQL DB would choke because:
•	Too many writes/sec.
•	Queries like “average per 10 min for last 7 days” would require scanning billions of rows.

⸻

2️⃣ Schema Design in TSDB (InfluxDB / TimescaleDB style)

Measurement (Table equivalent):

energy_usage

Tags (indexed dimensions):
•	home_id → identifies which home
•	device → fridge, AC, washing_machine

Fields (actual values):
•	power (watts)
•	voltage (V)
•	current (A)

Timestamp:
•	Auto-generated when reading arrives

⸻

3️⃣ Example Data Stored

energy_usage,home_id=H001,device=fridge power=150,voltage=220,current=0.7  2025-10-01T10:00:00Z
energy_usage,home_id=H001,device=AC     power=2000,voltage=220,current=9.1  2025-10-01T10:00:01Z
energy_usage,home_id=H002,device=fridge power=140,voltage=220,current=0.65  2025-10-01T10:00:01Z
energy_usage,home_id=H001,device=TV     power=80, voltage=220,current=0.36  2025-10-01T10:00:02Z

🔑 Notice:
•	Tag values (home_id, device) are indexed → queries filter fast.
•	Field values are numeric/time-series → optimized for compression + aggregation.
•	Time is the primary axis.

⸻

4️⃣ How Queries Work

Query 1: Real-time dashboard (last 5 min for Home H001)

SELECT time, power
FROM energy_usage
WHERE home_id='H001'
AND time > now() - interval '5 minutes';

👉 Fast because TSDB partitions data by time windows.

⸻

Query 2: Daily average usage of AC

SELECT time_bucket('1d', time) AS day, AVG(power)
FROM energy_usage
WHERE home_id='H001' AND device='AC'
GROUP BY day;

👉 Uses time_bucket, so you don’t manually group; DB optimizes using time indexes.

⸻

Query 3: Top 5 homes with highest usage in last hour

SELECT home_id, SUM(power) as total_power
FROM energy_usage
WHERE time > now() - interval '1 hour'
GROUP BY home_id
ORDER BY total_power DESC
LIMIT 5;


⸻

5️⃣ Internal Storage
•	Data is stored in time-chunks (shards), e.g., 1-day partitions.
•	Within each shard:
•	Tags are indexed → quick filtering by home_id or device.
•	Field values are compressed using Gorilla encoding / Delta compression (because time-series values are often similar).

Diagram (simplified):

Shard (2025-10-01):
home_id=H001:
fridge → [ (10:00,150W), (10:01,140W), (10:02,160W) ]
AC     → [ (10:00,2000W), (10:01,1980W), ... ]
home_id=H002:
fridge → [ (10:00,140W), (10:01,145W), ... ]


⸻

6️⃣ Why Not SQL/MongoDB?
•	SQL (Postgres/MySQL):
Can store, but not optimized. Queries like “hourly average for 1 year” require scanning billions of rows. No native time bucketing.
•	MongoDB:
Can store JSON documents with timestamp, but indexing large time ranges is costly. No built-in downsampling or retention policy.
•	Cassandra (Wide-column):
Can store time-stamped events but not optimized for aggregations like moving averages, time buckets. You’d need extra frameworks.
•	TSDB (Influx/Timescale):
Natively built for:
•	Fast inserts (millions/sec)
•	Compression of numeric + time data
•	Native time-window queries
•	Retention policies (auto delete old data)
•	Downsampling (keep raw for 7d, averages for 1y)

⸻

7️⃣ Tech Giants Using TSDB
•	Netflix → Uses Atlas (internal TSDB) for monitoring microservices metrics.
•	Uber → Uses M3DB for real-time metrics of rides & servers.
•	Amazon CloudWatch → Under the hood uses time-series storage for metrics/logs.
•	Tesla → Logs sensor data from cars (speed, battery, GPS) as time-series.
•	Facebook/Meta → Uses Gorilla TSDB for server monitoring at petabyte scale.

⸻

8️⃣ Python Example (with InfluxDB)

from influxdb_client import InfluxDBClient, Point, WriteOptions
from datetime import datetime

# Connect to InfluxDB
client = InfluxDBClient(url="http://localhost:8086", token="my-token", org="my-org")
write_api = client.write_api(write_options=WriteOptions(batch_size=1000))

# Insert a sample data point (energy usage)
point = Point("energy_usage") \
.tag("home_id", "H001") \
.tag("device", "fridge") \
.field("power", 150) \
.field("voltage", 220) \
.field("current", 0.7) \
.time(datetime.utcnow())

write_api.write(bucket="my-bucket", org="my-org", record=point)

# Query last 5 minutes
query = '''
from(bucket:"my-bucket")
|> range(start: -5m)
|> filter(fn: (r) => r._measurement == "energy_usage" and r.home_id == "H001")
'''
tables = client.query_api().query(query, org="my-org")

for table in tables:
for record in table.records:
print(record.get_time(), record.get_field(), record.get_value())


⸻

✅ Revision Notes
•	Type: Time-Series DB (specialized NoSQL).
•	Optimized for: High-write, time-window queries, aggregations.
•	Schema: Measurement (table) + Tags (indexed) + Fields (values) + Time.
•	Storage: Sharded by time, compressed field values.
•	Advantages: Retention, downsampling, native aggregations, low-latency real-time queries.
•	Use Cases: IoT sensors, monitoring, finance (stock ticks), rides (Uber), video streaming (Netflix QoS).
•	Difference from SQL/Mongo/Cassandra: General DBs can store events but not optimized for continuous, high-frequency, time-based queries.

⸻

👉 So the rule of thumb:
•	If your main query is by time, use TSDB.
•	If your main query is by entity ID (user, order, etc.), use SQL/NoSQL.

⸻

Would you like me to next show you a diagram comparing SQL vs TSDB storage for this IoT example, so you can visualize why TSDB is much faster?