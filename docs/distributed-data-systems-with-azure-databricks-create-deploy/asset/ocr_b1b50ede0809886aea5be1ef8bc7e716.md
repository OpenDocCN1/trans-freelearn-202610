2. Now, we can run queries while knowing that the layout of the data has been optimized, according to the filters that we will use to run queries. We also know that the number of files has been optimized to minimize the read of files and improve data skipping techniques:

```
display(covid_delta.groupBy("date").
  agg(count("*").alias("TotalTransitedPlaces")).
  orderBy("TotalTransitedPlaces", ascending=False).limit(20))
```

![](bbox=[0.195, 0.235, 0.58, 0.43])

Figure 4.10 – Count aggregation per day

3. Another way to create the table from the file is to use SQL commands. We can run the following commands in an Azure Databricks notebook cell in order to drop any existing table and create the table. We can do this by selecting all the columns and partitions by province:

```
%sql
DROP TABLE IF EXISTS covid_delta;
CREATE TABLE covid_delta
USING delta
PARTITIONED BY (province)
SELECT *
FROM delta.`/delta/covid_delta/`;
```