# bike-sharing-business-analysis
# 🚴 Bike Sharing Analytics using BigQuery SQL

## 📌 Project Overview

This project analyzes a bike-sharing platform dataset using Google BigQuery SQL to solve real-world business and operational problems.

The objective is to simulate how data analysts and analytics engineers at large-scale technology companies extract actionable insights from transactional datasets using SQL.

The project focuses on:
- Customer behavior analysis
- Ride activity trends
- Station utilization analytics
- Membership segmentation
- Retention analysis
- Operational optimization

The project is structured using a business-oriented analytics approach and includes beginner to intermediate-level SQL case studies.

---

# 🏗️ Tech Stack

- Google BigQuery
- SQL
- CSV Datasets
- Data Analytics
- Business Intelligence

---

# 📂 Dataset Information

The project uses three datasets.

## 1. users.csv

| Column | Description |
|---|---|
| user_id | Unique user identifier |
| username | User name |
| age | User age |
| membership_level | Membership type |
| created_at | Account creation date |

---

## 2. stations.csv

| Column | Description |
|---|---|
| station_id | Unique station ID |
| station_name | Station name |
| capacity | Bike capacity |
| lat | Latitude |
| lon | Longitude |

---

## 3. rides.csv

| Column | Description |
|---|---|
| ride_id | Unique ride ID |
| user_id | Ride user |
| start_station_id | Start station |
| end_station_id | End station |
| start_time | Ride start timestamp |
| end_time | Ride end timestamp |
| distance_km | Distance travelled |

---

# 🏛️ Data Model

- One user can have multiple rides
- One station can appear in multiple rides

### Tables Used
- `users`
- `rides`
- `stations`

---

# 🚀 BigQuery Table Creation

## Create Users Table

```sql
CREATE OR REPLACE TABLE bike_project.users (
    user_id INT64,
    username STRING,
    age INT64,
    membership_level STRING,
    created_at DATE
);
```

---

## Create Stations Table

```sql
CREATE OR REPLACE TABLE bike_project.stations (
    station_id INT64,
    station_name STRING,
    capacity INT64,
    lat FLOAT64,
    lon FLOAT64
);
```

---

## Create Rides Table

```sql
CREATE OR REPLACE TABLE bike_project.rides (
    ride_id INT64,
    user_id INT64,
    start_station_id INT64,
    end_station_id INT64,
    start_time TIMESTAMP,
    end_time TIMESTAMP,
    distance_km FLOAT64
);
```

---

# 📊 SQL Business Problems & Queries

---

# 1️⃣ Total Number of Users

## Business Problem
Find the total number of registered users.

```sql
SELECT COUNT(*) AS total_users
FROM bike_project.users;
```

### Insight
Measures overall platform size.

---

# 2️⃣ Users by Membership Type

## Business Problem
Find the number of users in each membership category.

```sql
SELECT 
    membership_level,
    COUNT(*) AS total_users
FROM bike_project.users
GROUP BY membership_level
ORDER BY total_users DESC;
```

### Insight
Helps identify the most popular subscription tier.

---

# 3️⃣ Average User Age

## Business Problem
Calculate average user age.

```sql
SELECT ROUND(AVG(age),2) AS avg_age
FROM bike_project.users;
```

### Insight
Identifies the core customer demographic.

---

# 4️⃣ Total Number of Rides

## Business Problem
Find total completed rides.

```sql
SELECT COUNT(*) AS total_rides
FROM bike_project.rides;
```

### Insight
Measures platform activity and operational scale.

---

# 5️⃣ Average Ride Distance

## Business Problem
Find average ride distance.

```sql
SELECT ROUND(AVG(distance_km),2) AS avg_distance
FROM bike_project.rides;
```

### Insight
Helps understand user travel behavior.

---

# 6️⃣ Longest Ride Distance

## Business Problem
Find the longest ride recorded.

```sql
SELECT MAX(distance_km) AS longest_ride
FROM bike_project.rides;
```

### Insight
Detects heavy usage behavior.

---

# 7️⃣ Total Bike Stations

## Business Problem
Find total stations available.

```sql
SELECT COUNT(*) AS total_stations
FROM bike_project.stations;
```

### Insight
Measures operational coverage.

---

# 8️⃣ Average Station Capacity

## Business Problem
Calculate average station capacity.

```sql
SELECT ROUND(AVG(capacity),2) AS avg_capacity
FROM bike_project.stations;
```

### Insight
Evaluates infrastructure capability.

---

# 9️⃣ Daily Ride Volume

## Business Problem
Analyze ride volume by day.

```sql
SELECT 
    DATE(start_time) AS ride_date,
    COUNT(*) AS total_rides
FROM bike_project.rides
GROUP BY ride_date
ORDER BY ride_date;
```

### Insight
Identifies demand spikes and usage patterns.

---

# 🔟 Top 5 Most Active Users

## Business Problem
Find users with the highest ride counts.

```sql
SELECT 
    u.user_id,
    u.username,
    COUNT(r.ride_id) AS total_rides
FROM bike_project.users u
JOIN bike_project.rides r
ON u.user_id = r.user_id
GROUP BY u.user_id, u.username
ORDER BY total_rides DESC
LIMIT 5;
```

### Insight
Identifies loyal customers for retention programs.

---

# 1️⃣1️⃣ Average Ride Duration

## Business Problem
Calculate average ride duration.

```sql
SELECT 
    ROUND(
        AVG(
            TIMESTAMP_DIFF(end_time, start_time, MINUTE)
        ),2
    ) AS avg_ride_duration
FROM bike_project.rides;
```

