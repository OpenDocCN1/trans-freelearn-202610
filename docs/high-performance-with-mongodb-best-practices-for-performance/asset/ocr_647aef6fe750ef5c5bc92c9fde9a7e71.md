+   - **Removed redundant indexes:** While reviewing Performance Advisor recommendations, the team identified 9 indexes on fields that were prefixes of another compound index that could potentially be dropped. However, dropping indexes is an operation that requires careful consideration and planning. The team first validated each recommendation in a test environment, assessed potential query impact, and implemented the changes gradually with close monitoring. Only after thorough evaluation did they proceed with removing these redundant indexes by initially hiding the indexes, then confirming no ill effects on the system, and dropping them. This ultimately improved the write performance without impacting the read performance.

These actions collectively reduced CPU usage, improved query efficiency, and stabilized system performance. More importantly, they laid the groundwork for ongoing optimization and proactive monitoring going forward.

# Results and learnings

This case illustrates several important principles about diagnosing inherently slow operations:

+   - Performance issues often coincide with application changes or new features
+   - Using multiple diagnostic tools such as Performance Advisor, Query Profiler, and logs provides a comprehensive view
+   - Creating appropriate indexes is often the most effective solution for collection scans
+   - Removing redundant indexes can improve the write performance
+   - Setting up alerts for key metrics enables proactive identification of future issues

Ultimately, this risk was first identified by monitoring key metrics over time. This helped us identify gradual performance degradation and act before there was an impact on customers. This performance tuning exercise allowed us to reduce cluster tiers and reduce costs.

# Case study: Unexpected admin query causing collection scan

Slow queries are often the first indicator of performance issues in MongoDB deployments. A major issue hit a customer’s self-managed MongoDB deployments. Their application monitoring showed a sudden increase in the latency of database operations, impacting users across the entire system. The suddenness of the problem meant we needed to investigate and fix it right away to avoid further disruption and get services back to normal.