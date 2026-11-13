# Checkpointing

In Chapter 4, Understanding Spark Programming Model, we discussed various techniques of caching/persisting RDDs to avoid all the re-computation in cases of failures. The same techniques can be followed with a DStream type as well. However, as streaming applications are meant to run all the time, an application may fail because of system failures or network failures as well. To make the Spark Streaming application capable of recovering from such failures, it should be checkpointed to all external locations, most likely a fault tolerant storage such as HDFS and so on.

Spark Streaming allows us to checkpoint the following types of information:

+   * Metadata checkpointing: This helps to checkpoint the metadata of the Spark Streaming application such as configurations provided to the application while submitting, the batches of data that are in processing, and a set of operations defined to be executed to incoming DStreams, and so on.

+   * Data checkpointing/generated DStream checkpointing: This helps to checkpoint generated DStreams in case of stateful operations as generated DStreams can be combination of various transformed DStreams across multiple batches and this chain keeps on increasing in a streaming application. So it must be checkpointed.

Checkpoint generated DStreams, in the case of stateful transformations, are mandatory in Spark Streaming. However, users can also create checkpoints in case of stateless transformations to checkpoint metadata of the Spark Streaming application or to avoid re-computation. Even RDDs can be checkpointed.

To enable checkpointing in Spark Streaming applications, the following method can be called on the Spark Streaming context:

```
| jssc.checkpoint("/path/of/the/checkpoint_directory");
```