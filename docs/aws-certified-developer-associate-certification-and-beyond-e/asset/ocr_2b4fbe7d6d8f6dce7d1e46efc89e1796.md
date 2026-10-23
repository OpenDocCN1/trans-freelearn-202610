![](bbox=[0.12, 0.12, 0.88, 0.36])

Figure 15.21 – Amazon Kinesis architecture

Kinesis Data Streams can store data for 24 hours by default, which is also the minimum retention period. This can be increased to a maximum of 365 days using the `IncreaseStreamRetentionPeriod` operation.

The next section will teach you more about Kinesis Data Streams scalability.

# Scaling Kinesis Data Streams

Kinesis scales by deploying one or more shards. As more data is ingested, additional shards are added. To effectively use Kinesis Data Streams, you must design your data ingestion architecture to handle the volume, velocity, and variety of your data sources. Amazon Kinesis Data Streams uses shards as the base throughput unit. Each shard provides a capacity of 1 MB/sec and 1,000 records per second for writes and 2 MB/sec for reads, as depicted in Figure 15.21. In addition, data is stored as records, a unit comprising a sequence number, partition key, and the data blob. The maximum size of this data blob is 1 MB. Data is added to the stream using the `PutRecord` and `PutRecords` operations, the **Kinesis Production Library (KPL)**, or the Kinesis Agent.

When reading data from the Kinesis stream, consumers can choose between a shared fanout type and an enhanced fanout type. In the shared fanout type, all consumers share a shard 2 MB/sec read throughput, whereas with the enhanced fanout type, each consumer gets its own 2 MB/sec read throughput.

Calculating the appropriate number of shards based on your data volume is crucial. This involves understanding your application’s data generation rate per second and dividing this rate by the data throughput per shard. Proper shard management ensures efficient data processing without incurring unnecessary costs.