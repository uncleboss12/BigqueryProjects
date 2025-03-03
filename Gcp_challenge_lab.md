```sql
CREATE OR REPLACE TABLE taxirides.2015_fare_amount_predictions AS
select 
  pickup_datetime,
  pickup_longitude AS pickuplon,
  pickup_latitude AS pickuplat,
  dropoff_longitude AS dropofflon,
  dropoff_latitude AS dropofflat,
  passenger_count AS passengers,
  ( tolls_amount + fare_amount ) AS fare_amount_116
from `taxirides.historical_taxi_rides_raw`
where trip_distance > 1
     and fare_amount <= 3
     and passenger_count > 1
     AND pickup_latitude > 40
     AND pickup_latitude < 42
     AND dropoff_latitude > 40
     AND dropoff_latitude < 42
     AND RAND() < 999999 / 1031673361
```

```sql
CREATE or REPLACE MODEL
  `taxirides.fare_model_851` OPTIONS (model_type='linear_reg',
    labels=['fare_amount_116']) AS
WITH
  taxitrips AS (
  SELECT
    *,
    ST_Distance(ST_GeogPoint(pickuplon, pickuplat), ST_GeogPoint(dropofflon, dropofflat)) AS euclidean
  FROM
    `taxirides.taxi_training_data_316` )
  SELECT
    *
  FROM
    taxitrips
```

```sql
#standardSQL
SELECT
  SQRT(mean_squared_error) AS rmse
FROM
  ML.EVALUATE(MODEL `taxirides.fare_model_851`,
    (
    WITH
      taxitrips AS (
      SELECT
        *,
        ST_Distance(ST_GeogPoint(pickuplon, pickuplat), ST_GeogPoint(dropofflon, dropofflat)) AS euclidean
      FROM
        `taxirides.taxi_training_data_316`)
      SELECT
        *
      FROM
        taxitrips ))

 ```


```sql
#standardSQL
SELECT
  *
FROM
  ML.PREDICT(MODEL `taxirides.fare_model_851`,
    (
    WITH
      taxitrips AS (
      SELECT
        *,
        ST_Distance(ST_GeogPoint(pickuplon, pickuplat)   , ST_GeogPoint(dropofflon, dropofflat)) AS    euclidean
      FROM
        `taxirides.report_prediction_data` )
    SELECT
      *
    FROM
      taxitrips ))
```



