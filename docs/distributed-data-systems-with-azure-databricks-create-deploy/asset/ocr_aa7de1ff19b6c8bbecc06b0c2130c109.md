The behavior of DFP concerning whether a process is enabled or not, the minimum size of a table, and the minimum number of files needed to trigger a process can be managed by the following options:

+   - `spark.databricks.optimizer.dynamicPartitionPruning` (default is true): Whether DFP is enabled or not.
+   - `spark.databricks.optimizer.deltaTableSizeThreshold` (default is 10 GB): The minimum size of the Delta table that activates DFP.
+   - `spark.databricks.optimizer.deltaTableFilesThreshold` (default is 1000): Represents the number of files of the Delta table on the probe side of the join required to trigger DFP. If the number of files is low, it's not worthwhile triggering the process.

DFP can be used for non-partitioned tables, nonselective joins, and when performing DML operations on them.

The improvement obtained by applying DFP is correlated with the level of granularity of the data. Therefore, it might be a good idea to apply Z-Ordering when working with DFP.

# Using DFP

To first understand how we apply DFP, let's see an example of where we manually use literal statements and another Delta Lake feature called **dynamic partition pruning (DPP)**.

During the physical planning phase, Delta Lake creates an executable plan for the query. This plan distributes the computation across the clusters of many workers. This plan is optimized to only scan files relevant to the query.

If we were to run a query but we wanted to explicitly filter the results based on a condition. A more concrete example of this would be to select col1, col2, and col3 from our table, and apply a filtering condition on one of the columns based on a maximum value, as illustrated in the following code snippet:

```
SELECT col1,col2,col3
FROM our_table
WHERE col1> max_value
```

Delta Lake keeps information on the minimum and maximum value in our columns, so it will automatically skip files that are not relevant to our query, based on our filtering condition.