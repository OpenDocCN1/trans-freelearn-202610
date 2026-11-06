# Optimizing aggregation for sharded collections

In a sharded cluster, aggregation pipelines can be executed in parallel across shards. MongoDB tries to push as much work as possible to each shard that holds the data needed for aggregation, minimizing the amount of data that needs to be transferred over the network.

If an aggregation query can be satisfied by data residing on a single shard (or a subset of shards), mongos will route the query only to the relevant shard(s). This is the most efficient scenario as it minimizes cross-shard communication and coordination overhead. This typically happens when the query includes a filter on the shard key, or the collection being aggregated only lives on one or a small subset of shards.

If the aggregation cannot be targeted (e.g., no filter on the shard key and data lives on multiple shards), mongos performs a scatter-gather operation. It sends the first part of the pipeline to all shards that hold data for the target collection. mongos then gathers the results and merges them by running the rest of the pipeline itself. There are also scenarios where the merge stage must happen on a selected shard rather than on mongos.

The choice of shard key can make or break your aggregation performance. A well-chosen shard key can enable many aggregation queries to be targeted to a single shard, dramatically improving performance. Conversely, a poorly chosen shard key can lead to most aggregations becoming scatter-gather operations, increasing latency and load on the cluster.

# Understanding shard-local versus merged operations

Not all pipeline stages are created equal. Some can be executed on individual shards, while others must be merged on mongos. Use the explain() command to determine where each stage runs. The output will show how the pipeline is split, and which parts are executed on the shards and which on mongos.

All streaming stages can run on shards, as well as the first parts of blocking stages. In some special cases, even $group can run entirely on a shard, if you are grouping by the shard key. The final part of the first blocking stage that cannot run entirely on the shard, and all stages that follow, will have to run on the merging location, which is usually mongos.

Keep as much processing as possible on the shards to reduce network traffic and speed up your pipeline. If an aggregation can run entirely on a single shard, it avoids network latency and the merging overhead on mongos.