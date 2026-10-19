# Azure Event Hubs

**Azure Event Hubs** is a data streaming platform used for ingesting event data. The source of the event data that's collected is usually application logs, clickstreams, and IoT. Azure Event Hubs can be deployed with minimal configuration.

You can use Azure Event Hubs for both batch and real-time processing. For batch processing, data from Event Hubs can be captured in Azure Blob storage or Azure Data Lake Storage so that it can be processed later. For real-time processing, you may have to send data to Azure Functions, which could take further action based on events:

![](bbox=[0.15, 0.32, 0.82, 0.58])

Figure 3.4 – Multi-tenancy and single-tenancy in Azure Event Hubs

Data in Azure Event Hubs is not stored permanently. By default, the queue is 1 day, but this can be increased to 7 days in the standard edition and 90 days in the dedicated edition.

Event data is stored in partitions – the higher the number of partitions, the higher the read and write throughput. The capacity of Azure Event Hubs is pre-purchased using throughput limits once the throughput limit equates to a write process (1 MB/1,000 events) or a read process (2 MB/4,096 events). The auto-inflate feature can be used to automatically scale your throughput units based on peak demands.

Azure Event Hubs replicates data within the primary data center with **automated failure detection** and recovery enabled. For extra protection, replication across availability zones can also be enabled.