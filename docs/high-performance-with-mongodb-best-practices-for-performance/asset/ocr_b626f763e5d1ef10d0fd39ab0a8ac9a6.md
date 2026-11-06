If you have a $group stage, and the group key is (or includes) the shard key, each shard can perform the full group operation locally, with the pipeline continuing locally, and mongos only needs to return the results to the client without additional processing. If the group key is different, or if there is another blocking stage, then the shards perform the initial part of that stage, and mongos does a final merge and all the following stages, which is more work for mongos.

Merges are frequently unavoidable, but you can minimize their impact. The earlier stages in your pipeline ($match) should aggressively filter out documents. The less data each shard sends to mongos for merging, the faster the merge operation will be.

# Monitoring and profiling aggregation performance

Even the most carefully crafted aggregation pipeline can run into performance issues. That’s why monitoring and profiling are essential skills for any MongoDB practitioner.

You saw briefly how the explain() method is your window into the inner workings of an aggregation pipeline. It reveals the execution plan, showing how each stage is processed, which indexes are used, and where the heavy lifting happens. We’ll show a lot more about explain in Chapter 12, Advanced Query and Indexing Concepts.

To get the explain output for an aggregation, append .explain() to your collection name before the aggregate() command, use the {explain:true} option inside aggregate, or in mongosh, append .explain() to the end of your aggregate method. Common verbosity modes include the following:

+   - **explain("queryPlanner")**: Shows the plan selected by the query optimizer, including stages as they will be executed and how indexes are utilized
+   - **explain("executionStats")**: Executes the query and returns detailed statistics about the execution, such as the number of documents returned (nReturned), total documents examined (totalDocsExamined), total execution time after each stage (execTimeMillis), and index usage
+   - **explain("allPlansExecution")**: Same as executionStats but includes additional information and statistics about rejected query plans in the $cursor stage

The primary goal of using explain() is to find opportunities for improvement. Look for COLLSCAN (collection scan) as an inputStage for $match or $sort operations where you expected an IXSCAN (index scan). COLLSCAN means MongoDB had to read all documents in the collection to find the relevant ones, which is usually inefficient.