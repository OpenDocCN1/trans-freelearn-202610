Figure 13.3: Build a data warehouse

+   17. Click the Publish button in the lower-right corner.

+   18. Wait for the Dataflow Gen2 refresh to complete.

You have now successfully created your data warehouse.

# How it works

Microsoft Fabric provides two data warehouse experiences, the Synapse Warehouse covered in this recipe and the SQL analytics endpoint of the lakehouse (see the Engineering Data with a Lakehouse recipe in this chapter). While the latter only supports read-only queries, and the creation of views and table-valued functions (TVFs), the former supports full transactional Data Definition Language (DDL), such as CREATE, DROP, and ALTER commands, and Data Manipulation Language (DML), which includes commands such as INSERT, UPDATE, and DELETE.

There are two methods for populating a warehouse within Fabric: data pipelines and Dataflow Gen2 queries. Data pipelines are an evolution of Azure Data Factory, the cloud successor to SQL Server Integration Services (SSIS). In fact, Data Factory includes the ability to provision an SSIS runtime in order to store and execute SSIS packages. The key difference between data pipelines and dataflows is that data pipelines are an orchestration mechanism and do not transform data while a dataflow is designed to transform data. Dataflow Gen2 is an evolution of the original Power BI dataflows (now called Dataflow Gen 1) and dataflows can actually be included as an activity within a data pipeline.