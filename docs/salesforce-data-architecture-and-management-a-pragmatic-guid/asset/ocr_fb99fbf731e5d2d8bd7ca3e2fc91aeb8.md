The client creates a Salesforce Bulk API job and then batches are created within that job so the data can be processed by Salesforce servers in an asynchronous mode. The client can check for the job status using methods provided with the API. The client application can also retrieve results as batches get completed but the status can also be checked using the user interface on the Monitor Bulk Data Load Jobs page. Figure 8.2 shows the major steps of how a Bulk API operation is processed:

![](bbox=[0.145, 0.225, 0.856, 0.555])

Figure 8.2 – A typical Bulk API operation

Figure 8.2 shows the major steps for using the Bulk API. A job is created, which then has batches created in it. These batches are processed asynchronously by Salesforce and the results are saved. These results can be retrieved by the client application to check on batch stastuses.

When deleting data using APIs, a soft delete happens, which means the data remains in the system available to be recovered, if needed. The Bulk API is the only API that can run a hard delete operation, which permanently deletes data from the system. Another consideration is that the Bulk API has its own governor limits, which are different from other APIs.