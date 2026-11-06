
Write concern for replica sets describes the number of data-bearing members (i.e., the primary and secondaries, but not arbiters) that must acknowledge a write operation before the operation returns as successful.

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_150_0.png)

*Figure 5.4: Write concern with majority acknowledgment in MongoDB*

An application that issues a write operation waits until the primary receives acknowledgment from the required number of members for the specified write concern. For a write concern of w greater than 1 or w: "majority", the primary waits until the required number of secondaries acknowledge the write before returning write concern acknowledgment. For a write concern of w:1, the primary can return write concern acknowledgment as soon as it locally applies the write, since it is eligible for contributing to the requested write concern.

The more members that acknowledge a write, the less likely the written data is to roll back if the primary fails. However, specifying a high write concern can increase latency as the client must wait until it receives the requested level of write concern acknowledgment.

Write concern is specified using two primary parameters: w and wtimeout. The first parameter (w) determines the number of nodes that must acknowledge the write, and can accept value configurations:

- Number values (1, 2, 3): Specific number of replica set members
- "majority": (Default) A majority of voting members must acknowledge
- Replica set tags: Custom-defined subset of replica set members

The second parameter (wtimeout) specifies a time limit, in milliseconds, for a write operation to propagate to enough members to achieve the write concern after the operation succeeds on the primary. If the wtimeout value is exceeded and a write concern error is returned to the client, the write operation will likely still succeed; however, the client will not be waiting longer than this period for acknowledgement. By default, wtimeout is “infinity”, so if you want your application to return an error rather than wait until replication to a calculated majority of the cluster succeeds, you should provide your own appropriate wtimeout value.

The following example demonstrates setting a custom write concern on an insert operation that ensures the write has been acknowledged by the calculated majority of nodes and will return a write concern error if the write hasn’t been acknowledged within 5 seconds:

```
db.collection.insertOne(
  { item: "example" },
  {
    writeConcern: {
      w: "majority",
      wtimeout: 5000
    }
  }
);
```

All MongoDB drivers provide the ability to specify write concerns at both the connection level and on a per-operation basis, allowing fine-grained control.

You can modify the global default write concern by issuing the setDefaultRWConcern command. From the MongoDB Shell, assuming you have the appropriate privileges to do so, you could modify this value as follows:

```
db.adminCommand({
  "setDefaultRWConcern" : 1,
  "defaultWriteConcern" : {
    "w" : 2
  }
})
```

The preceding example sets the global write concern to w:2. If you issue a write operation with a specific write concern, the write operation uses its own write concern instead of the default.

We recommend leaving w at its default "majority" value, even though it may seem that latency would be better without waiting for replication. Keep in mind that if the secondaries start falling behind, flow control will kick in and start throttling your writes.

## Summary

MongoDB’s replication system provides a robust foundation for building highly available and fault-tolerant database deployments. Replica set architecture, combined with automatic failover through elections, ensures that your database can withstand node failures while maintaining data integrity. The oplog serves as the backbone of the replication process, allowing secondary nodes to stay in sync with the primary.

When designing your replica set, consider factors such as the geographical distribution of data and the appropriate oplog size for your workload characteristics. These decisions will significantly impact your database’s resilience, performance, and ability to meet your application’s specific needs.

Remember that proper configuration is not a one-time task but an ongoing process that should evolve with your application’s growth and changing requirements. Through continuous observation and iterative improvements, you can maintain optimal replication behavior even as your MongoDB deployment scales.

In the next chapter, we’ll explore how MongoDB’s sharding works with replication to provide horizontal scalability for deployments with large datasets and high throughput requirements.

## 6

## Sharding

As modern applications generate and store increasingly vast amounts of data, ensuring high performance and scalability becomes a key concern for any database system. MongoDB’s sharding architecture is a solution designed to provide horizontal scalability for applications with massive datasets and demanding throughput requirements. Unlike traditional vertical scaling, which often results in expensive and size-limited hardware upgrades, sharding offers a more efficient and cost-effective way to scale by distributing data across multiple servers. Understanding sharding architecture will help you handle spikes in user activity, global expansion, and growing data complexity.

This chapter delves into the foundational principles of MongoDB’s sharding architecture, enabling you to understand how sharding interacts with replication for high scalability and reliability. We’ll begin by examining the core components of a sharded cluster and their roles in routing and data distribution. From there, we’ll explore critical aspects such as selecting an optimal shard key. Finally, we’ll address practical considerations such as shard key refinement, strategic data placement, and techniques for handling large-scale imports and resharding operations. By the end of this chapter, you’ll have a strong grasp of MongoDB’s sharding ecosystem, enabling you to design scalable architectures that support your application’s evolving data demands.

This chapter will cover the following topics:

- How sharding enables horizontal scalability by distributing data across replica sets
- The roles and functionalities of shards, mongos processes, and config servers in a sharded cluster
- Strategies for selecting an efficient shard key to optimize data distribution and query targeting
- Comparing range-based, hashed, and zone-based sharding methods for varying workloads
- Using resharding to redistribute data and address performance bottlenecks without downtime
- Techniques for colocating related data to improve transaction and aggregation query performance
- Configuring and monitoring the balancer to manage chunk migrations effectively
- Approaches to refine shard keys to resolve issues such as jumbo chunks or poor granularity

## Understanding core sharding architecture

MongoDB’s sharding works with replication to provide **horizontal scaling** for deployments with large datasets and high-throughput requirements. Traditionally, when a workload exceeded the capabilities of the database server, it had to be upgraded to a bigger server. This is known as **vertical scaling**, and it’s limited in terms of both the maximum size of any particular server and the price-performance ratio; supercomputers tend to be very expensive! Instead, if you can distribute the workload to multiple smaller servers, you can theoretically scale your workload horizontally nearly infinitely, with, ideally, the costs rising linearly with the number of servers. This is what MongoDB sharding allows you to do when your workload outgrows the capabilities of a single replica set.

If your workload has multiple distinct databases that are relatively isolated from each other, there is nothing wrong with deploying a separate replica set for each one of them. For instance, you might have a separate replica set for users, another for accounts, and a third one for user activity. However, over time, even a single cluster may no longer be able to handle a distinct subset of your overall application workload. That’s when it becomes necessary to shard or partition the workload across multiple shards, each one of which is itself a replica set.

So, when your data grows beyond what a single replica set can handle, sharding distributes it across multiple replica sets, each containing a subset of your data. This horizontal scaling allows MongoDB to handle massive datasets and high throughput requirements. This section reviews what the architecture of a sharded cluster is and its important performance elements.

## Architectural components of a sharded cluster

A sharded cluster consists of several components: **shards** (each a replica set responsible for a subset of data in the cluster), **mongos processes** (which are primarily responsible for routing operations to different shards and returning results back to the client), and **config servers** (a special replica set that maintains metadata about sharding configuration and location of data in the cluster). Starting from MongoDB version 8.0, config servers don’t need to be separate servers and can be embedded with one of the shards.
