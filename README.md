# 2025
Question 1. Version of pip
$ docker run -it --entrypoint bash python:3.12.8

root@27fe1be13a70:/# pip --version

Question 3. Trip Segmentation Count
SELECT
SUM(CASE WHEN trip_distance <= 1 THEN 1 ELSE 0 END) AS up_to_1_mile,
SUM(CASE WHEN trip_distance > 1 AND trip_distance <= 3 THEN 1 ELSE 0 END) AS between_1_and_3_miles,
SUM(CASE WHEN trip_distance > 3 AND trip_distance <= 7 THEN 1 ELSE 0 END) AS between_3_and_7_miles,
SUM(CASE WHEN trip_distance > 7 AND trip_distance <= 10 THEN 1 ELSE 0 END) AS between_7_and_10_miles,
SUM(CASE WHEN trip_distance > 10 THEN 1 ELSE 0 END) AS over_10_miles
FROM green_tripdata
WHERE lpep_pickup_datetime >= '2019-10-01'
AND lpep_dropoff_datetime < '2019-11-01';

Question 4. Longest trip
SELECT
DATE(lpep_pickup_datetime) AS pickup_date,
MAX(trip_distance) AS max_distance
FROM green_tripdata
GROUP BY DATE(lpep_pickup_datetime)
ORDER BY max_distance DESC
LIMIT 1;

Question 5. Biggest pickup zones
SELECT
t2.Zone AS pickup_location,
SUM(t1.total_amount) AS total_amount
FROM green_tripdata t1
JOIN taxi_zone_lookup t2
ON t1.PULocationID = t2.LocationID
WHERE DATE(t1.lpep_pickup_datetime) = '2019-10-18'
GROUP BY t2.Zone
HAVING SUM(t1.total_amount) > 13000
ORDER BY total_amount DESC;

Question 6. Largest tip 
SELECT
t2.Zone AS dropoff_zone,
MAX(t1.tip_amount) AS largest_tip
FROM green_tripdata t1
JOIN taxi_zone_lookup t2
ON t1.DOLocationID = t2.LocationID
WHERE DATE(t1.lpep_pickup_datetime) BETWEEN '2019-10-01' AND '2019-10-31'
AND t1.PULocationID = (
SELECT LocationID
FROM taxi_zone_lookup
WHERE Zone = 'East Harlem North'
)
GROUP BY t2.Zone
ORDER BY largest_tip DESC
LIMIT 1;