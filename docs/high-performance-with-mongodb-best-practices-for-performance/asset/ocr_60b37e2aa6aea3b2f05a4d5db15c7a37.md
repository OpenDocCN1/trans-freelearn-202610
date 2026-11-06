While you can choose to isolate different collections to different shards, most commonly, sharding is employed when you have a large enough collection that can’t be handled by a single shard. At this point, you have to shard or partition the collection, and you get to decide how it will be partitioned by selecting a shard key.

The shard key value is used to split the collection logically into ranges, called chunks. These chunks are then distributed across shards in a way that aims to balance the data evenly across all the shards. In older versions of MongoDB, this typically meant that each shard had roughly the same number of chunks, but in newer versions, chunks are only split when necessary. As a result, while data is still distributed evenly, this doesn’t necessarily translate into the same number of chunks on each shard.

Consider a simple example: if your shard key is username and the values of username range from aaaa to zzzz, then your chunks may be divided into ranges such as a-b, c-d, e-f, and so on. The balancer, a background process that runs on the sharded cluster, will automatically distribute data corresponding to these chunk ranges to all the shards, ensuring approximately equal distribution of data.

> **Balancer limitations in assessing shard key activity**
> 
> The balancer acts based on data size alone and cannot determine which ranges of shard key values may be more *active* than others.

Now, mongos will route operations that include username starting with a specific letter to the shard that *owns* the chunk range that includes this username.

You must be wondering, what happens to a request that doesn’t include a username? In such cases, mongos has to send the request to all the shards. This type of request is called a **broadcast** or **scatter-gather** operation, and it doesn’t scale horizontally nearly as effectively as targeted single-shard operations.

# Why scatter-gather is bad

It’s a common misconception that scatter-gather queries are efficient simply because each shard only has to process a small subset of the data. While this may seem intuitively true, and indeed mirrors how the original map-reduce data process worked (by splitting huge amounts of work across many servers), this approach doesn’t scale well horizontally in most cases. Let’s explore why that is.