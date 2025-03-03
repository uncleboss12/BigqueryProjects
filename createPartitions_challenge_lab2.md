
## Task 1
```sql
CREATE OR REPLACE TABLE covid.oxford_policy_tracker
PARTITION BY date
OPTIONS(
partition_expiration_days=1080,
description="oxford_policy_tracker table in the COVID 19 Government Response public dataset with  an expiry time set to 90 days."
) AS
SELECT
   *
FROM
    `bigquery-public-data.covid19_govt_response.oxford_policy_tracker`
WHERE
   alpha_3_code NOT IN ('GBR', 'BRA', 'CAN','USA')
```



## Task 2
```sql
ALTER TABLE `covid_data.global_mobility_tracker_data`
ADD COLUMN population INT64,
ADD COLUMN country_area FLOAT64,
ADD COLUMN mobility STRUCT<
   avg_retail      FLOAT64,
   avg_grocery     FLOAT64,
   avg_parks       FLOAT64,
   avg_transit     FLOAT64,
   avg_workplace   FLOAT64,
   avg_residential FLOAT64 >
```


## Task 3
```sql
UPDATE
   covid_data.consolidate_covid_tracker_data t0
SET
   t0.population = t2.pop_data_2019
FROM
(SELECT DISTINCT country_territory_code, pop_data_2019 FROM `bigquery-public-data.covid19_ecdc.covid_19_geographic_distribution_worldwide`) AS t2
WHERE t0.alpha_3_code = t2.country_territory_code;
```


## Task 4
```sql
UPDATE
   covid_data.consolidate_covid_tracker_data t0
SET
   t0.country_area = t1.country_area
FROM
   `bigquery-public-data.census_bureau_international.country_names_area` t1
WHERE
   t0.country_name = t1.country_name
```
