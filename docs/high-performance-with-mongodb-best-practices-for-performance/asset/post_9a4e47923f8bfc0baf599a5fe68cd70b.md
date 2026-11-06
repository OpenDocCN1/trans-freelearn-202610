
// Setup the change stream with lookups and filtering
const priceMonitor = async (minimumPriceChange) => {
  try {
    const pipeline = [
      { $match: { operationType: 'update' } },
      { $match: { 'updateDescription.updatedFields.price': { $exists: true } } },
      { $project: { documentKey: 1, 'updateDescription.updatedFields.price': 1 } }
    ];
    options = { fullDocumentBeforeChange: 'required' };
    const stream = collection.watch(pipeline, options);

    // Process each change as it occurs
    for await (const change of stream) {
      const newPrice = change.updateDescription.updatedFields.price;
      const oldPrice = change.fullDocumentBeforeChange.price;
      const priceDiff = Math.abs(newPrice - oldPrice);

      // Only process significant price changes
      if (priceDiff >= minimumPriceChange) {
        await notifyPriceSubscribers(change.documentKey._id, oldPrice, newPrice);
      }
    }
  } catch (error) {
    console.error("Change stream error:", error);
    // Implement reconnection logic here
  }
};

This example demonstrates several key performance practices. By implementing server-side filtering through the aggregation pipeline, the application reduces network traffic between MongoDB and the client. Additionally, the application applies a business-specific filter (the minimumPriceChange threshold) on the client side. However, keep in mind that capturing the document state (preImage) before the update introduces some overhead.

This can have subtle performance impacts. In effect, enabling changeStreamPreAndPostImages causes write amplification because the copies need to be stored. However, they are likely to be in cache when accessed by the application. If you use 'updateLookup', you don't need changeStreamPreAndPostImages, so these extra writes don't happen. When the read occurs later, it is relatively less likely to hit cache. From a performance point of view, ideally you would use $match on fields in updatedFields. If that's not possible, experiment with both changeStreamPreAndPostImages and 'updateLookup'.

## Managing performance and durability

Building high-performance applications with change streams requires careful consideration of both resource usage and data durability. Let's explore strategies to optimize both.

### Resource optimization strategies

Change streams consume resources across your entire stack. In some cases, performing additional processing on the server can reduce the workload on your application. In other situations, extra processing on the client side can enhance overall system performance. Table 8.2 summarizes these trade-offs:

| Resource area | Impact factors | Optimization approaches |
|---|---|---|
| Server-side | - CPU usage for oplog processing
- Memory usage for cursor state
- Network bandwidth consumption | - Filter aggressively.
- Use the smallest appropriate scope.
- Multiplex several change streams into one and let the application filter to reduce the number of cursors consuming the oplog.
- Size the oplog to avoid streams falling too far behind the updates. Ultimately, the oplog is the buffer if the application change stream processing falls behind. |
| Application-side | - Connection pool usage
- Event processing capacity
- Memory used for buffering
- Network bandwidth usage | - Dedicate connections
- Use batch processing |

*Table 8.2: Trade-offs between server and application processing*

As with most performance decisions, there’s no one-size-fits-all solution. Optimizing change stream processing requires careful consideration of where resource usage is most critical in your architecture. By understanding the trade-offs between server-side and application-side processing, you can tailor your approach to balance efficiency, scalability, and responsiveness.

> **Best practice**
> 
> In high-volume deployments, consider setting up a dedicated service to process change streams. This approach consolidates multiple consumers into a single stream connection, improving efficiency and reducing resource usage.

### Handling high-volume event streams

In applications with extremely high write rates, change streams will produce a high volume of events. To help such applications scale effectively, consider the following tactics:

- Implement aggressive server-side filtering
- Deploy dedicated application infrastructure for change stream processing
- Use batching strategies that strike the right balance between throughput and latency

If your oplog is too small, the oplog rolls over and overwrites older entries before your application has had a chance to process their change stream events. This will result in a change stream error. This risk is especially high for applications where the change stream processing can go offline for extended periods.

To diagnose this, monitor the oplog.rs collection’s size and current oplog window. You can check this using the db.getReplicationInfo() command in the MongoDB Shell. If your oplog window is short (e.g., just a few hours), there’s a higher chance of falling off the oplog and incurring an error. To address this, increase the oplog size. In replica sets, this typically involves adjusting the oplogSizeMB parameter in the configuration or reinitializing the replica set with a larger oplog. As a rule of thumb, aim for an oplog that can retain several days, or even a week, of operations, depending on your recovery point objective (RPO).

If you suspect your change stream application is lagging behind actual database changes, this can result in delayed processing or stale data in downstream systems. To prevent this, consider the following methods:

- Monitor network I/O on both the MongoDB primary node and your change stream service. Look for spikes in network utilization that correspond to periods of high database activity.
- Consider the physical proximity of your application to the database. Ensure optimal network connectivity between your MongoDB cluster and your change stream application. This might involve co-locating them in the same data center or cloud region.
- If possible, provision more network bandwidth for both the database server and the consumer application.
- Consider processing events in batches rather than individually.

Your application also needs enough compute resources to process events effectively. If it’s resource-constrained and it can’t keep up with the volume of changes, it will fall further and further behind before eventually falling off the oplog and receiving an error. Similarly, the MongoDB server must have adequate resources to generate and send change events. Monitor CPU, memory, and disk I/O on the system running your application. Look for high CPU usage, memory pressure, or I/O bottlenecks. Try to minimize expensive operations inside the change stream processing loop. On the database side, high CPU or I/O usage can indicate write bottlenecks. For example, heavy indexing can slow down writes, which in turn affects the speed at which the oplog is written, and consequently, change stream performance.

If your change stream application frequently receives invalidate events, forcing it to close and reopen, this can be disruptive and lead to increased latency or missed events if not handled correctly. An invalidate event signifies that the collection or database being watched by the change stream no longer exists or has undergone a structural change (such as dropping or renaming) that prevents the stream from continuing.

To troubleshoot, count the types of events your change stream receives. If you see invalidate events, identify the specific operation that triggered them. If possible, avoid dropping or renaming collections or databases that are actively being watched by change streams, especially in production environments, where continuous processing is critical. If these operations are necessary, ensure your change stream application is designed to gracefully handle invalidate events by re-establishing the stream.
