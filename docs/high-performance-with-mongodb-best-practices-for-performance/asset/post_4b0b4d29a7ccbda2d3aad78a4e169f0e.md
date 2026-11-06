
### Query Pattern Analysis
Using grep commands, they analyzed the log file patterns:

```
grep 'aCollection' mongodb.log* | grep -v 'getMore' | wc -l
122784

grep 'aCollection' mongodb.log* | grep '"cursorExhausted":true' | wc -l
46954
```

The first command counts all operations on 'aCollection' while excluding 'getMore' operations, revealing 122,784 initial cursor creations. The second command counts only cursors that were properly exhausted (fully iterated through or explicitly closed), showing just 46,954 such cases. This significant discrepancy (only about 38% of cursors being properly closed) confirmed our suspicion of a cursor leak, as the majority of cursors created by the application were being abandoned without proper cleanup, causing them to accumulate until their timeout period elapsed.

## Root cause identification

The problem originated from a specific bug in the customer’s application that had remained hidden until recently. When queries began returning more than the default 101 documents in a single batch due to data growth, the issue finally became apparent. The application was designed to process small result sets and would properly handle cursors in those cases.

This issue only manifests when a client keeps an active session open while not fully exhausting the cursor (either by not iterating through all results or not explicitly calling close() on the cursor). Under normal circumstances, with properly implemented cursor handling or smaller result sets, this problem wouldn’t occur. Most MongoDB driver implementations and standard query patterns automatically handle cursor lifecycle management, making this type of leak relatively uncommon in well-designed applications.

For more information on how cursors behave, visit the MongoDB Documentation on cursors at https://www.mongodb.com/docs/manual/core/cursors/#behavior.

## Implementing the solution

While the customer’s development team worked on a permanent fix, the MongoDB support team provided a key temporary measure: a shell command to terminate idle cursors on the affected namespace. This helped prevent a build-up of inactive cursors, which could have worsened performance and caused system instability. This command offered immediate relief and gave the development team valuable time to fix the core issue.

The customer developed a complete and proper fix for the bug. The fix was deployed collaboratively with MongoDB support on an active call, ensuring real-time guidance and immediate troubleshooting.

The deployment had an immediate and significant impact. Before the fix, open cursors would routinely spike to over 600,000 before temporary drops. For this particular application, normal operations should have required only around 100 concurrent cursors at any given time. After deployment, the system stabilized. The maximum number of open cursors consistently stayed at a much lower and manageable level of 100. This reduction greatly improved system efficiency and resource utilization.

Further confirming the success of the deployed fix, server status reports showed the issue was resolved. Specifically, the `cursor.lifespan.greaterThanOrEqual10Minutes` metric consistently reported 0. This metric is useful for identifying long-lived, potentially problematic cursors. A value of 0 showed that no cursors were staying open for 10 minutes.

## Results and learnings

The customer’s situation represented an unusual edge case where their application code was repeatedly creating new cursors without properly closing them.

This example applied a number of troubleshooting principles:
- Gradual performance degradation can be caused by cursor leaks
- Diagnosing the root cause often requires multiple approaches
- While a permanent solution is being developed, temporary mitigation might be necessary
- Monitoring metrics enables early detection

Finally, this situation provides a vivid example of how application code issues from the development phase can appear as database performance problems in production.

## Use case: Burst of poorly optimized queries

A customer noticed a sudden burst of timed-out operations. Atlas monitoring showed a spike in normalized CPU utilization to 100% and over 3.7 GB/s of data being read into the storage engine cache, as demonstrated in Figure 15.5:

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_371_0.png)

*Figure 15.5: Cache Activity and user CPU utilization*

Application queries were severely impacted. Users experienced timeouts, slow responses, and in some cases complete request failures. The system was effectively blocked as normal operations couldn’t proceed while these resource-intensive processes monopolized the available CPU and I/O capacity. But what caused this sudden, crippling load? Since the database primarily serves customer queries, these performance metrics strongly suggested that one or more problematic queries were likely behind the spike. To identify the specific culprits behind this unusual activity, we needed to look deeper into the query patterns. This led us to examine Atlas Query Insights for a more detailed understanding of what was happening at the query level.

