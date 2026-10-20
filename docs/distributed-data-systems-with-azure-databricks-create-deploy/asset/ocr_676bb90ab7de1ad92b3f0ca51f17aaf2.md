We can choose between these two options but not use both at the same time. Also, the changes made by setting this option will only take place once a new stream has been started. If the stream was already running and a checkpoint was written, these options would be ignored:

+   For example, suppose you have a user_messages table. If you want to read changes since version 10, you can use the following code:

```
data_events.readStream
  .format("delta").option("startingVersion", "10")
  .load("/mnt/delta/user_messages")
```

+   If you want to read changes since 2020-10-15, you can use the following code:

```
data_events.readStream
  .format("delta").option("startingTimestamp", "2020-10-15")
  .load("/mnt/delta/user_messages")
```

Keep in mind that if you set the starting point using one of these options, the schema of the table will still be the latest one of the Delta table, so we must avoid creating a change that makes the schema incompatible. Otherwise, the streaming source will either yield incorrect results or fail when reading the data.

Using Structured Streaming allows us to write into Delta tables and, thanks to the transaction log, Delta Lake will guarantee exactly once processing is done, even with several queries being run against the streaming table.

By default, Structured Streaming runs in Append mode, which adds new records to the table. This option is controlled by the outputMode parameter.

For example, we can set the mode to "append" and reference the table using the path method, as follows:

```
data_events.writeStream.format("delta")
  .outputMode("append")
  .option("checkpointLocation", "/delta/data_events/_checkpoints/etl-from-json")
  .start("/delta/data_events")
```