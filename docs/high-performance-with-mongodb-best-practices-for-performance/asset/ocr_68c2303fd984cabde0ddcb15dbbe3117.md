# Core MongoDB metrics and signals

Understanding the internal state of your MongoDB deployment is crucial for maintaining optimal performance and diagnosing issues. MongoDB exposes a comprehensive set of metrics that provide insights into its health, resource utilization, and operational characteristics. By monitoring and correlating these core signals, you can proactively identify potential bottlenecks, make informed decisions about capacity planning, and ensure that your database effectively supports your application workloads.

These metrics generally fall into two categories: operational metrics, which give a real-time view of the database’s current activity, and performance-specific metrics, which delve deeper into workload behavior and potential areas of contention.

# Operational metrics

Operational metrics answer the question How is my database operating right now?. These are exposed by the serverStatus command and can be collected via the shell, drivers, or external monitoring solutions such as Prometheus, as discussed later in the Integration with external monitoring systems section.

The following example demonstrates how to extract a few of the most important operational metrics using the MongoDB Shell. This code retrieves the current server status and prints a selection of key metrics that are essential for monitoring real-time health and workload:

```
// Run from the MongoDB shell
// Example: Extract key operational metrics in the MongoDB shell
const status = db.serverStatus();
printjson({
    opcounters:    status.opcounters,    // reads, writes, commands
    memUsageMB:    status.mem.resident,  // resident memory in MB
    cacheStats:    status.wiredTiger.cache,  // WiredTiger cache
    utilization
    connections:   status.connections    // active vs available
});
```

The preceding code reveals several of the most actionable metrics for day-to-day monitoring, but it’s important to note that serverStatus exposes a much broader set of metrics covering nearly every aspect of MongoDB’s operation. In the following list, we highlight only some of the most relevant fields for practical, ongoing observability. Here’s what each field represents, and where to find related metrics not shown in the code: