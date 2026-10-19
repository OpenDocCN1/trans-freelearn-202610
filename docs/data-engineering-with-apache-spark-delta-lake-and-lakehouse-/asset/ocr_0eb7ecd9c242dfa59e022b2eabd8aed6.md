![](bbox=[0.28, 0.09, 0.66, 0.22])

Figure 9.3 – The Electroniz ingestion, curation, and aggregation pipelines

We will get started by performing the following steps:

+   1. Using the Azure portal, navigate to Home > All Resources > trainingdatafactory100. Click on Open underneath Open Azure Data Factory Studio.
+   2. Using the panel on the left-hand side, from the menu, click on Author. In the Factory Resources panel, click on the three dots to the right of Pipelines and choose New Pipeline.

In the Properties panel, input the following:

+   - Name: electroniz_master_pipeline
+   - Description: This pipeline runs the ingestion, curation, and aggregation pipeline.

Using the bottom panel, click on Parameters and add the following parameters:

+   - Name: STORAGE_ACCOUNT Type: String
+   - Name: BRONZE_LAYER_NAMESPACE Type: String
+   - Name: STORE_SALES_FOLDER Type: String
+   - Name: TABLE_LIST Type: String
+   - Name: CURRENCY_LIST Type: String
+   - Name: CURRENCY_FOLDER Type: String
+   - Name: GEOLOCATION_FOLDER Type: String
+   - Name: LOGS_FOLDER Type: String
+   - Name: ECOMM_FOLDER Type: String
+   - Name: EXT_TAB_FILE_FORMAT Type: String