## Diagnosing the cause

The Query Insights/Query Profiler revealed a significant number of queries on a collection examining over 1 million documents each, shown in Figure 15.6, represented as green dots at the top of the graph. At the bottom of the screenshot, we can also see that, in total, 233 queries scanned approximately 144 million documents during this 16-minute period. We looked at the previous 20 minutes and saw only 8 million documents in total were scanned.

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_372_0.png)

Figure 15.6: Query Profiler showing a large number of documents being examined

Clicking on one of the green dots displayed in the Query Profiler, as shown in Figure 15.6, revealed deeper insights into each individual query. When clicking on one of those queries, the profiler showed the following:

```
"replanReason": "cached plan was less efficient than expected: expected trial execution to take 2 works but it took at least 20 works"
```

This indicated the query planner was struggling to find an efficient execution plan. Upon reviewing the collection, they found 25 indexes that were eligible to be used, but none were a good match for these queries, indicating that a new index might be necessary.

In addition, further log analysis also revealed that some of the slow queries in this collection were trying to match documents over a six-month range.

## Implementing the solution

To address the inefficiencies caused by the lack of an optimal index, the team reviewed the Performance Advisor, which, as expected, provided a recommendation for a new compound index tailored to resolve the issue, as shown in Figure 15.7. Acting on this advice, the team promptly created the compound index, significantly improving query performance.

### aDB.aCollection

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_373_0.png)

Figure 15.7: Performance Advisor recommendations

Additionally, the team enhanced the application by modifying its query logic to match documents within a shorter time span. This adjustment further optimized the query execution, reducing both the size of data returned and query latency, resulting in faster performance and more efficient use of system resources.

## Results and learnings

This case underscores several key principles of query optimization. Sudden performance issues often correlate with changes in application behavior, making it essential to monitor application patterns closely. One critical indicator of inefficient queries is when the database examines a disproportionately large number of documents compared to the actual results returned. This can signal an underlying issue with query design or indexing. Additionally, the Query Planner may encounter difficulties when operating over extensive data ranges, highlighting the importance of narrowing the scope of queries whenever possible.

Addressing performance challenges often requires a combination of strategies, including both database changes, such as creating optimized indexes, and application-level adjustments, such as reducing the time span of queried data. Tools such as the Performance Advisor are invaluable in identifying missing indexes, even for infrequent query patterns. It provides actionable recommendations to enhance efficiency and resolve bottlenecks.

## Addressing hardware resource constraints

Hardware resource constraints represent a critical challenge in maintaining optimal MongoDB performance. Even with well-optimized queries and thoughtfully designed schemas, the underlying hardware ultimately dictates how efficiently your database can handle workloads. Resource constraints arise when the computational, memory, network, or storage capacity of your deployment is insufficient to support the demands placed on it, leading to cascading performance issues. Imagine a production line where machines are running at their maximum capacity. Any increase in demand or inefficiency pushes systems past their limits, resulting in bottlenecks and delays. Similarly, resource constraints in MongoDB can significantly impair your application’s responsiveness and scalability.

These constraints typically surface in four primary areas: CPU, memory, disk I/O, and network. High CPU utilization may limit the processing power available for queries, aggregations, and background tasks, forcing operations to queue and delaying results. Memory constraints occur when the available RAM is insufficient to store the working set, leading to increased reliance on slower disk reads and degraded performance. Disk I/O constraints, such as a slow or overloaded storage subsystem, can drastically impede operations that require frequent or large data reads and writes. Network constraints can significantly impact distributed database operations, causing issues such as increased replication lag, slower initial syncs, extended backup times, and delayed elections in replica sets, all of which can compromise data availability and consistency across your deployment.
