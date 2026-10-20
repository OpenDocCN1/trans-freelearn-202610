Or, we could use the table method, as follows:

```
data_events.writeStream.format("delta")
    .outputMode("append")
    .option("checkpointLocation", "/delta/data_events/_checkpoints/etl-from-json")
    .table("data_events")
```

We can use **Complete** mode in order to reprocess all the information available on a table. For example, if we want to aggregate the count of users by user_id value every time we update the table, we can set the mode to complete and the entire table will be reprocessed in every batch, as illustrated in the following code snippet:

```
spark.readStream.format("delta")
    .load("/mnt/delta/data_events")
    .groupBy("user_id").count()
    .writeStream.format("delta")
    .outputMode("complete")
    .option("checkpointLocation", "/mnt/delta/messages_by_user/_checkpoints/streaming-agg")
    .start("/mnt/delta/messages_by_user")
```

This behavior can be better controlled using one-time triggers that allow us to have fine-grained control on what the actions are that should trigger the table update. We can use them in applications such as aggregation tables or with data that needs to be processed on a daily basis.

# Azure Event Hubs

Azure Event Hubs is a telemetry service that collects, transforms, and stores events. We can use it to ingest data from different telemetry sources and to trigger processes in the cloud.