The startingOffsets option accepts just one of these options and is only used on queries that start from a new checkpoint. A query that is restarted from an existing checkpoint will always resume where it was left off, except when the data at that offset is older than the retention period.

One advantage when using Apache Kafka in Structured Streaming is that we can manage what to do when the stream first starts and what to do if the query is not able to pick up from where it left off because the data is older than the retention period, using the startingOffsets and failOnDataLoss options, respectively. This is set with the auto.offset.reset configuration option.

In Structured Streaming, you can express complex transformations such as one-time aggregation and output the results to a variety of systems. As we have seen, using Structured Streaming with Apache Kafka allows you to transform augmented streams of data read from Apache Kafka using the same APIs as when working with batch data and integrate data read from Kafka, along with data stored in other systems including such as S3 or Azure Blob storage.

In order to work with Apache Kafka, it is recommended that you store your certificates in Azure Blob storage or Azure Data Lake Storage Gen2, to be later accessed using a mount point. Once your paths are mounted and you have already stored your secrets, you can connect to Apache Kafka by running the following code:

```
streaming_kafka_df = spark.readStream.format("kafka")
    .option("kafka.bootstrap.servers", ...)
    .option("kafka.ssl.truststore.location",<dbfs-truststore-location>)
    .option("kafka.ssl.keystore.location", <dbfs-keystore-location>)
    .option("kafka.ssl.keystore.password", dbutils.secrets.get(scope=<certificate-scope-name>,key=<keystore-password-key-name>))
    .option("kafka.ssl.truststore.password", dbutils.secrets.get(scope=<certificate-scope-name>,key=<truststore-password-key-name>))
```

To write into Apache Kafka, we can use writeStream on any PySpark dataframe that contains a column named value and, optionally, a column named key. If a key column is not specified, then a null-valued key column will be automatically added. Bear in mind that a null-valued key column may lead to uneven partitions of data in Kafka, so you should be aware of this behavior.