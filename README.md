# SQL-Analysis-for-Zuber
SQL Query for Analyzing Trip Duration Based on Weather Conditions
This README file provides an overview of an SQL query designed to analyze the duration of trips between two specific locations, filtered by weather conditions on Saturdays. The query joins trip data with weather records to categorize weather conditions and then selects trips occurring on Saturdays with defined pickup and dropoff locations.

Query Overview
The purpose of this SQL query is to:

Join Trip Data with Weather Conditions: Combine trip records with corresponding weather conditions.
Categorize Weather Conditions: Classify the weather as "Good" or "Bad" based on descriptions.
Filter Specific Trips: Select trips between specific pickup and dropoff locations.
Focus on Specific Day of the Week: Restrict the data to trips occurring on Saturdays.
SQL Query
sql
Copy code
SELECT
    start_ts,
    T.weather_conditions,
    duration_seconds
FROM 
    trips
INNER JOIN (
    SELECT
        ts,
        CASE
            WHEN description LIKE '%rain%' OR description LIKE '%storm%' THEN 'Bad'
            ELSE 'Good'
        END AS weather_conditions
    FROM 
        weather_records          
) T ON T.ts = trips.start_ts
WHERE 
    pickup_location_id = 50 AND dropoff_location_id = 63 AND EXTRACT (DOW from trips.start_ts) = 6
ORDER BY trip_id;
Detailed Explanation
Step-by-Step Breakdown
Weather Condition Categorization:

Subquery T creates a temporary table with timestamps (ts) and categorizes weather conditions as "Bad" if the description contains "rain" or "storm", otherwise as "Good".
sql
Copy code
SELECT
    ts,
    CASE
        WHEN description LIKE '%rain%' OR description LIKE '%storm%' THEN 'Bad'
        ELSE 'Good'
    END AS weather_conditions
FROM 
    weather_records
Joining Weather Data with Trip Data:

The main query joins the trips table with the subquery T on matching timestamps.
sql
Copy code
INNER JOIN (
    -- Subquery T
) T ON T.ts = trips.start_ts
Filtering Criteria:

Filter trips to include only those with:
pickup_location_id = 50
dropoff_location_id = 63
Occurring on Saturdays (EXTRACT(DOW FROM trips.start_ts) = 6)
sql
Copy code
WHERE 
    pickup_location_id = 50 AND dropoff_location_id = 63 AND EXTRACT (DOW from trips.start_ts) = 6
Sorting:

Order the results by trip_id.
sql
Copy code
ORDER BY trip_id
Selected Columns
start_ts: The start timestamp of the trip.
weather_conditions: Categorized weather conditions at the trip's start time.
duration_seconds: Duration of the trip in seconds.
Usage
To use this query:

Ensure you have access to the trips and weather_records tables.
Run the query in your SQL environment.
Analyze the results to understand how weather conditions affect trip durations between the specified locations on Saturdays.
Requirements
Database Tables:
trips table with columns: start_ts, pickup_location_id, dropoff_location_id, duration_seconds, trip_id.
weather_records table with columns: ts, description.
Conclusion
This query helps in analyzing the impact of weather conditions on trip durations for trips between specified locations on Saturdays. It provides insights into how adverse weather may affect travel time, which can be useful for optimizing routes, scheduling, and improving customer experience.

For any questions or further information, please feel free to contact me.
