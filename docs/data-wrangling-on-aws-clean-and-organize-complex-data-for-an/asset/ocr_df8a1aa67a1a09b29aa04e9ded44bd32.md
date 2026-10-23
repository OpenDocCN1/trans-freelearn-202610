Search for Athena start and you should see the Athena StartQueryExecution API object in the results window. Drag that inside the map function, as shown here:

![](bbox=[0.42, 0.15, 0.67, 0.44])

Figure 7.48: Step function workflow – Map function executing Athena queries in parallel

Enter the following parameters for the Athena StartQueryExecution API object:

API Parameters: Enter the following values. If you are using a different workgroup for specific features, update the input accordingly:

```
{
  "QueryString.$": "$",
  "WorkGroup": "primary",
  "ResultConfiguration": {
    "OutputLocation": "s3://<<path to store athena results>>/"
  }
}
```

Wait for task to complete: Check the checkbox. This is very important to do so that the workflow waits for the completion of the query. If you fail to check this checkbox, the subsequent steps will fail when they try to retrieve the results of an Athena query that has not finished yet.