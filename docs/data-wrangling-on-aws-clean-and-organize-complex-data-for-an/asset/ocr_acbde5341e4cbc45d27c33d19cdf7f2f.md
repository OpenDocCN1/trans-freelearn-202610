# Challenge

You can expand the query architecture to match queries that had a wider dataset earlier and then apply filters to get a smaller dataset. Expand on the high-level process listed in the following list.

+   1. Identify the older query and split the where clause and from clause in the queries before comparison.
+   
+   2. Parse the new query in a similar format and compare it with the older query. If the new query has more filters than the old query or/and fewer columns than the old query, we can use the data from the old query and apply the filters in the pandas DataFrame and return the results, such as the following:

```
Query 1: select * from hr. employee where city='NY'
Query 2: select * from hr. employee where city='NY' and join_date>'2020-10-01'
```

Here, we can see that Query 2 is a subset of Query 1, and we can use the already executed data and apply further filters on the columns in the pandas DataFrame.

Also, there is tremendous scope for improving the query matching logic to improve the cache hit, and the updates you make can be contributed back to the AWS SDK for pandas repository. Happy coding!!!

Also, It provides many helpful modules, a couple of which are listed here:

+   - Repair table: This runs the MSCK REPAIR command to update the table metadata with the most recent data from the storage layer. This function executes Hive's metastore consistency check – MSCK REPAIR TABLE <tablename>. The command recovers partitions and data associated with partitions and updates the table definition. This is needed to query the data that is available under those partitions. This is useful to execute when new partitions are created from an ETL job that doesn't update the Glue catalog.
+   
+   - Wait query: This iterates through the query status every 250 ms until the query moves to either FAILED, SUCCEEDED, or CANCELLED status (the final status for a query execution). You have to customize your code if you want to verify query execution status, using a shorter or longer polling mechanism.

The aws wrangler package takes care of data type conversion internally. The Parquet files from the resulting folder are read using S3 read functions (which in turn uses pyarrow to read the S3 object). The data type conversion of the Parquet files – arrow – athena are handled internally within the aws wrangler package. The following snippet from the data_types.py module shows the conversion of the pyarrow and athena data types.