# # How to utilize query metadata for performance

The pandas DataFrame also has a query_metadata dictionary, which is similar to the output of the GetQueryExecution command from Athena that retrieves each query metadata. In the following screenshot, you can see information such as when the query started and finished, where the results are stored, the format of the file, and so on.

```
```json
result_type = {
  'results.query_metadata': {
    'QueryExecutionId': 'a551b95f-c7e6-4423-b3ef-a2994f0e7f6',
    'Query': 'CREATE TABLE `default`...',
    'StatementType': 'DDL',
    'ResultConfiguration': {'OutputLocation': 's3://...'},
    'Statistics': {'TotalExecutionTime': 1416, 'EngineExecutionTime': 1289, 'QueryQueueTime': 106, 'ServiceProcessingTime': 21, 'DataScannedInBytes': 1495},
    'DataManifestLocation': 's3://...'
  }
}
```
```

![](bbox=[0.05, 0.5, 0.95, 0.55])

However, we also have a bunch of important attributes under the “Statistics” segment. They provide information on the query wait time, the planning time, the execution time, and also the data scanned. Note that Athena queries are charged based on the data scanned:

> TotalExecutionTime = EngineExecutionTime(includes QueryPlanningTime) + QueryQueueTime + ServiceProcessingTime

Here is an example from the preceding case – 1,416 ms = 1,289 ms + 106 ms + 21 ms.

The EngineExecution time was 1,289 ms, out of which Queryplanning time was 156 ms. So, the actual execution time for the query in the Athena engine was 1,133ms.

+   - TotalExecutionTime: The number of ms that Athena took to run a DDL or DML query.
+   - EngineExecutionTime: The number of ms that the Athena engine took to run the query. The more complex the query and the larger the data volume, the longer this phase takes.
+   - QueryPlanningTime: The number of ms that Athena took to plan the query processing flow.