### Insight
Useful for pricing and operational planning.

---

# 1️⃣2️⃣ Top 10 Most Used Start Stations

## Business Problem
Find stations with highest ride starts.

```sql
SELECT 
    s.station_name,
    COUNT(r.ride_id) AS total_rides
FROM bike_project.stations s
JOIN bike_project.rides r
ON s.station_id = r.start_station_id
GROUP BY s.station_name
ORDER BY total_rides DESC
LIMIT 10;
```

### Insight
Highlights high-demand stations.

---

# 1️⃣3️⃣ Peak Ride Hours

## Business Problem
Find busiest ride hours.

```sql
SELECT 
    EXTRACT(HOUR FROM start_time) AS ride_hour,
    COUNT(*) AS total_rides
FROM bike_project.rides
GROUP BY ride_hour
ORDER BY total_rides DESC;
```

### Insight
Reveals commuter traffic patterns.

---

# 1️⃣4️⃣ Membership-wise Average Ride Distance

## Business Problem
Compare average distance across membership types.

```sql
SELECT 
    u.membership_level,
    ROUND(AVG(r.distance_km),2) AS avg_distance
FROM bike_project.users u
JOIN bike_project.rides r
ON u.user_id = r.user_id
GROUP BY u.membership_level
ORDER BY avg_distance DESC;
```

### Insight
Shows engagement differences between plans.

---

# 1️⃣5️⃣ Users Above Average Ride Distance

## Business Problem
Find users riding above platform average distance.

```sql
SELECT 
    user_id,
    ROUND(AVG(distance_km),2) AS avg_distance
FROM bike_project.rides
GROUP BY user_id
HAVING AVG(distance_km) > (
    SELECT AVG(distance_km)
    FROM bike_project.rides
)
ORDER BY avg_distance DESC;
```

### Insight
Identifies high-value users.

---

# 1️⃣6️⃣ Monthly Ride Trends

## Business Problem
Analyze monthly ride growth.

```sql
SELECT 
    FORMAT_TIMESTAMP('%Y-%m', start_time) AS ride_month,
    COUNT(*) AS total_rides
FROM bike_project.rides
GROUP BY ride_month
ORDER BY ride_month;
```

### Insight
Identifies seasonality and growth trends.

---

# 1️⃣7️⃣ Station Utilization Analysis

## Business Problem
Analyze ride activity relative to station capacity.

```sql
SELECT 
    s.station_name,
    s.capacity,
    COUNT(r.ride_id) AS total_rides,
    ROUND(COUNT(r.ride_id)/s.capacity,2) AS utilization_ratio
FROM bike_project.stations s
LEFT JOIN bike_project.rides r
ON s.station_id = r.start_station_id
GROUP BY s.station_name, s.capacity
ORDER BY utilization_ratio DESC;
```

### Insight
Identifies overloaded stations.

---

# 1️⃣8️⃣ User Retention Analysis

## Business Problem
Find users with more than 20 rides.

```sql
SELECT 
    u.user_id,
    u.username,
    COUNT(r.ride_id) AS total_rides
FROM bike_project.users u
JOIN bike_project.rides r
ON u.user_id = r.user_id
GROUP BY u.user_id, u.username
HAVING COUNT(r.ride_id) > 20
ORDER BY total_rides DESC;
```

### Insight
Represents highly retained users.

---

# 1️⃣9️⃣ Average Ride Duration by Membership Type

## Business Problem
Compare ride duration across memberships.

```sql
SELECT 
    u.membership_level,
    ROUND(
        AVG(
            TIMESTAMP_DIFF(
                r.end_time,
                r.start_time,
                MINUTE
            )
        ),2
    ) AS avg_duration_minutes
FROM bike_project.users u
JOIN bike_project.rides r
ON u.user_id = r.user_id
GROUP BY u.membership_level
ORDER BY avg_duration_minutes DESC;
```

### Insight
Helps optimize pricing strategies.

---

# 2️⃣0️⃣ Top 5 Longest Rides

## Business Problem
Identify the longest individual rides.

```sql
SELECT 
    ride_id,
    user_id,
    distance_km
FROM bike_project.rides
ORDER BY distance_km DESC
LIMIT 5;
```

### Insight
Useful for operational monitoring and anomaly detection.

---

# 📈 Key Business Insights

## Customer Analytics
- Highly active users contribute significantly to ride volume.
- Membership type impacts ride behavior and engagement.

## Operational Analytics
- Certain stations consistently experience high traffic.
- Peak hours align strongly with commuting schedules.

## Growth Opportunities
- High-utilization stations may require expansion.
- Loyal users can be targeted with premium offerings.

---

# 🧠 SQL Skills Demonstrated

## Beginner SQL
- SELECT
- WHERE
- ORDER BY
- GROUP BY
- Aggregate Functions

## Intermediate SQL
- JOIN Operations
- LEFT JOIN
- Subqueries
- HAVING Clause
- TIMESTAMP Functions
- Date Formatting
- KPI Calculations

---

# 🚀 Future Improvements

Potential project extensions:
- Power BI Dashboard
- Geospatial Analytics
- Demand Forecasting
- Window Functions
- Churn Prediction
- Machine Learning Integration

---

# 💼 Portfolio Value

This project demonstrates:
- Real-world SQL analytics
- Business problem-solving
- BigQuery expertise
- Data storytelling
- Analytical thinking

Suitable for:
- Data Analyst roles
- Business Analyst roles
- Analytics Engineer portfolios
- SQL Interview preparation

---

# 👨‍💻 Author

Shatabdi Dutta


