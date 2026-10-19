
15. Now, chain **Create Synapse Credentials** to **Create Silver Layer Views**. Once you've done this, the pipeline should look like this:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_280_0.png)

Figure 8.21 – Chaining activities – Create Synapse Credentials to Create Silver Layer Views

16. In the **Activities** panel, search for Delete. Now, drag the **Delete** activity to the right-hand panel and provide the following properties:

- **Name:** `Cleanup Gold Data.`
- **Click on Source.** Now, click on **New**.
- **Choose Azure Data Lake Storage Gen 2** and click **Continue**.
- **Name:** `gold_data.`
- **Linked service:** `traininglakehouse.`
- **File path:** `gold/external:`

# Set properties

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_280_1.png)

Figure 8.22 – Properties of the Delete activity

17. Click on **OK**. Click on the **Logging** setting and uncheck the **Enable Logging** box.

18. Now, chain **Create Silver Layer Views** to **Cleanup Gold Data**. Once you've done this, the pipeline should look like this:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_281_0.png)

Figure 8.23 – Chaining activities – Create Silver Layer Views to Cleanup Gold Data

19. In the **Activities** panel, search for **Stored Procedure**. Now, drag the **Stored procedure** activity to the right-hand panel and provide the following properties:

- **Name**: Create Gold Layer Tables.
- Click on **Settings**.
- **Linked service**: Synapse Serverless.
- **Stored procedure name**: [dbo].[create_gold_tables].
- Under **Stored procedure parameters**: Add three parameters. Start by clicking **New**:
  - **Name**: goldns  **Type**: String
    **Value**: @pipeline().parameters.GOLD_LAYER_NAMESPACE
  - **Name**: format  **Type**: String
    **Value**: @pipeline().parameters.EXT_TAB_FILE_FORMAT
  - **Name:** extds  **Type:** String
    **Value:** @pipeline().parameters.EXT_DATA_SOURCE:

Linked service *  Synapse Serverless  Test connection  Edit  New

Stored procedure name * [dbo].[create_gold_tables]  Edit

# Stored procedure parameters

Import  New  Delete

| Name | Type | Value |
|---|---|---|
| goldns |  | @pipeline().parameters.GOLD_LAYER... |
| format |  | @pipeline().parameters.EXT_TAB_FILE... |
| extds |  | @pipeline().parameters.EXT_DATA_SO... |

Figure 8.24 – Parameters of the create_gold_tables stored procedure

20. Now, chain Cleanup Gold Data to Create Gold Layer Tables. At this point, we've finished developing electroniz_aggregation_pipeline. Click on Publish all, followed by Publish.

This is what the final version of electroniz_aggregation_pipeline should look like:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_282_0.png)

Figure 8.25 – Final electroniz_aggregation_pipeline

At this point, we are ready to unit test electroniz_aggregation_pipeline.

## Running the aggregation pipeline

In production, the aggregation pipeline will be invoked automatically once the curation pipeline completes successfully. Since we are in the unit testing phase, we will trigger it manually for now. Let's get started:

- 1. You must invoke the following commands on Azure Cloud Shell. They will invoke **electroniz_aggregation_pipeline** based on the parameters we pass. Please make sure you edit **EXT_TAB_LOCATION** as per the instructions provided. Using the Azure portal, navigate to your Azure Data Lake Storage account by going to **Home > All Resources > traininglakehouse > Endpoints.**
  
  Note the URL of **Blob Service** and use this to edit EXT_TAB_LOCATION:

```
RESOURCEGROUPNAME="training_rg"
DATAFACTORYNAME="traininglakehousedf"
PIPELINENAME="electroniz_batch_aggregation_pipeline"
az datafactory pipeline create-run --factory-name $DATAFACTORYNAME --name $PIPELINENAME --resource-group $RESOURCEGROUPNAME \
--parameters "{\"EXT_TAB_FILE_FORMAT\":\"parquetformat\", \"EXT_DATA_SOURCE\":\"trainingds\", \"EXT_CREDENTIALS\":\"trainingcreds\", \"EXT_TAB_LOCATION\":\"https://traininglakehouse.blob.core.windows.net\", \"SILVER_LAYER_NAMESPACE\":\"silver\", \"GOLD_LAYER_NAMESPACE\":\"gold\" }"
```

- 2. To verify whether the pipeline ran successfully, you can check the status of the pipeline:

  - Using the Azure portal, navigate to **Home > All Resources > trainingdatafactorydf.**
  - Click **Monitor > Pipeline Runs.**
  - Click **electroniz_batch_aggregation_pipeline**:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_284_0.png)

Figure 8.26 – Monitoring the run of electroniz_aggregation_pipeline

As you can see, we can safely infer that **electroniz_batch_aggregation_pipeline** has been completed successfully. However, this does not mean that it performed the job it was supposed to do. To be certain, we will need to verify the aggregated data in Azure Synapse.

## Understanding data consumption

Before we start verifying the aggregated data, we should focus on how our end users will be able to consume data for dashboarding, ML, and AI purposes. As per the laid-out architecture of the **Electroniz** lakehouse, we decided to publish data from both the gold and silver layers.

Publishing data from the gold layer is necessary; otherwise, how would users be able to access aggregated data? But why do we need to publish data from the silver layer? You guessed it – analytics is an ongoing process. In the future, users may want to create new dashboards and ML models for the betterment of the company.

> **Important**
> Publishing data from the gold and silver layers is acceptable because they store data that is in a clean and secure state. But the same cannot be said for data in the bronze layer. Publishing raw/unclean data not only throws a lot of work around standardization, validation, and deduplication at end users, but it also ends up exposing classified information that should have been protected.

The following diagram shows the lakehouse's structure:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_285_0.png)

Figure 8.27 – Electroniz data publishing points for end users

Our promise to Electroniz was that by the end of the project, end users will not only be able to access the aggregated data in the gold layer but also have access to curated data in the silver layer for ad hoc access. Also, in the future, there will be new requirements for aggregations and ML models that will require access to the curated data.

You may have noticed that, while creating electroniz_batch_aggregation_pipeline, a series of activities were defined in the pipeline. These end up creating views and external tables in Azure Synapse. It is important to understand how the data in the lakehouse is published for end users. The following data has been provided:

- Access to Silver Layer Data: For access to curated data, we provided EXTERNAL TABLES and VIEWS in the Azure Synapse serverless SQL pool. As we mentioned previously, not all aggregations can be predicted initially in the project. Having access to the data in the silver layer will enable end users to perform ad hoc querying and create new aggregations in the future.

- **Access to Gold Layer Data:** We provided access to the aggregated tables in the gold layer by using **EXTERNAL TABLES** in the **Azure Synapse serverless SQL pool**. These external tables were created using **CREATE EXTERNAL TABLE AS SELECT (CETAS)**, and the query results were saved (persisted) in the gold layer in **Azure Data Lake Storage Gen2**. Since aggregated data is saved in the gold layer, this substantially improves query processing times:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_286_0.png)

The preceding diagram shows how the external views and tables were created in **electroniz_batch_aggregation_pipeline.**

## Accessing silver layer data

**Create Silver Layer Views** invokes a stored procedure named **create_silver_views** in Azure Synapse. The code for this stored procedure is available here:

```
https://github.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/blob/main/project/aggregation/sql/create_silver_views.sql.
```
