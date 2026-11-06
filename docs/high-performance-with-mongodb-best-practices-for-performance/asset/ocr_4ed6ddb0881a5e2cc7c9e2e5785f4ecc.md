APM agents are typically integrated into your application code via language-specific libraries. When your application makes a database call, the APM agent does the following:

+   1. Intercepts the call
+   2. Measures its duration
+   3. Captures relevant context (such as the query itself, often normalized)
+   4. Correlates this with the broader request context

This integration enables APM platforms to do the following:

+   - Trace individual requests from the user’s browser or API client through multiple services down to specific database queries
+   - Identify slow database calls within the context of a complete user transaction
+   - Correlate application errors or latency spikes with specific database operations
+   - Provide aggregated views showing how database performance impacts overall application performance

Ultimately, APM platforms provide a powerful lens to understand how database interactions fit into the larger picture of application behavior, making it easier to pinpoint database-related bottlenecks that affect the end-user experience.

# OpenTelemetry

OpenTelemetry is an open source observability framework providing vendor-neutral APIs, libraries, and instrumentation to capture distributed traces, metrics, and logs. It represents the future of observability instrumentation, with growing adoption across the industry.

OpenTelemetry offers instrumentation libraries for popular programming languages and MongoDB drivers. By incorporating these libraries into your application, MongoDB driver calls are automatically captured as “spans” within distributed traces. Each span records the following:

+   - The duration of the database operation
+   - The operation type (query, update, insert, etc.)
+   - The target collection or database
+   - Any errors that occurred

These spans provide a detailed record of each database interaction, capturing what operation was performed, how long it took, where it was executed, and whether it succeeded. This granular data is crucial for understanding database performance and diagnosing issues.