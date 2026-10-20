+   - In a more general way, we can read a stream into a DataFrame using the following Python command:
+   - Then, once we have read the stream, we can write it to a delta table by specifying a checkpoint location:

```
df = spark.readStream.format("cloudFiles").
option(<cloudFiles-option>, <option-value>) \
.schema(<file_schema>) .load(<input-path>)
```

```
df.writeStream.format("delta").
option("checkpointLocation", <checkpoint-path>).
start(<output-path>)
```

When a new stream is created, a checkpoint is created in that directory. If the checkpoint is changed when the service is restarted, it will be assumed that a new stream has been created. The file notification process is more suitable when we're dealing with large amounts of data. This is because files are processed just once when they arrive, with no need for us to list the entire directory, as well as because multiple streaming queries can run in one input directory simultaneously.

One of the drawbacks of file notification is the need to spin up more resources in order to detect the files arriving in storage. It's more suitable when we're managing a large number of files, and it's also a more scalable solution.

Directory listing is the default mode of Auto Loader. This mode does what it says and lists the directory to identify files that arrive in it. It doesn't need to configure new permissions and is suitable for when a small number of files are arriving regularly in storage. However, if the number of files increases rapidly, it can quickly become difficult to list the entire directory.

If you need to change to file notification mode, you will need to restart the streaming process.

In the next section, we will learn how we can optimize DDL operations that are being performed on the data that we just fetched by batching table operations.