Additionally, if your application is globally distributed, it’s important to account for cross-region transaction latency. To mitigate this, consider distributing your collections in such a way that your writes are limited to a single region or explore other techniques to balance consistency and performance.

# WiredTiger cache considerations

MongoDB’s WiredTiger storage engine uses an in-memory cache to improve performance, but transactions interact with this cache in ways that can sometimes cause performance issues.

Transactions rely on cache by creating snapshots of documents they read or modify. These snapshots must remain in the cache until the transaction is complete. Long-running transactions can prevent cache eviction, leading to memory pressure.

The most serious issue is when this leads to the TransactionTooLargeForCache error, which occurs when a transaction’s operations and snapshots exceed the available cache size. This commonly happens in the following instances:

+   - A transaction touches too many documents
+   - A transaction includes very large documents
+   - A transaction runs for too long, preventing cache eviction
+   - The WiredTiger cache is configured too small for the workload

To prevent these scenarios, there are a few strategies depending on the situation:

+   - **Limit transaction size:** Keep the number of documents per transaction below 1,000
+   - **Limit transaction duration:** Commit or abort transactions within 60 seconds (the default limit)
+   - **Configure appropriate cache size:** Ensure the WiredTiger cache is set to at least 50% of RAM for transaction-heavy workloads
+   - **Monitor cache pressure:** Watch for an increase in metrics such as `wiredTigerCacheSizeBytes` and `wiredTigerCacheMaxBytes`

Ensuring good transaction performance requires using best practices, diligent monitoring, and minimizing transaction use when it’s not necessary.