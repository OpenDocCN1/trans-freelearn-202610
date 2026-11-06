
## Query Shapes and Query Profiler

Within Query Insights, Atlas provides a user-friendly interface for surfacing insights about logged operations. Operations are logged to mongos/mongod logs when they exceed the slowms threshold (which defaults to 100 milliseconds).

The Query Shapes tab groups these operations by shape and allows you to see their performance metrics. The Query Profiler can help you see more details to diagnose and resolve performance bottlenecks with greater precision through the following:

- **Slow query identification:** Detects queries that consistently exceed the slowms threshold. This can occur on a collection scan (reading entire collections without using an index) or operations that aren’t optimally indexed. If you see a slow query with planSummary: COLLSCAN, consider an appropriate index.
- **Recent slow operations:** List of operations exceeding your configured threshold, along with key details such as execution time, namespace (database and collection), and documents scanned versus returned. If execution time is high and the ratio of documents examined versus documents returned is elevated, optimize the query or add indexes to reduce unnecessary scanning.
- **Execution statistics:** Drill down into individual slow operations to examine their execution plan, index usage, read/write latency, and response size. Add or adjust indexes as needed. Large response sizes may indicate the need for projections to limit returned fields.
- **Filter and drill down:** Narrow down the profiler output by time range, operation type, or namespace to isolate specific issues. Use this to correlate slowdowns with application changes, deployment events, or specific workloads.

In practice, use the Query Insights in tandem with Performance Advisor to confirm that new indexes or query changes yield the expected latency improvements.

## Alerts

Atlas issues alerts for the database and server conditions configured in your alert settings. When a condition triggers an alert, Atlas displays a warning symbol on the cluster and sends alert notifications. Your alert settings determine the notification methods. Atlas continues sending notifications at regular intervals until the condition resolves or you delete or disable the alert.

Atlas has a number of predefined alerts at both the organization and project levels; however, you can create custom alerts as well. Let’s look at some common alert categories and how the underlying issues can be addressed.

### Atlas Search issues

Atlas Search triggers alerts when the amount of CPU and memory used by Atlas Search processes reaches a specified threshold. If the search process (mongot) runs out of memory, indexing and queries fail.

Atlas Search alerts often occur when you try to build a large or complex search index. These indexes remain in the Initial Sync phase until you resolve the memory issue.

If the search process runs out of memory or disk space, you can upgrade your cluster to fix the immediate problem. You can select a cluster tier with more memory, storage, and IOPS, or consider a dedicated search node for even greater availability, performance, and workload balancing.

### Connection issues

Connection alerts typically occur when the maximum number of allowable connections to a MongoDB process has been exceeded. Once the limit is exceeded, no new connections can be opened until the number of open connections drops below the limit.

Connection alerts are generally a symptom of a larger problem. If these alerts are being triggered often, a proper solution will likely involve the following:

- Examining your database applications for flawed connection code. Situations in which connections are opened but never closed can allow old connections to pile up and eventually exceed the connection limit. Additionally, you may need to implement some form of connection pooling.
- Upgrading to a larger Atlas cluster tier, which allows a greater number of connections, if your user base is too large for your current cluster tier.

By identifying the root cause and addressing it, whether in your application code or cluster configuration, you can prevent recurring connection issues and ensure smoother, more reliable database performance.

### Query issues

The query targeting alert typically occurs when there is no index to support a query or queries, or when an existing index only partially supports a query or queries. Examples of these alerts are as follows:

- **Query Targeting: Scanned Objects / Returned:** These alerts are triggered when the average number of documents scanned relative to the average number of documents returned server-wide across all operations during a sampling period exceeds a defined threshold. The default alert uses a 1000:1 threshold. Ideally, the ratio of scanned documents to returned documents should be close to 1. A high ratio negatively impacts query performance.
- **Query Targeting: Scanned / Returned:** These alerts occur if the number of index keys examined to fulfill a query relative to the actual number of returned documents meets or exceeds a user-defined threshold. This alert is not enabled by default.

The most common source of this issue is a missing index, which the Performance Advisor provides the easiest and quickest way to investigate. The Performance Advisor monitors queries that MongoDB considers slow and recommends indexes to improve performance. Identifying issues using these alerts, coupled with the concepts introduced in *Chapter 12, Advanced Query and Indexing Concepts*, can help you more effectively troubleshoot and address underperforming queries.

### Oplog issues

In *Chapter 5, Replication*, we learned about the **operations log (oplog)** and the role it plays in ensuring write operations are propagated to all replica set members. We also learned that the oplog is a capped collection of a fixed size (either by disk size or window size in time).

If a large amount of write operations are conducted on the primary node of the replica set and the secondary nodes do not have enough time to replay all of the operations contained in the oplog, this will typically result in “falling off the oplog,” which requires an initial sync in order to recover and ensure that the data is consistent across all nodes.

Atlas offers monitoring and alerting of events relevant to this scenario in the form of the following:

- **Replication Oplog Window is (X):** This occurs if the approximate amount of time available in the primary replication oplog meets or goes below the specified threshold. This refers to the amount of time that the primary can continue logging, given the current rate at which oplog data is generated.
- **Oplog Data Per Hour is (X):** This occurs if the amount of data per hour being written to a primary’s replication oplog meets or exceeds the specified threshold.

The most common trigger for these alerts is intensive write and update operations in a short period of time. When this occurs, some common mitigations are as follows:

- Increase the oplog size by editing your cluster’s configuration to ensure it is higher than the peak value from the Oplog GB/Hour graph in the cluster metrics view.
- Ensure that all write operations specify a write concern of majority to ensure that writes are replicated to at least one node before moving on to the next write operation. This controls the rate of traffic from your application by preventing the primary from accepting writes more quickly than the secondaries can handle.

To get the most value from your alerting system, regularly review and tune alert thresholds to match your workload and business requirements, which helps avoid both alert fatigue and missed incidents. Integrate alert notifications with your team’s preferred communication and incident management tools to ensure rapid response. Treat alerts as actionable signals: when triggered, promptly investigate the root cause, take corrective action (such as scaling resources, optimizing queries, or adjusting thresholds), and document the resolution for future reference.

Periodically audit the configured alerts to ensure they remain relevant as your application and infrastructure evolve. Finally, use alert history and trends to inform capacity planning, identify recurring issues, and drive continuous improvement in your monitoring strategy.

For a full list of available alert conditions, see the MongoDB Atlas documentation at https://www.mongodb.com/docs/atlas/reference/alert-conditions/.

## Self-managed monitoring tools

When operating MongoDB outside of the managed Atlas environment, you are responsible for assembling your own observability pipeline. Fortunately, MongoDB provides a suite of built-in command-line utilities and database commands designed to offer deep insights into the server’s real-time activity and historical performance. Mastering these tools is essential for diagnosing issues, understanding workload characteristics, and ensuring the health of your self-hosted deployments.

### mongostat: real-time activity snapshot

Think of mongostat as a MongoDB-specific monitoring utility that provides a dynamic, real-time view of the status of a running mongod or mongos instance. By sampling the server status periodically (by default, every second), it presents key metrics in a concise, tabular format, allowing you to quickly assess the current operational load and resource utilization.
