Partition keys play a vital role in data distribution across shards. When a data record is put into the stream, a partition key associated with that record determines which shard receives the record. The partition key should be chosen such that the data is evenly distributed across shards, avoiding “hot spots” where too many data records are directed to a single shard, potentially throttling the processing capability of your application.

In the next section, you will learn more about the capacity mode options for your data stream.

# Kinesis Data Stream Capacity Modes

With Kinesis Data Streams, you can choose between two capacity modes – **on-demand** and **provisioned.**

The **provisioned** capacity mode requires you to specify the number of shards for the data stream. You must increase the number of shards within the data stream if you need more capacity. Each shard gives you 1 MB/sec or 1,000 records per second for data in and 2 MB/sec for data out throughput.

You can scale out capacity by splitting shards using the `SplitShard` API, thereby creating additional shards, and if you need to scale down, you can merge two shards using the `MergeShard` API.

The **on-demand** capacity mode provides 4 MB/sec and 4,000 records per second for writes and can scale up to 200 MB/sec and 200,000 records per second. It doubles the capacity based on observed throughput over the last 30 days. So, for example, if your write steam capacity ranges between 20 MB/sec and 40 MB/sec, then Kinesis Data Streams will ensure that you can burst to double the capacity to 80 MB/sec.

Note, however, that you may encounter the `ProvisionedThroughputExceeded` error if your traffic increases more than double the previous peak within 15 minutes. You must then throttle your stream requests, which involves reducing the frequency or the size of the requests. You can also attempt retries with exponential backoff.

In the next section, you will learn how you can develop custom applications to consume and process data from Kinesis Data Streams using the **Kinesis Client Library (KCL)**.

# Kinesis Client Library

The KCL enables you to consume and process data from a Kinesis data stream for your custom applications. The KCL client, which is a Java-based library, needs to be configured on your instances. It handles backend processing and tasks associated with distributed computing, such as load balancing, managing consumer instance failures, pulling data from the stream, checkpoint record processing, and balancing shards to worker associates, which are called leases if the worker instance count changes or if the data stream is reshared either via a shard split or shard merge.