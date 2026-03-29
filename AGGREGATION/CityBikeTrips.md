# Use CONCAT on the Bike Sharing Dataset

The CONCAT function can combine data from separate columns to provide new insights.

## Query

```sql
SELECT
  usertype,
  CONCAT(start_station_name, " to ", end_station_name) AS route, 
  COUNT(*) AS num_trips,
  ROUND(AVG(CAST(tripduration AS INT64) / 60), 2) AS duration 
FROM 
  `bigquery-public-data.new_york.citibike_trips`
GROUP BY
  start_station_name, end_station_name, usertype 
ORDER BY 
  num_trips DESC 
LIMIT 10;
```

## Instructions

In the BigQuery editor, enter `SELECT` and press **Enter** (Windows) or **Return** (Mac).

- Enter `usertype,` on line 2.
- On line 3, enter `CONCAT(start_station_name, " to ", end_station_name)` to combine the names of the beginning and ending stations for each trip in a new column. This will create one column of routes.
- Enter `AS route,` at the end of line 3 to name the column `route`.
- On line 4, enter `COUNT(*) AS num_trips,` to count the number of trips. The asterisk tells SQL to count the number of rows you're selecting. Each row represents a trip, so you can count all of the rows you've selected to count the number of trips.
- Next, calculate the average trip duration for each route. On line 5, enter:

```sql
ROUND(AVG(CAST(tripduration AS INT64) / 60), 2) AS duration
```

## How Line 5 Works

This line of code accomplishes several tasks:

- **CAST**: Casts `tripduration` as an integer and divides by 60 to convert from seconds to minutes.
- **AVG**: Finds the average duration of each route.
- **ROUND**: Rounds the output to 2 decimal places.
- **AS**: Gives the output the alias `duration`.
