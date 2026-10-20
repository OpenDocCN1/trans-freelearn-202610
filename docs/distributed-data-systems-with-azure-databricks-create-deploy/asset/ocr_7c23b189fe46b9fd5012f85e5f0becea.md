As we have discussed in previous chapters, in Azure Databricks, streams of data are represented as Spark dataframes. We can verify that the data frame is a stream of data by checking that the `isStreaming` property of the data frame is set as `true`. In order to operate with Structured Streaming, we can summarize the steps as read, process, and write, as exemplified here:

+   1. We can read streams of data that are being dumped in, for example, an S3 bucket. The following example code shows how we can use the `readStream` method, specifying that we are reading a **comma-separated values (CSV)** file stored in the S3 **Uniform Resource Interface (URI)** passed:
+   2. Now, you can use the usual data frame operations to transform the data as if it were static. For example, if we want to count action types of each hour, we can do this by grouping the data by `message` and 1-hour windows of time, as illustrated in the following code snippet:
+   3. Finally, we write this table to a sink and start the streaming computation by creating a connection to a SQL database using a **Java Database Connectivity (JDBC)** connection string, as illustrated in the following code snippet:

```
input_df=spark.readStream.csv("s3://data_stream_uri")
```

The resulting PySpark dataframe, `input_df`, will be the input table, continuously extended with new rows as new files arrive in the input directory. We will suppose that this table has two columns—`update_time` and `message`.

```
counts_df = input_df.groupBy($"message", window($"update_time", "1 hour")).count()
```

The new dataframe, `counts_df`, is our result table, which has message, window, and counts columns, and will be continuously updated when the query is started. Note that this operation will yield the resulting hourly counts even if `input_df` is a static table. This is a feature that provides a safe environment to test a business logic on static datasets and seamlessly migrate into streaming data without changing the logic.

```
final_query = counts_df.writeStream.format("jdbc").start("jdbc://...")
```

The returned query is a `StreamingQuery` query, which is a handle to the active streaming execution running in the background. It can later be used to manage and monitor the execution of our streaming query. Beyond these basics, there are many more operations that can be done in Structured Streaming.