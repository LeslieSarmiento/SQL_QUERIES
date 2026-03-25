# 🚕 Ride Sharing Demand Peaks — NYC Taxi Analysis

## 📌 Project Overview
Urban transportation systems rely heavily on understanding when and where demand occurs. This project analyzes NYC Yellow Taxi trip data to uncover ride demand patterns, peak usage periods, and high-traffic zones.

The goal is to simulate how a data analyst would support a ride-sharing or transportation company in optimizing driver allocation, pricing strategies, and operational efficiency.

---

## 🎯 Business Problem
Ride-sharing platforms and taxi services need to:
- Anticipate peak demand periods
- Allocate drivers efficiently across locations
- Maximize revenue during high-demand windows
- Understand seasonal and behavioral ride patterns

This analysis answers:
- When is demand highest?
- Where are the busiest pickup zones?
- Which time periods generate the most revenue potential?

---

## 📊 Dataset
**Source:** BigQuery Public Dataset  
`bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2019`

**Data Includes:**
- Trip timestamps
- Pickup and drop-off locations
- Fare amounts
- Trip distances
- Passenger counts

---

## 1. Data Exploration

```sql
SELECT 
  COUNT(*) AS total_trips,
  MIN(trip_distance) AS min_distance,
  MAX(trip_distance) AS max_distance,
  AVG(trip_distance) AS avg_distance,
  COUNT(DISTINCT pickup_location_id) AS unique_pickup_locations
FROM `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2019`;
```

### 2. Peak Demand by Hour & Day

```sql
SELECT 
  EXTRACT(DAYOFWEEK FROM pickup_datetime) AS day_of_week,
  EXTRACT(HOUR FROM pickup_datetime) AS hour_of_day,
  COUNT(*) AS total_rides
FROM `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2019`
GROUP BY day_of_week, hour_of_day
ORDER BY day_of_week, hour_of_day;
```

- Morning and evening commute hours show consistent spikes
- Weekend evenings display elevated demand due to nightlife activity
- Demand patterns clearly differ between weekdays and weekends
- Enables dynamic driver allocation based on predictable demand cycles
- Supports surge pricing strategies during peak hours


### 3. Pickup Location Hotspots

```sql
SELECT
  pickup_location_id,
  COUNT(*) AS total_pickups,
  AVG(fare_amount) AS avg_fare,
  AVG(trip_distance) AS avg_distance
FROM `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2019`
GROUP BY pickup_location_id
ORDER BY total_pickups DESC
LIMIT 20;
```

- A small number of locations account for a large percentage of rides
- High-demand zones often correlate with business districts and transit hubs
- Some locations generate higher fares despite similar ride volumes
- Prioritize driver availability in high-demand zones
- Optimize pickup logistics and reduce rider wait times


### 4. Monthly Demand Trends

```sql
SELECT 
  EXTRACT(MONTH FROM pickup_datetime) AS month,
  COUNT(*) AS total_rides
FROM `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2019`
GROUP BY month
ORDER BY month;
```

- Demand fluctuates across the year, reflecting seasonal trends
- Potential spikes during holidays, tourism seasons, or major events
- Supports long-term forecasting and resource planning
- Helps anticipate seasonal staffing and pricing adjustments

### 5. Peak Demand Index (Derived Metric)

```sql
WITH hourly_stats AS (
  SELECT 
    EXTRACT(HOUR FROM pickup_datetime) AS hour,
    COUNT(*) AS rides,
    AVG(fare_amount) AS avg_fare
  FROM `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2019`
  GROUP BY hour
)
SELECT 
  hour,
  rides,
  avg_fare,
  rides * avg_fare AS peak_demand_index
FROM hourly_stats
ORDER BY peak_demand_index DESC;
```

- Identifies not just busy hours, but the most profitable ones
- Some hours generate fewer rides but higher revenue per trip
- Optimizes pricing strategies
- Highlights high-value operational windows

  ##  Visualization Strategy

To effectively communicate findings, the following visuals can be created:

- Heatmap → Ride demand by hour and day of week  
- Bar chart → Top pickup locations by volume  
- Line chart → Monthly demand trends  
- Geographic map → Ride density across NYC  

---

##  Key Takeaways

- Ride demand follows strong and predictable time-based patterns  
- A limited number of locations drive a large portion of ride activity  
- Seasonal trends impact overall ride volume  
- Combining ride count and fare reveals deeper revenue insights  

---

##  Skills Demonstrated

- SQL aggregation (`COUNT`, `AVG`, `SUM`)  
- Grouping and segmentation using `GROUP BY`  
- Time-based analysis using `EXTRACT`  
- Derived metrics and analytical thinking  
- Translating raw data into business insights  

---

##  Final Business Recommendations

- Increase driver availability during peak commute and weekend evening hours  
- Focus operational resources on high-demand pickup zones  
- Implement dynamic pricing during high-value demand periods  
- Use seasonal trends to forecast demand and adjust strategy accordingly  
