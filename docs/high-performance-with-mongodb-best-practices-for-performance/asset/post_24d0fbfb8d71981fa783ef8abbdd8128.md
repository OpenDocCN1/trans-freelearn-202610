
mongos is the most interesting component here because from the point of view of the driver or client application, it acts as if it’s the database, accepting CRUD operation requests and returning results to the driver. On the flip side, it acts as the proxy for the driver when it’s talking to shards, sending them CRUD operations, and then forwarding the result back to the driver. Normally, there are multiple mongos processes in a production sharded cluster. Figure 6.1 shows all the components of a sharded cluster and how the application connects to it.

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_155_0.png)

Cluster with a dedicated config server

*Figure 6.1: Sharded cluster architecture*

But how does mongos know which shards to forward the operations to? Based on the information persisted in the config servers, mongos keeps a current routing table, meaning for each collection, it knows where its data lives, and if the collection is sharded, it tracks where each part of the collection is located. It then forwards the operations it receives to one or more shards, as needed, and if it’s more than one shard, then it also has to merge or combine results from each of the shards to return a single result to the client.

## Sharding a collection and selecting a shard key

When you first deploy a sharded cluster, you have to decide how you want to distribute the data across the shards. This is critical for achieving horizontal scalability because a scalable sharded cluster distributes data in a way that operations are divided equally across all the shards. This means you want all your read and write operations to be spread roughly equally across all the shards.

While you can choose to isolate different collections to different shards, most commonly, sharding is employed when you have a large enough collection that can’t be handled by a single shard. At this point, you have to shard or partition the collection, and you get to decide how it will be partitioned by selecting a shard key.

The shard key value is used to split the collection logically into ranges, called chunks. These chunks are then distributed across shards in a way that aims to balance the data evenly across all the shards. In older versions of MongoDB, this typically meant that each shard had roughly the same number of chunks, but in newer versions, chunks are only split when necessary. As a result, while data is still distributed evenly, this doesn’t necessarily translate into the same number of chunks on each shard.

Consider a simple example: if your shard key is username and the values of username range from aaaa to zzzz, then your chunks may be divided into ranges such as a-b, c-d, e-f, and so on. The balancer, a background process that runs on the sharded cluster, will automatically distribute data corresponding to these chunk ranges to all the shards, ensuring approximately equal distribution of data.

> **Balancer limitations in assessing shard key activity**
> 
> The balancer acts based on data size alone and cannot determine which ranges of shard key values may be more *active* than others.

Now, mongos will route operations that include username starting with a specific letter to the shard that *owns* the chunk range that includes this username.

You must be wondering, what happens to a request that doesn’t include a username? In such cases, mongos has to send the request to all the shards. This type of request is called a **broadcast** or **scatter-gather** operation, and it doesn’t scale horizontally nearly as effectively as targeted single-shard operations.

## Why scatter-gather is bad

It’s a common misconception that scatter-gather queries are efficient simply because each shard only has to process a small subset of the data. While this may seem intuitively true, and indeed mirrors how the original map-reduce data process worked (by splitting huge amounts of work across many servers), this approach doesn’t scale well horizontally in most cases. Let’s explore why that is.

When a query request goes to a single shard, that shard executes the operation and sends the results back to mongos, which forwards them to the client. But when a query request targets all the shards (a scatter-gather operation), each shard has to execute the operation on its own portion of the data and send the result back to mongos to merge and forward it to the client. Notice something about these two scenarios? In targeted queries, only one shard has to execute the operation. In scatter-gather queries, every shard has to execute the operation.

So, in a cluster with 20 shards, a single scatter-gather operation is translated into 20 operations. Even though it may seem like a cheaper operation, processing only 1/20th of the total data, the operation execution overhead is still the same for each shard, and mongos has to do the extra work of merging the results. In some cases, mongos cannot start returning data to the client until it has heard back from every shard that received the query.

Suppose your cluster is processing tens or hundreds of thousands of queries. Here’s how a single request going to one shard versus a scatter-gather query scaling across a 20-shard cluster will look:

| Client queries | Targeted queries only | Scatter-gather queries only |
|---|---|---|
| 100 | 100 | 2,000 |
| 1,000 | 1,000 | 20,000 |
| 10,000 | 10,000 | 200,000 |
| 100,000 | 100,000 | 2,000,000 |

*Table 6.1: Table showing total queries executing across all 20 shards*

Now, imagine if you were to double the number of shards in Table 6.1. The targeted queries column will remain the same, but the scatter-gather queries column will double! So, you don’t get the benefit of linear scaling because increasing the number of shards in your cluster will increase the total number of queries too. As an additional detriment, scatter-gather queries that need to hear from all the shards increasingly get slower as the number of shards grows. This is because the chances of one shard experiencing a latency hiccup (such as a network misconfiguration, an election, or a crash) also increase. This contributes to additional latency to every broadcast query, unlike targeted queries, which only impact a subset of queries directed to the impacted shard.

## Strategic shard key selection

Shard key selection is a critical decision that directly impacts the performance and scalability of a sharded cluster. A good shard key can target the operations to specific shards, provide appropriate granularity to evenly distribute data, and minimize the risk of creating hot shards. Let’s look at some important elements of a good shard key.

### Shard key for targeting operations

To understand how data partitioning influences the distribution of operations, let’s look at another example of how collections can be partitioned across shards based on the defined shard key.

Suppose you have an orders collection. You might partition it by order_id, which is unique and will allow many chunk ranges to be created, allowing fine distribution of data across shards. However, if order_id is monotonically increasing, then the last chunk (the one from a high order_id to maxInt range) will always get all the new inserts.

That may or may not be a problem. If new order creation is the bottleneck in your system, this could be problematic, but if new orders are created relatively infrequently and the majority of the load is from orders being updated, read, or aggregated, it may not be a big issue.

Now, let’s consider an operation that reads all recent orders for a particular customer. Since you don’t know which range the customers’ orders are in, you will have to broadcast the query to all the shards that have chunks for the orders collection, as shown in Figure 6.2, and that’s problematic for horizontal scalability.

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_158_0.png)

*Figure 6.2: Broadcasting query*

If you know that your application will always have the customer_id value when it’s dealing with an order (a reasonable assumption), it would be better to use a compound shard key such as {customer_id:1,order_id:1}. This will keep each customer’s orders on a single shard (since they will tend to fall within a single chunk), and queries about a particular customer’s orders will be targeted to a single shard instead of scattered to all shards.
