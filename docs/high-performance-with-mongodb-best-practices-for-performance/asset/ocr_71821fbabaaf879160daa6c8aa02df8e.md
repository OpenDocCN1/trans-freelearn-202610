Here are some recommended sharded cluster optimization techniques:

+   - **Configure periodicNoopIntervalSecs appropriately** (default: 10 seconds) to **reduce latency** from cold- or low-write shards. These no-op operations ensure that oplog entries are regularly created, even when no real writes occur on the primary, allowing members of the shard to update their logical clocks and replication positions, thus reducing coordination traffic to colder shards.
+   - **Target change streams at specific collections** whenever possible. This significantly reduces cross-shard communication overhead, especially for high-write collections.
+   - **Apply server-side filtering using $match pipelines** for high-activity collections.
+   - **Place change stream consumers geographically close** to your primary shards to minimize network latency.
+   - **Monitor each shard’s oplog window independently** to detect potential issues that may impact change stream-based services.
+   - **Limit the number of document-specific change streams.** For example, instead of creating hundreds of change streams to watch individual documents, create a single change stream and filter the relevant events in your application.

In summary, running change streams efficiently in a sharded cluster requires careful attention to both the configuration and deployment strategy. By tuning parameters such as `batchSize` and `maxAwaitTimeMS`, targeting streams at specific collections, and reducing unnecessary coordination across shards, you can significantly improve performance and reliability. As with most distributed systems, continuous monitoring and iterative tuning are key to maintaining optimal behavior at scale.

# Advanced patterns and production readiness

Taking change streams to production requires understanding advanced options and monitoring needs. In this section, we’ll explore key considerations for production deployments, based on the official MongoDB documentation.

# Transaction visibility and event batching

When working with transactions in MongoDB, change streams follow specific visibility rules that affect event delivery. All changes made within a transaction only become visible to change streams after the transaction commits. At that point, all changes from that transaction appear at once as separate events in the stream while maintaining their logical order within the transaction.