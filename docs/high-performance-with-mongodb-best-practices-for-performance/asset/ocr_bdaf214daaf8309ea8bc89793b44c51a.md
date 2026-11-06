As with most performance decisions, there’s no one-size-fits-all solution. Optimizing change stream processing requires careful consideration of where resource usage is most critical in your architecture. By understanding the trade-offs between server-side and application-side processing, you can tailor your approach to balance efficiency, scalability, and responsiveness.

> **Best practice**
> 
> In high-volume deployments, consider setting up a dedicated service to process change streams. This approach consolidates multiple consumers into a single stream connection, improving efficiency and reducing resource usage.

# Handling high-volume event streams

In applications with extremely high write rates, change streams will produce a high volume of events. To help such applications scale effectively, consider the following tactics:

+   - Implement aggressive server-side filtering
+   - Deploy dedicated application infrastructure for change stream processing
+   - Use batching strategies that strike the right balance between throughput and latency

If your oplog is too small, the oplog rolls over and overwrites older entries before your application has had a chance to process their change stream events. This will result in a change stream error. This risk is especially high for applications where the change stream processing can go offline for extended periods.

To diagnose this, monitor the oplog.rs collection’s size and current oplog window. You can check this using the db.getReplicationInfo() command in the MongoDB Shell. If your oplog window is short (e.g., just a few hours), there’s a higher chance of falling off the oplog and incurring an error. To address this, increase the oplog size. In replica sets, this typically involves adjusting the oplogSizeMB parameter in the configuration or reinitializing the replica set with a larger oplog. As a rule of thumb, aim for an oplog that can retain several days, or even a week, of operations, depending on your recovery point objective (RPO).