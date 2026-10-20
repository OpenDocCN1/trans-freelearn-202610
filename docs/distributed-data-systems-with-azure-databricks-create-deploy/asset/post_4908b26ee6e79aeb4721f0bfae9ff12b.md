
As mentioned before, when working with structured streaming, the stream can restart in the case of a failure if checkpoints are enabled as shown:

+   To do this, we can set the delta.checkpoint.writeStatsAsStruct option to true in our streaming table, as follows:

```
ALTER TABLE our_table SET TBLPROPERTIES
('delta.checkpoint.writeStatsAsStruct' = 'true')
```

We can run this in a SQL cell in a notebook, which should return OK if the operation was successful, as illustrated in the following example:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_158_0.png)

Figure 5.6 – Setting checkpoint settings on a Delta table

+   Or, when working with Delta tables, we can use the path of the table with a delta prefix, as illustrated in the following code snippet:

```
ALTER TABLE delta.`path_to_our_table` SET TBLPROPERTIES
('delta.checkpoint.writeStatsAsStruct' = 'true')
```

+   We can also use the Apache Spark checkpointLocation option when working with a streaming dataframe by running the following Python command, which writes a streaming dataframe named data in Parquet format and sets the checkpoint location path. Each query must have a unique checkpoint:

```
data.writeStream.format("parquet")
    .option("path", output_path)
    .option("checkpointLocation", checkpoint_path)
    .start()
```

Having checkpoints in our Structured Streaming allows us to have a backup in case of possible failures. If a stream gets restarted, it will automatically continue from where it was left.

## Automatically optimizing files with Delta Engine

Auto Optimize is a feature of Delta Engine to further optimize Delta tables. It automatically compacts small files during **data manipulation language (DML)** actions in big tables, consolidating files and keeping metadata clean. We have two options to apply Auto Optimize: **Optimized Writes** and **Auto Compaction**. Optimized Writes tries to arrange table partitions evenly into 128 **megabytes (MB)**-sized files representing each partition to improve writes. Auto Compaction is an `OPTIMIZE` command with the size of compaction set to 128 MB, which is run after each write operation.

Auto Optimize keeps frequently modified tables optimized, which is very useful when dealing with—for example—low-latency streams of data or frequent table merges.

We need to enable Auto Optimize by enabling the options in `our_delta_table.autoOptimize.optimizeWrite` and `our_delta_table.autoOptimize.autoCompact` on specific tables:

+   - To enable this option on existing tables, we can use the `TBLPROPERTIES` SQL command to set those properties to `true`, as in the following example:

```
ALTER TABLE our_table SET TBLPROPERTIES (delta.
autoOptimize.optimizeWrite = true, delta.autoOptimize.
autoCompact = true)
```

We can run this command in a SQL-specified cell in the notebook, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_159_0.png)

Figure 5.7 – Enabling Auto Optimize on a Delta table

+   - We can also define these options as `true` by default on all new Delta tables, using the following SQL configuration:

```
set spark.databricks.delta.properties.defaults.
autoOptimize.optimizeWrite = true.
```

```
set spark.databricks.delta.properties.defaults.
autoOptimize.autoCompact = true.
```

+   - If we would like to apply this only to our working session, we can use the following code to set the following Spark configurations:

```
spark.databricks.delta.optimizeWrite.enabled
```

```
spark.databricks.delta.autoCompact.enabled
```

One reason to have to enable these options manually is that if we have many concurrent DML operations on the same table from different users, this may cause conflicts in the transaction log, which can be dangerous because this will not cause any failure or retry.

Another thing to keep in mind is that on big tables, having Auto Optimize enabled on a table can be used in combination with OPTIMIZE and does not replace the running of ZORDER.

Let's see in more detail how to use Optimized Writes and Auto Compaction, as follows:

