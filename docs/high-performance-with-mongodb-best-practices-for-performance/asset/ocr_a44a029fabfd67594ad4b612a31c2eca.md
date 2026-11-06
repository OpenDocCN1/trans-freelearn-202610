As we saw earlier, with Atlas, you can select different types and configurations of cloud storage. You typically have the same options when running your own cloud instances or self-managed storage. In addition, you can choose different filesystems and settings, such as readahead. When a program reads a file, Linux anticipates future reads and loads additional data into memory. This reduces the number of disk access operations, making sequential reads faster. This will be discussed in more detail in Chapter 13, Operating System and System Resources.

# Managing complexity in modern data platforms

Let’s zoom back out from a single mongod process to look at the data services layer again. To your application, the data services element is one component, but in reality, it can be a complex distributed system in its own right with multiple individual servers.

MongoDB’s approach is to provide a data services API that simplifies this complexity using three key principles: a flexible data model, built-in redundancy and resilience, and horizontal scaling.

# Flexible data model with rigorous capabilities

The document model allows you to represent relationships naturally while still supporting rigorous data governance when needed. One MongoDB user explained it perfectly: “We can push updates to our application without having to coordinate complex database migrations. The schema evolves with our code, not against it.” You can learn more about this flexible data model in Chapter 2, Schema Design for Performance.

# Built-in redundancy and resilience

Rather than treating high availability as an add-on feature, MongoDB bakes resilience into its core architecture through replication:

+   - **Automatic failover:** If the primary node fails, an election determines a new primary within seconds
+   - **Self-healing:** When failed nodes recover, they automatically catch up and rejoin the replica set

Again, Atlas provides defaults, implements best practices, and simplifies the number of settings you need to consider. When self-managing MongoDB, you can dig deeper into replication settings. For example, if running MongoDB on your own servers, you can change server parameters to tune how replication data is batched. Larger batches reduce network traffic and the number of IOPS required on secondaries, but can add latency for writes. You can also configure **flow control**, which can prevent replication lag by slowing down writes if secondaries are falling behind. You can learn more about replication in Chapter 5, Replication.