# San-Francisco-Trees

credit: https://www.coursera.org/projects/googlecloud-how-to-build-a-bi-dashboard-using-google-data-studio-and-bigqu-cfi1a

## Purpose
To build a BI dashboard using Google Data Studio and BigQuery

## Data source
- In the BigQuery console, add the dataset 'bigquery-public-data' and star it
- In your projects, create a new and seperate dataset 'Reports'
- Write a one-time sql query below to pull the data from 'Street Trees' in 'bigquery-public-data' to our created dataset 'Report'. The query includes the last year information of plant months, the number of trees planted, the species of the planted trees, the care_taker's names, the address and the location where the trees were planted. 
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
- Before running the data, we want to put the results into a new table 'Trees' under the 'Reports' dataset. In the 'More' setting, choose a destination table for query results. Put in the 'Reports' dataset and save. Then run the above sql query. You can see the results pop out.
- To get up-to-date data as new trees will be planted and new information will be added into the original 'Stree Trees' dataset. We can schedule daily refreshing on our sql query to let it run daily and appends new data to our 'Trees' table. Open a new query editor and write the similar sql query but with changing in the where statement as below. Before running the query, click 'Schedule' and create a new scheduled query, give a name to the scheduled  query, and choose daily. Then don't forget to choose the final destinaltion as 'Reports' dataset and 'Trees' table.
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
