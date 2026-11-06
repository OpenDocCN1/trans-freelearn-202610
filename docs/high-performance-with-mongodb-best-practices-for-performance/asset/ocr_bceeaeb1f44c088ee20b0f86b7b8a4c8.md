Addressing disk I/O bottlenecks may involve optimizing queries to reduce disk access, improving indexing, increasing provisioned IOPS/throughput, upgrading storage hardware, or ensuring the working set fits in RAM to minimize disk reads.

# Cache pressure (WiredTiger cache utilization)

MongoDB’s WiredTiger storage engine uses an internal cache to hold frequently accessed data and indexes in memory, minimizing the need for slower disk access. Cache pressure occurs when the active working set (the data and indexes required by the current workload) exceeds the size of the configured WiredTiger cache. This leads to increased cache eviction activity and page faults, degrading performance.

Several key metrics can help you detect and diagnose WiredTiger cache pressure in MongoDB. Monitoring these indicators gives you early warning signs of performance bottlenecks caused by memory constraints:

+   - **Cache utilization:** Track the amount of the WiredTiger cache currently in use (serverStatus().wiredTiger.cache["bytes currently in the cache"]). While high utilization is generally good, consistently hitting the maximum limit indicates pressure.
+   - **Cache eviction rate:** Monitor the rate at which pages are being evicted from the cache to make room for new data (serverStatus().wiredTiger.cache["unmodified pages evicted"]). A high or rapidly increasing eviction rate suggests the cache is churning frequently. Atlas often visualizes cache activity, highlighting evictions.
+   - **Pages read into cache:** Track the rate at which data is being read from disk into the cache (serverStatus().wiredTiger.cache["pages read into cache"]). High rates often correlate with high eviction rates and indicate that the required data is frequently not found in memory.
+   - **Page faults:** While not solely tied to the WiredTiger cache (OS page faults can also occur), frequent page faults ( serverStatus().extra_info.page_faults or OS metrics) often accompany cache pressure, indicating that MongoDB processes are requesting data not currently in physical RAM.

Monitoring these metrics provides a clear picture of cache health. High utilization isn’t inherently bad, but when combined with high eviction rates, increased page reads from disk, and page faults, it signals that the cache is struggling to keep up with the workload, forcing frequent reads from slower disk storage.