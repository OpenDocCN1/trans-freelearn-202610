Structured Streaming is a fault-tolerant system that allows queries to be run incrementally on streaming data, as explained in the following examples:

+   - For example, if we want to read JavaScript Object Notation (JSON) files that arrive continuously at an S3 location, we write the following code:
+   ```
+   input_df = spark.readStream.json("s3://datalogs")
+   ```
+   - We can operate on the Spark dataframe and do time-based aggregations to finally write to a SQL database, as follows:
+   ```
+   input_df.groupBy($"message", window("update_time", "1 hour")).count()
+   .writeStream.format("jdbc").start(jdbc_conn_string)
+   ```
+   - If we run this as a batch operation, the only changes will be about how we write and read the data. For example, to read the data just once, run the following code:
+   ```
+   input_df = spark.read.json("s3://datalogs")
+   ```
+   - Then, we can do operations using the standard Spark DataFrame API and write to a SQL database, as illustrated in the following code snippet:
+   ```
+   inputDF.groupBy("message", window("update_time", "1 hour")).count()
+   .writeStream.format("jdbc").save(jdbc_conn_string)
+   ```

In the next section, we will see in detail how to use the Structured Streaming API in more detail, including how to perform interactive queries and windowed aggregations, and we will discuss mapping and filtering operations.

# Using the Structured Streaming API

Structured Streaming is integrated into the PySpark API and embedded in the Spark DataFrame API. It provides ease of use when working with streaming data and, in most cases, it requires very small changes to migrate from a computation on static data to a streaming computation. It provides features to perform windowed aggregation and for setting the parameters of the execution model.