Additionally, Figure 15.9 illustrates a significant observation: over the past year, write IOPs for shard-0 have increased at a faster rate compared to other shards in the cluster.

![](bbox=[0.13, 0.17, 0.88, 0.65])

*Figure 15.9: Graph showing the last year of write IOPS on shard-0*

# Diagnosing the cause

The root cause of the imbalance stemmed from the cluster’s initial configuration. The cluster contained a single database with numerous collections, and when the database was first created, shard-0 was designated as the primary shard. As a result, all unsharded collections were placed on shard-0 by default. Over time, as the number of unsharded collections and their respective data volumes grew, shard-0 became overloaded with disproportionate workloads.

This uneven distribution led to a higher concentration of operations and data on shard-0, causing it to consistently experience elevated CPU, memory, and disk usage compared to the other shards. The heavy utilization of shard-0 also increased query latency and limited the overall throughput of