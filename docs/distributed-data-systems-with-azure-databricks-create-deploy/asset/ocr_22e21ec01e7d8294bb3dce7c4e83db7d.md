# Automatically optimizing files with Delta Engine

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

![](bbox=[0.145, 0.545, 0.835, 0.695])

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