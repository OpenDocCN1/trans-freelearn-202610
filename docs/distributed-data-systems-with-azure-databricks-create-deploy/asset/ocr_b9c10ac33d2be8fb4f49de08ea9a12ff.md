# Structured Streaming model

A Structured Streaming model is based on a simple but powerful premise: any query executed on the data will yield the same results as a batch job at a given time. This model ensures consistency and reliability by processing data as it arrives in the data lake, within the engine, and when working with external systems.

As seen before in the previous chapters, to use Structured Streaming we just need to use Spark dataframes and the API, stating which are the I/O locations.

Structured Streaming models work by treating all new data that arrives as a new row appended to an unbound table, thereby giving us the opportunity to run batch jobs on data as if all the input were being retained, without having to do so. We then can query the streaming data as a static table and output the result to a data sink.

Structured Streaming is able to do this, thanks to a feature called **Incrementalization**, which plans a streaming execution every time that we run a query. This also helps to figure out which states need to remain updated every time new data arrives.

We define when the tables will be updated using specific triggers when using Structured Streaming dataframes. Each time a trigger is activated, Azure Databricks will update the table incrementally by creating new rows for each file that arrives in our data source.

Finally, the last part to be defined are the output modes. These modes allow us to determine how to write incrementally to external systems such as **Simple Storage Service (S3)** or a database in three different ways, outlined as follows:

+   - **Append mode**: Only new rows are appended. This mode doesn't allow changes to existing rows.
+   - **Complete mode**: The entire table is rewritten to the external storage every time there is an update.
+   - **Update mode**: Only rows that have changed since the last trigger are written to the external storage, which allows in-place updates such as SQL tables.

Structured Streaming output tables provide consistent results based on a sequential stream, and failure tolerance is also ensured when working with output sinks. Using the Complete output mode, for example, out-of-order data will be handled by updating the records in a SQL table. We can establish a threshold to avoid overly old data updating a table.