serverStatus is less suited for real-time monitoring due to the volume of data, but it is essential for deep analysis, establishing performance baselines, and programmatic monitoring where specific metrics are extracted and tracked over time by external systems.

# Database profiler: Deep dive into slow operations

When you need to understand the performance characteristics of individual queries, the database profiler is the tool of choice. It captures detailed information about database operations for a specific database, allowing you to identify slow or inefficient queries that might impact application performance.

To get started, you can enable the profiler for a specific mongod instance using the setProfilingLevel() method from the MongoDB Shell. This method offers three profiling levels, each suited for different needs:

+   - **Level 0 (off)**: The profiler is disabled by default and does not collect any data
+   - **Level 1 (slow operations)**: It captures data only for operations slower than a specified threshold (slowms, default 100 ms)
+   - **Level 2 (all operations)**: It captures data for all operations (use with caution in production due to performance overhead)

Additionally, you can configure slowms, which is the slow operation threshold. Any user operation that completes faster than this will not be logged to the mongod log. sampleRate can also be set, which allows you to control what fraction of slow operations should be logged.

The following command, when run via the MongoDB Shell, will enable the database profiler for the current database with a slow operation threshold of 20 milliseconds and a sample rate of 42%:

```
db.setProfilingLevel(1, { slowms: 20, sampleRate: 0.42 })
```

Once enabled, you can query the profiler data to analyze recent operations. For example, this command finds and displays the five most recent profiled operations:

```
// Run from the MongoDB shell
// Find the 5 most recent profiled operations
db.system.profile.find().sort({ ts: -1 }).limit(5).forEach(printjson)
```

Profiled operations are stored in the system.profile capped collection in each database for which profiling has been enabled. Each document in this collection contains details such as the following: