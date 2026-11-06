# Use case: Diagnosing insufficient IOPs in self-managed MongoDB

On self-managed systems, it’s common to see a similar pattern across customer deployments: workloads naturally grow over time, often outpacing the capabilities of the underlying infrastructure. With MongoDB Atlas, this is easy to catch early using built-in monitoring tools that provide real-time visibility into key metrics such as idle CPU, disk IOPs headroom, and the number of operations being executed. However, when managing MongoDB clusters independently, teams must rely on scripts or external monitoring tools to track these metrics.

In this example, we revisit a case from a customer who was self-managing MongoDB on their cloud infrastructure. As performance issues began to surface, the MongoDB support team turned to the server’s diagnostic data.

# Diagnosing the cause

MongoDB servers capture system information and internal counters every second in **Full-Time Diagnostic Data Capture (FTDC)**. This data doesn’t contain any sensitive information such as queries, query predicates, results, or credentials.

Analyzing the storage read requests per second (which on Linux comes from the iostat command), they found the database server was issuing 16,600 IOPs. FTDC data from vmstat showed 39% iowait, indicating the system was spending significant time waiting for I/O operations.

The customer had provisioned the underlying storage with 15,000 IOPs. Cloud providers typically allow bursting beyond the provisioned limit for a period before throttling. This bursting, followed by throttling, was experienced as stalls by the customer’s application.

Looking at the IOPS allocation, we realized we were using more IOPS than provisioned. Performance was acceptable for a time because higher burst IOPS were provided. However, after the burst allowance was exhausted, a sharp performance degradation followed. The system no longer had enough IOPS, and operations slowed, waiting for I/O.

# Implementing the solution

The customer increased the provisioned IOPS from 15,000 to 18,000, which immediately resolved the performance stalls.