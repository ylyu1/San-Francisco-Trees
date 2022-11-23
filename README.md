# San-Francisco-Trees

credit: https://www.coursera.org/projects/googlecloud-how-to-build-a-bi-dashboard-using-google-data-studio-and-bigqu-cfi1a
        <br/> https://cloud.google.com/blog/products/gcp/how-to-build-a-bi-dashboard-using-google-data-studio-and-bigquery

## Purpose
To build a BI dashboard using Google Data Studio and BigQuery

## Data source
- In the BigQuery console, add the dataset 'bigquery-public-data' and star it
- In your projects, create a new and seperate dataset named as 'Reports'. This dataset will only includes our interested information extracted from the public data, which reduce the amount of the data and costs.
- Write a one-time sql query below to pull the data from 'Street Trees' in 'bigquery-public-data' to our created dataset 'Report'. The query includes the last year information of plant months, the number of trees planted, the species of the planted trees, the care_taker's names, the address and the location where the trees were planted. Before running the data, we want to put the results into a new table named as 'Trees' under the 'Reports' dataset. In the 'More' setting, choose a destination table for query results. Put in the 'Reports' dataset and save. Then run the below sql query. You can see the results pop out.
```
SELECT
  TIMESTAMP_TRUNC(plant_date, MONTH) as plant_month,
  COUNT(tree_id) AS total_trees,
  species,
  care_taker,
  address,
  site_info
FROM
  'bigquery-public-data.san_francisco_trees.street_trees'
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

- To get up-to-date data as new trees will be planted and new information will be added into the original 'Stree Trees' dataset. We can schedule daily refreshing on our sql query to let it run daily and appends new data to our 'Trees' table if there is any. Open a new query editor and write the similar sql query but with changing in the where statement as below. Before running the query, click 'Schedule' and create a new scheduled query. Give a name to the scheduled query and choose 'daily'. Don't forget to choose the final destinaltion as the 'Trees' table in the 'Reports' dataset.
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
*WHERE
  address IS NOT NULL
  AND plant_date >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 1 DAY)
  AND plant_date < TIMESTAMP_TRUNC(CURRENT_TIMESTAMP(), DAY)**
GROUP BY
  plant_month,
  species,
  care_taker,
  address,
  site_info
  ```
  ## Data visualizations
  - In Data Studio, create a new report and choose the dataset 'Reports' and then the table 'Trees'. Then create the dashboard. 
