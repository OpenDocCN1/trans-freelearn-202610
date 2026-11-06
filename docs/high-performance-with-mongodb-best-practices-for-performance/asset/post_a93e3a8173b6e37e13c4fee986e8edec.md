
## Check SSD health

SSDs use sophisticated firmware techniques such as wear leveling, garbage collection, over-provisioning, and TRIM to reduce the performance impact of erase-before-write. At the operating system level, make sure TRIM and garbage collection are enabled. These features make modern SSDs fast and reliable across a wide range of use cases. However, it’s important to monitor their health to detect when wear is starting to impact performance or when the drive is nearing the end of its lifespan.

On Linux, most SSDs support SMART data reporting, which provides detailed health and diagnostic metrics such as wear leveling status, remaining life, and bad block count. Many SSD vendors offer proprietary tools to monitor their drives. These tools often expose wear-related data and let you set alerts for health issues or performance degradation. If you’re using enterprise-grade SSDs in a server or data center, additional monitoring tools may be available, such as Nagios, Datadog, Dynatrace, Zabbix, or Prometheus with Node Exporter.

SSDs also log system-level warnings or errors when health or performance begins to degrade. On Linux, check /var/log/syslog, use journalctl, or run dmesg to look for I/O-related errors tied to your SSDs.

## Avoid double encryption and compression

Some storage technologies include support for compression or encryption, and some support both. For example, enterprise storage arrays and SSDs often include both. Cloud storage services typically don’t support compression but do support encryption to protect data at rest, in transit, and within snapshots.

Compressed or encrypted data is harder to compress. If the database compresses and/or encrypts the data, the storage device’s compression logic may find fewer redundant patterns, resulting in little to no further compression. In some cases, attempting compression on already compressed data may cause storage overhead, increasing the required compute resources.

Data processed by both the database and the storage device could be compressed and encrypted twice. This is often inefficient and may impact performance while offering no significant benefits. To prevent this, you should determine where compression and encryption are most effectively applied, either at the database layer or at the storage layer.

In some industries, double encryption may provide an extra layer of defense by ensuring encryption is enforced independently at multiple layers (e.g., database encryption for logical security and storage device encryption for physical security).

## Ensure resident memory usage stays below 80%

As a general rule, you should try to keep resident memory usage below 80% because low memory can have a big impact on performance. For example, code could be paged out (evicted from physical RAM) because it can be reloaded from its original source. However, if it needs to run again, it will need to be paged back in and wait for I/O. If swap is configured, the system moves less frequently used memory pages to disk to free up RAM for active processes. This causes a write and subsequent read from storage when the memory is accessed again. If memory gets critically low, the **out-of-memory (OOM)** “killer” could terminate the server process.

If you are self-managing a MongoDB system and can’t add memory, you should regularly monitor the memory usage of your cluster for insights on whether adjustments are needed. You could experiment with reducing the size of WiredTiger’s cache, as we discussed in *Chapter 7, Storage Engines*.

In Atlas, you can set up fine-grained alerts to notify you when resident memory approaches critical thresholds. You can remediate this by scaling to a higher tier or enabling auto-scaling, which is described in more detail in the *Using autoscaling for Atlas performance* section later in this chapter.

In both Atlas and self-managed deployments, if a large amount of memory is being used by complex aggregations running analytical workloads, you could use MongoDB’s features for workload isolation.

Workload isolation minimizes contention for memory and ensures each workload runs efficiently. When creating an Atlas cluster, you have options to create analytics nodes or search nodes for workload isolation.

Use analytics nodes to isolate queries that you do not wish to contend with your operational workload. For example, analytics nodes can handle data analysis operations, such as reporting queries from BI Connector for Atlas. The cluster tier for your analytics nodes can be different from the operational nodes. If you select an analytics node on a significantly lower tier, you need to monitor replication lag to make sure it’s not under-resourced.

Similarly, you can use dedicated search nodes to isolate search workloads from your operational workload. Finally, if you are not using sharding yet, this could be a good reason to start. See *Chapter 6, Sharding*, for more details on when and how to add sharding.

## Networking best practices

As with all distributed systems, following networking best practices is essential for stable and high-performance MongoDB deployments. If you’ve determined that the network is the current limiting factor, the following checklist may help:

- Deploy MongoDB clusters close to your application servers (e.g., in the same region if using cloud infrastructure).
- Utilize low-latency network connections with high-speed interconnects.
- Use network compression and monitor/optimize query patterns to reduce data transfer. This is a feature that MongoDB drivers support but is not enabled by default.
- Allocate sufficient bandwidth for MongoDB servers, especially in high-traffic production environments.
- Use network interfaces optimized for higher throughput (e.g., gigabit or multi-gigabit Ethernet).
- Rely on a stable, well-maintained network infrastructure to minimize packet loss.
- Prioritize MongoDB traffic using quality-of-service (QoS) settings.
- Set short and consistent time-to-live (TTL) values for DNS entries to prevent stale cache issues.
- Deploy MongoDB clusters with fixed IPs or DNS names that resolve quickly; use local DNS servers for faster resolution.
- Isolate MongoDB traffic from non-critical workloads using VLANs or QoS.
- Scale network infrastructure or migrate MongoDB to a less congested environment if needed.
- Avoid deep packet inspection of MongoDB traffic where possible.
- Use virtual private clouds (VPCs), trusted networks, and VPNs to secure traffic without overloading packet filtering systems.
- Implement consistent routing policies across all nodes.
- Test network topology regularly to ensure balanced and efficient traffic distribution.
- Deploy MongoDB clusters within the same geographic region when possible.
- Leverage MongoDB Atlas’s global clusters for distributed workloads while minimizing latency.
- Choose cloud providers with optimized regional interconnects.

Additionally, consider tuning the following operating system-level networking parameters:

- **somaxconn**: In Linux, this parameter defines the maximum number of pending connection requests that can be queued on a socket. Increasing the somaxconn value allows MongoDB to queue more incoming connections, reducing the likelihood of connection errors during periods of heavy network traffic.
- **tcp_max_syn_backlog**: Controls how many pending SYN requests are queued during the initial TCP handshake.
- **tcp_syncookies**: Prevents SYN flood attacks when the SYN backlog is full.
- **tcp_fin_timeout**: This parameter controls the amount of time (in seconds) a socket remains in the TIME_WAIT state after the connection has been closed. The TIME_WAIT state is a safety mechanism ensuring that all packets related to the closed connection have been properly transmitted and acknowledged before the socket is released for reuse. While this is critical for preventing packet retransmission errors, an unnecessarily high tcp_fin_timeout value can cause resource exhaustion in busy environments, as the system holds onto a large number of closed connection states for an extended period.

Although networking bottlenecks are typically less common than those related to CPU, memory, or storage, it’s still wise to monitor network health actively and address issues proactively with the right tools and configuration.

## Using auto-scaling for Atlas performance

MongoDB Atlas offers auto-scaling, a powerful feature designed to streamline resource management and ensure optimal performance for your cluster workloads. By dynamically adjusting both cluster tier and storage capacity, auto-scaling helps you maintain high availability and performance while minimizing manual intervention. It can scale compute up or down; storage is auto-scaled up only. Users benefit from Atlas’s simplicity and efficiency. If your particular needs aren’t addressed, consult MongoDB support for guidance tailored to your specific application.

When workloads change and data grows, scaling resources manually can be challenging and inefficient. Automatic scaling addresses these challenges by leveraging automation, ensuring that your cluster resources align with your application’s demands in real time. This eliminates over-provisioning or underutilization, resulting in cost efficiency and consistent performance.