+   - **Optimized Writes**: When Optimized Writes is enabled on a table, Delta Engine will try to reorder the table into approximately 128 MB files for each table partition, which can vary according to the data schema, to balance the number of files being written in parallel. This process reorders the files based on the partition structure of the table. This option is useful when having data streams with a latency of minutes.

+   - **Auto Compaction**: As mentioned before, Auto Compaction is run after each write, so if it is enabled on a table it is not necessary to call OPTIMIZE after each write operation. It is a good idea to enable this option if we do not have a periodically scheduled OPTIMIZE job running on the working table.

Most of the time when optimizing data processing in Azure Databricks, it is easy to just focus on the transformations. But we should always remember that Databricks uses distributed computing, having the need to distribute data across workers. Making sure that data is neatly organized and its storage has been optimized will lead to better performance metrics, not only for storage but also for processing of the data.

## Using caching to improve performance

Caching is an operation in which we keep data stored closer to where it will be processed, to improve performance. It can be applied in two different ways in Azure Databricks: Delta caching and Apache Spark caching. Depending on the situation we are dealing with, we can leverage the specific characteristics of each option to improve the reading speed of our tables.

The good thing about this is that there is no need to make a choice on which option to use because they can be used simultaneously.

Let's dive into the characteristics of both options and look at how they are applied to improve performance.

### Delta and Apache Spark caching

Delta and Apache Spark have different characteristics, which might be summarized as follows:

+   - **Delta caching**: The Delta cache automatically makes copies of remote Parquet files into the local node storage to accelerate its processing. The remote locations that it can access are **Databricks File System (DBFS)**, **Hadoop Distributed File System (HDFS)**, Azure Blob storage, Azure Data Lake Storage Gen1, and Azure Data Lake Storage Gen2. It can operate faster than the Spark cache thanks to optimized decompression and an output format consistent with processing requirements. Data can be preloaded using a `CACHE` statement. Data is then stored on the local disk and can be read quickly thanks to a **solid-state drive (SSD)**.
+   - **Spark caching**: The Spark cache can store results from subqueries and handles several file formats besides Parquet. Queries and tables to be cached must be manually specified. Data is stored in the memory, which can hurt performance.

We can enable Delta caching by selecting one of the **Delta Cache Accelerated** workers from the cluster configuration, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_162_0.png)

Figure 5.8 – Selecting a Delta Cache accelerated storage

Then, we continue as follows:

+   - To enable the Spark cache, we need to run the cache option on our dataframe—as shown in the following Python command—where we cache a dataframe called data:

```
data.cache()
```

+   - The cache will call the persist method, passing MEMORY_DISK as a parameter, meaning that this is the same as the following Python command:

```
data.persist(StorageLevel.MEMORY_AND_DISK)
```

We have several parameters to pass as storage levels, which are outlined as follows:

+   - DISK_ONLY: This will persist only serialized data on disk.
+   - MEMORY_ONLY: This will persist deserialized data in memory only, achieving a very good computational performance but having as a limitation the amount of memory available for storage.
+   - MEMORY_AND_DISK: This will persist data in memory and if not enough memory is available, it will evict data to be stored on disk. This is the default mode in Spark, where it will store as much data as possible in the floating memory, with the rest evicted to the disk. The computation time will be faster than just with a disk but slower than with memory only.
+   - OFF_HEAP: Data is persisted in off-heap memory, which is memory that is not supervised by the Java virtual machine (JVM) garbage collector. The data will be read faster but it should be used carefully because it's the user who must specify the allocated memory.

Having fine-grained control over where the cached data is stored in the worker allows us to also have fine-grained control over the performance of the algorithms making use of the data. Data that is stored in the local worker memory will be read faster than from the disk but will be able to hold smaller amounts of data. OFF_HEAP memory should be used carefully because its data is also used by the JVM for internal processes.

## Caching a subset of the data

We can also explicitly select only a portion of data to be cached in the worker local storage by using the CACHE SELECT command in the following way:

```
CACHE SELECT col1, col2, col3
FROM our_table
WHERE our_condition
```
