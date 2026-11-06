While having allowDiskUse enabled by default provides a safety net for memory-intensive operations, it’s important to be aware that disk I/O is significantly slower than in-memory operations. You may want to explicitly disable it by setting allowDiskUse:false in scenarios where low latency is critical and it’s preferable for operations to fail fast rather than potentially becoming slow due to disk usage.

Only certain aggregation stages are subject to this limit; they are all blocking stages:

+   - **Unindexed $sort operations on large datasets:** Sorting a large number of documents *without the benefit of an index* and without an immediate $limit following it requires all documents to be sorted to be maintained in memory
+   - **$group with high cardinality keys and/or large result documents for each key:** If the field(s) used in the _id of a $group stage have many unique values and/or if each grouping has to maintain a large amount of data, the stage needs to maintain a large amount of data for each distinct group in memory
+   - **$bucket, $bucketAuto, and $setWindowFields:** These stages are variants of $group and can consume significant memory in the same circumstances as $group

![](bbox=[0.14, 0.56, 0.2, 0.63])

# Blocking stages and memory usage considerations

Not all blocking stages are able or likely to exceed this stage limit. For instance, $count is a blocking stage that needs to count all the incoming documents, but since it only outputs a single document as its result, its memory use is trivially tiny. The $sortByCount stage is a blocking stage that groups all of its input into small documents containing only two fields, but then it has to sort them in memory. If you don’t specify a $limit stage after the $sortByCount and you have well over 1 million distinct group key values, then the result could possibly exceed 100 MB, but that scenario is not very likely. You would likely use $limit rather than returning millions of results to the client.

# Aggregation in distributed environments

Sharding, which we cover in Chapter 6, Sharding, is MongoDB’s secret sauce for scaling horizontally, but it adds a layer of complexity to aggregation pipelines. When your data is spread across multiple shards, understanding how aggregation works in this distributed environment is key to maintaining performance and avoiding bottlenecks.