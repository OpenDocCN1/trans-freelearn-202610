Here, table_version is a value that can be obtained by using DESCRIBE HISTORY on the table that we wish to query.

+   If we would like to use this feature in a Python block of code in an Azure Databricks notebook, we can set the DataFrameReader option to one specific point in time or version of a table. To do this, we need to pass the timestampAsOf option:

```
df = spark.read.format("delta").option("timestampAsOf", timestamp_string).load("/mnt/delta/data")
```

Here, '/mnt/delta/data' is the location of the table.

The same applies to accessing a specific version. We can do this by passing versionAsOf to the DataFrameReader option:

```
df = spark.read.format("delta").option("versionAsOf", version).load("/mnt/delta/data")
```

It's easy to use time travel to select data from an earlier version of a table. Users can view the history of a table and see what the data looked like at that point by using either a version history number (as shown in the following code, when selecting the VERSION AS OF 0 table data), or by timestamp:

+   We can learn how to obtain the available versions and see the history of a table by reading the HISTORY property of the data using DESCRIBE:

```
DESCRIBE HISTORY data
```

+   If we would like to access the snapshot of the table to see what the data looked like at a certain point in time, we can use TIMESTAMP combined with AS OF:

```
SELECT * FROM loan_by_state_delta TIMESTAMP AS OF '2020-07-14 16:30:00'
```

Delta Lake guarantees that while querying the previous state of a table, all the transformations can be tracked, reproduced, and remain in a reversible state. This is especially useful while working with data that's bound by GDPR or CCPA regulations, in which it is required to have proof of deletion. We can also use this method to visualize every DML operation, such as updates and merges that have been made on this table.