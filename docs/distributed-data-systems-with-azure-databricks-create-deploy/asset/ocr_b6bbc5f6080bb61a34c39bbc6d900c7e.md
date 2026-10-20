After changing the file detection mode, we can start a new stream by writing it back again to a different checkpoint path, like this:

```
df.writeStream.format("delta") \
.option("checkpointLocation", <checkpoint-path>).
start(<output-path>)
```

Next, we will see how we can use Apache Kafka in Structured Streaming as one of its sources of data.

# Apache Kafka

Pub/sub messaging systems are used to provide asynchronous service-to-service communication. They are used in serverless and microservices architectures to construct event-driven architectures. In these systems, messages published are immediately received by all subscribers to the topic.

Apache Kafka is a distributed pub/sub messaging system that consumes live data streams and makes them available to downstream stakeholders in a parallel and fault-tolerant fashion. It's very useful when constructing reliable live-streaming data pipelines that need to work with data across different processing systems.

Data in Apache Kafka is organized into topics that are in turn split into partitions. Each partition is an ordered sequence of records that resembles a commit log. As new data arrives at each partition in Apache Kafka, each record is assigned a sequential ID number called an **offset**. The data in these topics is retained for a certain amount of time (called a **retention period**), and it's a configurable parameter.

The seemingly infinite nature of the system means that we need to decide the point in time from where we want to start read our data. For this, we have the following three choices:

+   - **Earliest**: We will start reading our data at the beginning of the stream, excluding data that is older than the retention period deleted from Kafka.
+   - **Latest**: We will process only new data that arrives after the query has started.
+   - **Per-partition assignment**: With this, we specify the precise offset to start from for every partition, to control the exact point in time from where the processing should start. This is useful when we want to pick up exactly where a process failed.