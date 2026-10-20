
The output is as follows:

```
%sql
select action, date_format(window.end, "MMM-dd HH:mm") as time, count from data_counts order by time, action
```

![](img/7e45bd08916c2a17d4e75cbb898f41e8_208_0.png)

Figure 6.6 – Windowed counts on a streaming data frame

9. In the following code example, we will use a similar definition to the one used here but using the readStream option instead of the read method. Also, in order to simulate a stream of files, we will pick one file at a time using the maxFilesPerTrigger option and setting it to 1:

```
from pyspark.sql.functions import *
streaming_df = (
    spark
    .readStream
    .schema(json_schema)
    .option("maxFilesPerTrigger", 1)
    .json(input_path)
)
```

10. We can then make the same query that we have applied to the static dataframe previously, as follows:

```
streaming_counts_df = (
    streaming_df
    .groupBy(
        streaming_df.action,
        window(streaming_df.time, "1 hour"))
    .count()
)
```

11. We can check that the data frame is actually a streaming source of data by running the following code:

```
streaming_df.isStreaming
```

The output is as follows:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_209_0.png)

Figure 6.7 – Checking if the dataframe is a streaming source

As you can see, the output of streamingCountsDF.isStreaming is True, therefore streaming_df is a streaming dataframe.

12. The next option is set, to keep the size of shuffles small, as illustrated in the following code snippet:

```
spark.conf.set("spark.sql.shuffle.partitions", "2")
```

13. We can then store the results of our query, passing as format "memory" to store the result in an in-memory table. The queryName option will set the name of this table to data_counts. Finally, we set the output mode to complete so that all counts are on the table. The code can be seen in the following snippet:

```
query =
    streaming_counts_df
    .writeStream
    .format("memory")
    .queryName("data_counts")
    .outputMode("complete")
    .start()
```

The query object is a handle that runs in the background, continuously looking for new files and updating the windowed resulting aggregation counts.

Note the status of query in the preceding code snippet. The progress bar in the cell shows the status of the query, which in our case is active.

14. We will artificially wait some time for a few files to be processed and then run a query to the in-memory data_counts table. First, we introduce a sleep of 5 seconds by running the following code:

```
from time import sleep
sleep(5)
```

15. Then, we can run a query in another cell as a SQL command, using SQL magic, as follows:

```
%sql
select action, date_format(window.end, "MMM-dd HH:mm") as time, count
from data_counts
order by time, action
```

The following screenshot shows us the resulting graph created by running this query in one of the Databricks notebook code cells:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_210_0.png)

Figure 6.8 – The counts before waiting

We can see the timeline of windowed counts growing bigger. Running these queries will always result in the latest updated counts, which the streaming query is updating in the background.

16. If we wait a few seconds more, we will have new data computed into the result table, as illustrated in the following code snippet:

```
sleep(5)
```

17. And then, we run the query again, as follows:

```
%sql
select action, date_format(window.end, "MMM-dd HH:mm") as time, count
from data_counts
order by time, action
```

We can see in the following screenshot of a graph created by running the query that the results at the right side of the graph have changed as more data is being ingested:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_211_0.png)

Figure 6.9 – The counts after waiting

18. We can also see the resulting number of "opens" and "closes" by running the following SQL query:

```
%sql
select action, sum(count) as total_count
from data_counts
group by action order by action
```

This example has its limitations due to the small number of files present in the dataset. After consuming them all, there will be no more updates to the table.

19. Finally, we can stop the query running in the background, either by clicking on the Cancel link in the cell of the query or by executing the query.stop function, as illustrated in the following code snippet:

```
query.stop()
```

Either way, the status of the cell where the query is running will be updated to TERMINATED.

## Summary

Throughout this chapter, we have reviewed different features of Structured Streaming and looked at how we can leverage them in Azure Databricks when dealing with streams of data from different sources.

These sources can be data from Azure Event Hubs or data derived using Delta tables as streaming sources, using Auto Loader to manage file detection, reading from Apache Kafka, using Avro format files, and through dealing with data sinks. We have also described how Structured Streaming provides fault tolerance while working with streams of data and looked at how we can visualize these streams using the display function. Finally, we have concluded with an example in which we have simulated JSON files arriving in the storage.

In the next chapter, we will dive more deeply into how we can use the PySpark API to manipulate data, how we can use Python popular libraries in Azure Databricks and the nuances of installing them on a distributed system, how we can easily migrate from Pandas into big data with the Koalas API, and—finally—how to visualize data using popular Python libraries.

## Section 3:
Machine and Deep Learning with Databricks

This section explores concepts around the use of machine learning and deep learning models. At the end of this section, you will be able to create, train, and deploy predictive models harnessing the power of distributed computing.

This section contains the following chapters:

- Chapter 7, Using Python Libraries in Azure Databricks
- Chapter 8, Databricks Runtime for Machine Learning
- Chapter 9, Databricks Runtime for Deep Learning
- Chapter 10, Model Tracking and Tuning in Azure Databricks
- Chapter 11, Managing and Serving Models with MLflow and MLEap
- Chapter 12, Distributed Deep Learning in Azure Databricks

## 7
Using Python Libraries in Azure Databricks

Azure Databricks has implementations on different programming languages, but we will focus on Python developers, therefore we will explore all the nuances regarding working with it, as well as introducing core concepts regarding models and data that later will be studied in more detail.

In this chapter, we will cover the following:

- Installing popular Python libraries in Azure Databricks
- Learning key concepts of the PySpark API
- Using the Koalas API to manipulate data in a similar way as we would do with pandas
- Using visualization libraries to make plots and graphics

These concepts will be introduced more deeply in the next sections of this chapter.

## Technical requirements

This chapter will require you to have an Azure Databricks subscription available to work on the examples in notebooks attached to running clusters.

Without further ado, we can start by looking into the different ways in which we can install libraries in Azure Databricks and the differences of each method.

## Installing libraries in Azure Databricks

We can make use of third-party or custom code by installing libraries written in Python, Java, Scala, or R. These libraries will be available to notebooks and jobs running on your clusters depending on the level at which the libraries were installed.

In Azure Databricks, installing libraries can be done in different ways, the most important decision being at which level we will be installing these libraries. The options available are at the workspace, cluster, or notebook level:

- **Workspace** libraries serve as a local repository from which you create cluster-installed libraries. A workspace library might be custom code created by your organization or might be a particular version of an open-source library that your organization has standardized on.
- **Cluster** libraries are available to be used by all notebooks attached to that cluster. You can install a cluster library from a public repository or create one from a previously installed workspace library.
- **Notebook-scoped**: Python libraries allow you to install Python libraries and create an environment scoped to a notebook session. Notebook-scoped libraries will not be visible in other notebooks running on the same cluster. These libraries do not persist and need to be installed at the beginning of each session. Use notebook-scoped libraries when you need a custom Python environment for a specific notebook. With these libraries, it is easy to create, modify, save, reuse, and share Python environments. Notebook-scoped libraries are available using `%pip` in all Databricks Runtime instances and using the `%conda` magic commands in Databricks Runtime ML 6.4.
