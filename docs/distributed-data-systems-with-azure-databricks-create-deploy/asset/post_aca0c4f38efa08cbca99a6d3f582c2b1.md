
## Relationships and columns

Skew join optimization is a computationally intensive operation that must be run only when it is required. That is why the argument of SKEW is column names. When running joins, it is useful to then just run SKEW on the column that we are using to do this.

In the following code example, we apply the skew hint to a single column, which is user_id:

```
SELECT /*+ SKEW('orders', user_id) */ *
FROM orders, users
WHERE user_id = our_condition
```

We can also apply the hint to multiple columns, as follows:

```
SELECT /*+ SKEW('orders', ('user_id', 'o_region_id')) */ *
FROM orders, users
WHERE user_id = our_condition AND o_region_id= c_region_id
```

You can also specify skew values in the hint. Depending on the query and data, the skew values might be known (for example, because they never change) or might be easy to find out. Doing this reduces the overhead of skew join optimization. Otherwise, Delta Lake detects them automatically.

Values can be also specified on the hints, depending on the type of query. The more specific the parameters passed to SKEW, the less overhead there will be for the optimization task.

In the following code example, we run a query on a single column specifying a single skew value:

```
SELECT /*+ SKEW('orders', 'user_id', 0) */ *
FROM orders, users
WHERE user_id = our_condition
```

We can also pass several skew values, as follows:

```
SELECT /*+ SKEW('orders', 'user_id', (0, 1, 2)) */ *
FROM orders, users
WHERE user_id = our_condition
```

And finally, we can also specify several columns and—respectively—several values to those columns, as follows:

```
SELECT /*+ SKEW('orders', ('user_id', o_region_id), ((0, 1001), (1, 1002))) */ *
FROM orders, users
WHERE user_id = our_condition AND o_region_id= c_region_id
```

We can see that we can correct skewness in our data in a selective manner, applying this transformation only in columns that we select that also comply with the stated condition. That way, we can optimize in a more effective manner.

## Summary

In this chapter, we have discussed how we can apply Delta Engine features such as Z-Ordering, Data Skipping, and data caching, among others, to improve the layout structure of our data, leading to an overall improvement in query performance. We can leverage these tools to reorganize files into lesser, much more compact ones, in which data is distributed according to how frequently it is accessed. We can also create Bloom filters on data columns to improve the speed in which we run our queries.

In the next chapter, we will deepen our knowledge of how to ingest streams of data in our Delta tables.

## 6
Introducing
Structured
Streaming

Many organizations have a need to consume large amounts of data continuously in their everyday processes. Therefore, in order to be able to extract insights and use the data, we need to be able to process this information as it arrives, resulting in a need for continuous data ingestion processes. These continuous applications create a need to overcome challenges such as creating a reliable process that ensures the correctness of the data, despite possible failures such as traffic spikes, data not arriving in time, upstream failures, and so on, which are common when working with continuously incoming data or transforming data without consistent file formats that have different structure levels or need to be aggregated before being used.

The most traditional way of dealing with these issues was to work with batches of data executed in periodic tasks, which processed raw streams and data and stored them into more efficient formats to allow queries on the data, a process that might incur high latency.

In this chapter, we will learn about Structured Streaming, which is the Apache Spark **application programming interface (API)** that allows us to make computations on streaming data in the same way you would express a batch computation on static data. Structured Streaming is a fault-tolerant engine that allows us to perform optimized operations for low-latency **Structured Query Language (SQL)**-like queries on the data thanks to the use of Spark SQL, while ensuring data consistency.

We can use Structured Streaming when creating ETL pipelines that will allow us to filter, transform, and clean data as it comes in a raw structure, formatting data types into more efficient storage formats, applying efficient partition strategies based on relevant columns, and so on. Specifically, in this chapter, we will look at the following topics:

- Using the Structured Streaming API
- Using different sources available in Azure Databricks when dealing with continuous streams of data
- Recovering from query failures
- Optimizing streaming queries
- Triggering streaming query executions
- Visualizing data on streaming dataframes
- Example on Structured Streaming

Without further ado, we will start our discussion by talking about Structured Streaming models and how can we leverage their architecture to improve performance when working with streams of data.

## Technical requirements

This chapter will require you to have an Azure Databricks subscription available to work on the examples, as well as a notebook attached to a running cluster.

Let's start by looking into Structured Streaming models in more detail to find out which alternatives are available to work with streams of data in Azure Databricks.

## Structured Streaming model

A Structured Streaming model is based on a simple but powerful premise: any query executed on the data will yield the same results as a batch job at a given time. This model ensures consistency and reliability by processing data as it arrives in the data lake, within the engine, and when working with external systems.

As seen before in the previous chapters, to use Structured Streaming we just need to use Spark dataframes and the API, stating which are the I/O locations.

Structured Streaming models work by treating all new data that arrives as a new row appended to an unbound table, thereby giving us the opportunity to run batch jobs on data as if all the input were being retained, without having to do so. We then can query the streaming data as a static table and output the result to a data sink.

Structured Streaming is able to do this, thanks to a feature called **Incrementalization**, which plans a streaming execution every time that we run a query. This also helps to figure out which states need to remain updated every time new data arrives.

We define when the tables will be updated using specific triggers when using Structured Streaming dataframes. Each time a trigger is activated, Azure Databricks will update the table incrementally by creating new rows for each file that arrives in our data source.

Finally, the last part to be defined are the output modes. These modes allow us to determine how to write incrementally to external systems such as **Simple Storage Service (S3)** or a database in three different ways, outlined as follows:

- **Append mode**: Only new rows are appended. This mode doesn't allow changes to existing rows.
- **Complete mode**: The entire table is rewritten to the external storage every time there is an update.
- **Update mode**: Only rows that have changed since the last trigger are written to the external storage, which allows in-place updates such as SQL tables.

Structured Streaming output tables provide consistent results based on a sequential stream, and failure tolerance is also ensured when working with output sinks. Using the Complete output mode, for example, out-of-order data will be handled by updating the records in a SQL table. We can establish a threshold to avoid overly old data updating a table.

Structured Streaming is a fault-tolerant system that allows queries to be run incrementally on streaming data, as explained in the following examples:

- For example, if we want to read JavaScript Object Notation (JSON) files that arrive continuously at an S3 location, we write the following code:
```
input_df = spark.readStream.json("s3://datalogs")
```
- We can operate on the Spark dataframe and do time-based aggregations to finally write to a SQL database, as follows:
```
input_df.groupBy($"message", window("update_time", "1 hour")).count()
.writeStream.format("jdbc").start(jdbc_conn_string)
```
- If we run this as a batch operation, the only changes will be about how we write and read the data. For example, to read the data just once, run the following code:
```
input_df = spark.read.json("s3://datalogs")
```
- Then, we can do operations using the standard Spark DataFrame API and write to a SQL database, as illustrated in the following code snippet:
```
inputDF.groupBy("message", window("update_time", "1 hour")).count()
.writeStream.format("jdbc").save(jdbc_conn_string)
```

In the next section, we will see in detail how to use the Structured Streaming API in more detail, including how to perform interactive queries and windowed aggregations, and we will discuss mapping and filtering operations.

## Using the Structured Streaming API

Structured Streaming is integrated into the PySpark API and embedded in the Spark DataFrame API. It provides ease of use when working with streaming data and, in most cases, it requires very small changes to migrate from a computation on static data to a streaming computation. It provides features to perform windowed aggregation and for setting the parameters of the execution model.
