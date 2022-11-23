# San-Francisco-Trees

reference: https://www.coursera.org/projects/googlecloud-how-to-build-a-bi-dashboard-using-google-data-studio-and-bigqu-cfi1a

## Purpose
To build a BI dashboard using Google Data Studio and BigQuery

## Data source
- In BigQuery, add the dataset 'bigquery-public-data' and star it
- In my projects, create a new and seperate dataset 'Reports'
- Write a one-time sql query below to pull the data from 'Street Trees' in 'bigquery-public-data' to our created dataset 'Report'. 
```
SELECT
 TIMESTAMP_TRUNC(plant_date, MONTH) as plant_month,
  COUNT(tree_id) AS total_trees,
  species,
  care_taker,
  address,
  site_info
FROM
  `bigquery-public-data.san_francisco_trees.street_trees`
WHERE
  address IS NOT NULL
  AND plant_date >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 365 DAY)
  AND plant_date < TIMESTAMP_TRUNC(CURRENT_TIMESTAMP(), DAY)
GROUP BY
  plant_month,
  species,
  care_taker,
  address,
  site_info
```
