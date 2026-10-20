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

![](bbox=[0.26, 0.41, 0.7, 0.54])

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
```