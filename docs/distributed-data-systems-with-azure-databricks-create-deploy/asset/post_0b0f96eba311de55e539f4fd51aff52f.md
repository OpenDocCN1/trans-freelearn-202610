
As we have discussed in previous chapters, in Azure Databricks, streams of data are represented as Spark dataframes. We can verify that the data frame is a stream of data by checking that the `isStreaming` property of the data frame is set as `true`. In order to operate with Structured Streaming, we can summarize the steps as read, process, and write, as exemplified here:

1. We can read streams of data that are being dumped in, for example, an S3 bucket. The following example code shows how we can use the `readStream` method, specifying that we are reading a **comma-separated values (CSV)** file stored in the S3 **Uniform Resource Interface (URI)** passed:
2. Now, you can use the usual data frame operations to transform the data as if it were static. For example, if we want to count action types of each hour, we can do this by grouping the data by `message` and 1-hour windows of time, as illustrated in the following code snippet:
3. Finally, we write this table to a sink and start the streaming computation by creating a connection to a SQL database using a **Java Database Connectivity (JDBC)** connection string, as illustrated in the following code snippet:

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

## Mapping, filtering, and running aggregations

Structured Streaming allows the use of mapping, filtering, selection, and other methods to transform data. In the previous example, we used one of these features: time-based aggregations on data through the Spark API. As we have seen in our previous example, aggregations such as windowing can be expressed as a simple group by operations on a data frame. The use of such operations will be exemplified in the next section, but it is good to remember that Structured Streaming allows us to perform mapping, filtering, and other data-wrangling methods in the same way as with a common PySpark dataframe.

In the next section, we will see some examples of how these operations are performed using Structured Streaming.

## Windowed aggregations on event time

When working with data that has a temporal dimension, compute operations often need to be windowed to certain periods of time, which in cases such as sliding windows overlap with each other. For example, we might need to perform operations over a window of 1 hour that slides forward every 5 minutes.

Windows are specified using the window function in a PySpark dataframe. For example, change to a sliding window approach (shown in the example previously given in the Structured Streaming model section) by doing the following:

```
input_df.groupBy("message", window("update_time", "1 hour", "5 minutes")).count()
```

In the way our previous example was structured, the resulting information was formatted as (hour, message, count); now, it will have the form (window, message, count). Out-of-order data arriving late will be processed and the results will be updated accordingly, and if we are using an external data sink in Complete mode, the data will be updated there as well.

## Merging streaming and static data

As we have mentioned before, working with Structured Streaming means working with Spark data frames, which is straightforward and allows us to use streaming and static data in combination. For example, if we have a table named users and we want to append it to a streaming data frame we can do the following, where we bring in a table called users and append it to our input_df dataframe:

```
users_df = spark.table("users")
input_df.join(users_df, "user_id").groupBy("user_name", hour("time")).count()
```

We could also create this static data frame using a query and run it in the same batch and streaming operations.

## Interactive queries

In Structured Streaming, we can make use of the results from computations directly to interactive queries using the Spark JDBC server. There is an option to use a small memory sink designed for small amounts of data, whereby we can write results to a SQL table and then query the data in it. For example, we can create a table named message_counts that can be used as a small data sink that we can later query.

We do this by creating an in-memory table, as follows:

```
message_counts.writeStream.format("memory")
.queryName("user_counts")
.outputMode("complete")
.start()
```

And then, we use a SQL statement to query it, like this:

```
%sql
select sum(count) from message_counts where message='warning'
```

In the next section, we will discuss all the different sources from where we can read stream data.

## Using different sources with continuous streams

Streams of data can come from a variety of sources. Structured Streaming provides support from extracting data from sources such as Delta tables, publish/subscribe (pub/sub) systems such as Azure Event Hubs, and more. We will review some of these sources in the next sections to learn how we can connect these streams of data into our jobs running in Azure Databricks.

## Using a Delta table as a stream source

As mentioned in the previous chapter, you can use Structured Streaming with Delta Lake using the readStream and writeStream Spark methods, with a particular focus on overcoming issues related to handling and processing small files, managing batch jobs, and detecting new files efficiently.

When a Delta table is used as a data stream source, all the queries done on that table will process the information on that table as well as any data that has arrived since the stream started.

In the next example, we will load both the path and the tables into a dataframe, as follows:

```
df = spark.readStream.format("delta").load("/mnt/delta/data_events")
```

Or, we can do the same by referencing the Delta table, as follows:

```
df = spark.readStream.format("delta").table('data_events')
```

One of the features of Structured Streaming in Delta Lake is that we can control the maximum number of new files to be considered every time a trigger is activated. We can control this by setting the maxFilesPerTrigger option to the desired number of files to be considered. Another option to set is the rate limit on how much data gets to be processed in every micro-batch. This is controlled by using the maxBytesPerTrigger option, which controls the number of bytes processed each time the trigger is activated. The number of bytes to be processed will approximately match the number specified in this option, but it can slightly surpass this limit.

One thing to mention is that Structured Streaming will fail if we attempt to append a column or modify the schema of a Delta table that is being used as the source. If changes need to be introduced, we can approach this in two different ways, outlined as follows:

- After the changes are made, we can delete the output and checkpoint and restart the stream from the start.
- We can use the `ignoreDeletes` option to ignore operations that causes the deletion of data, or `ignoreChanges` so that the stream doesn't get interrupted by deletions or updates in the source table.

For example, suppose that we have a table named `user_messages` from where we stream out data that contains date, `user_email`, and message columns partitioned by date. If we need to delete data at partition boundaries, which means deleting using a `WHERE` clause on the partition column, the files are already segmented by that column. Therefore, the delete operation will just remove those files from the metadata.

So, if you just want to delete data using a `WHERE` clause on a partition column, you can use the `ignoreDeletes` option to avoid having your stream disrupted, as illustrated in the following code snippet:

```
data_events.readStream
  .format("delta").option("ignoreDeletes", "true")
  .load("/mnt/delta/user_messages")
```

However, if you have to delete data based on the `user_email` column, then you will need to use the following code:

```
data_events.readStream
  .format("delta").option("ignoreChanges", "true")
  .load("/mnt/delta/user_messages")
```

If you update a `user_email` variable with an UPDATE statement, the file that contains that record will be rewritten. If the `ignoreChanges` option is set to true, this file will be processed again, inserting the new record and other already processed records into the same file, producing duplicates downstream. Thus, it is recommended to implement some logic to handle these incoming duplicate records.

In order to specify a starting point of the Delta Lake streaming source without processing the entire table, we can use the following options:

- `startingVersion`: The Delta Lake version to start from
- `startingTimestamp`: The timestamp to start from
