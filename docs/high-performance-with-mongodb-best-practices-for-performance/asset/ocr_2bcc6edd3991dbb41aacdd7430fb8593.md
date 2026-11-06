+   - **A framework bottleneck:** The application software may send seemingly simple requests to the framework, but under the hood these requests can trigger expensive database operations, such as a full collection or table scan on the database. This inefficient querying uses CPU and storage bandwidth on the database while also sending more data than necessary back to the application servers. As a result, unnecessary network bandwidth is used, and additional CPU resources are required on the application server to process the data.

This latter bottleneck could be identified in a number of ways:

+   - Profiling the application could find that the requests to the framework are taking a surprising amount of time.
+   - During development, an end-to-end trace can be used to track a user request from a client through the application code, framework, and data services abstraction to see what kind of query is run on the data services and the subsequent **input/output (I/O)** operations.
+   - This could identify read or write amplifications, where a small client request results in a disproportionately large database operation. For example, a client query for one data point of 32 bytes could make its way through the full system and result in a table scan on the database that pulls 32 GB of data through I/O and caches for no reason. This could be remedied by configuring the framework to be more efficient.

We will provide specific examples of these kinds of bottlenecks and solutions throughout the book, culminating in a number of specific case studies in *Chapter 15, Debugging Performance Issues.*

Identifying potential bottlenecks is only the first step in optimizing a software system. To remove these bottlenecks, we need a structured approach applying performance engineering principles.

Performance engineering principles span a range from algorithmic theory to Little’s law. By applying these principles, developers can minimize bottlenecks, improve scalability, and maximize resource efficiency. In the next subsections, we’ll explore some of these principles and see how they guide the design of high-performing systems, which you can keep in mind as you read this book.

# Algorithmic efficiency (complexity)

The choice of algorithm and data structure is typically where most computer science courses first introduce performance. An algorithm that scales linearly or logarithmically with input size will perform better than one that scales exponentially. No amount of micro-optimization will compensate for an inherently slow algorithm. In modern systems, this might mean using built-in libraries (often optimized in C or assembly) or leveraging parallel algorithms.