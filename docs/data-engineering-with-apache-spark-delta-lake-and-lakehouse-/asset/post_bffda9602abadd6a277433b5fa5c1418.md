
2. Copy the contents of the following file in the training Git repository:
https://raw.githubusercontent.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/main/project/prep/stores/sql/incremental/1/storedb_incremental_1.sql.

Paste the contents into the Query Editor window. Select Run. This will insert 100 new rows in the store_customers table and 200 rows in the store_orders table:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_166_0.png)

3. Additionally, we have received a new website tracking log that needs to be ingested. Copy each of the following commands, line by line, and paste them into the Cloud Shell window. Then, press Enter:

```
LOGSTORAGEACCOUNTNAME="ecommercetracking"
```

```
LOGSCONTAINER="ecommercelogs"
```

```
RESOURCEGROUPNAME="training_rg"
```

```
LOCATION="eastus"
```

```
KEY=`az storage account keys list -g $RESOURCEGROUPNAME -n $LOGSTORAGEACCOUNTNAME | jq .[0].value`
```

```
az storage container create -n $LOGSCONTAINER --account-name $LOGSTORAGEACCOUNTNAME --account-key $KEY
```

```
az storage blob upload --container-name $LOGSCONTAINER --account-name $LOGSTORAGEACCOUNTNAME --name electroniz_access_log_2.json --file ~/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/prep/ecommerce_logs/electroniz_access_log_2.log --account-key $KEY
```

4. Go back to Azure Data Factory and wait until the current hour expires. In the next iteration, the pipeline will only pull incremental data from the sales database. When the next hour arrives, rerun the pipeline using the following command:

```
PIPELINENAME="electroniz_batch_ingestion_pipeline"
DATAFACTORYNAME="trainingdatafactory100"
RESOURCEGROUPNAME="training_rg"
az datafactory pipeline create-run --factory-name $DATAFACTORYNAME --name $PIPELINENAME --resource-group $RESOURCEGROUPNAME
```

5. Now, click on Pipeline runs. After some time, Status will change to Succeeded. This indicates a successful run:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_167_0.png)

Figure 5.52 – The status of the Electroniz batch ingestion pipeline for the second iteration

6. From the preceding screenshot, it looks like the pipeline has been successfully completed for the second iteration. Let’s check whether the watermark was updated. Run the following watermark query one more time:

```
SELECT TOP (1000) * FROM [dbo].[watermarktable]
```

Notice that the watermark value for the store_customers and store_orders tables have been updated:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_168_0.png)

Figure 5.53 – The edited watermark values after the second run of the batch ingestion pipeline

7. Let's validate whether the incremental data was correctly ingested. From the Azure portal, navigate to Home | All resources | traininglakehouse.

Then, navigate to Containers | Bronze | sales. Browse to the newly created folder from the last hour.

You should expect a new hour folder in the [dbo].[store_customers] and [dbo].[store_orders] folders as follows:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_168_1.png)

Figure 5.54 – New hourly folder for the second iteration

8. Click on the new hour folder. It should contain a new file that has 100 rows for the store_customers table and 200 rows in the store_orders table:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_169_0.png)

Figure 5.55 – The results of the incremental data run of the batch ingestion pipeline

## The third run of the pipeline – updates

Currently, the second hour after the initial ingestion is taking place. Once again, we are going to assume that, at this time, the sales database is still fully operational, but the database activity in this hour amounted to five customers calling the help desk to **update** their orders:

- 1. Before the next run happens, let's show some update activity in the sales database. Copy the contents of the following file in the training Git repository:
https://raw.githubusercontent.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/main/project/prep/stores/sql/incremental/2/storedb_incremental_2.sql.

Paste the contents inside the Query Editor window. Click on Run. This will update five orders in the store_orders table:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_170_0.png)

Figure 5.56 - Updating data in the stores_orders table

- 2. In the next iteration, the pipeline will only pull incremental data from the sales database. When the next hour arrives, rerun the pipeline using the following command:

```
PIPELINENAME="electroniz_batch_ingestion_pipeline"
DATAFACTORYNAME="trainingdatafactory100"
RESOURCEGROUPNAME="training_rg"
az datafactory pipeline create-run --factory-name $DATAFACTORYNAME --name $PIPELINENAME --resource-group $RESOURCEGROUPNAME
```

- 3. Now click on Pipeline runs. After some time, Status will change to Succeeded. This indicates a successful run:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_170_1.png)

Figure 5.57 – The status of the Electroniz batch ingestion pipeline for the third iteration

4. From the preceding screenshot, it looks like the pipeline has been successfully completed for the third time. Let's validate whether the incremental data was correctly ingested. From the Azure portal, navigate to Home | All resources | traininglakehouse.

Then, navigate to Containers | Bronze | sales. Browse to the newly created folder from the last hour.

You should expect a new hour folder in the [dbo].[store_customers] and [dbo].[store_orders] folders, as follows:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_171_0.png)

Figure 5.58 – New hourly folder for the third iteration

5. Click on the new hour folder for store_orders. It should contain a new file that has five rows. Notice that each row has been marked as EDIT, signaling an updated row:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_171_1.png)

Figure 5.59 – The results of the incremental data run of the batch ingestion pipeline

## The fourth run of the pipeline – inserts

Currently, the third hour after the initial ingestion is taking place. In this run, we will assume that some customers have called to cancel (delete) their orders:

- 1. Before the next scheduled run occurs, let's show some activity in the sales database. Copy the contents of the following file in the training Git repository: https://raw.githubusercontent.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/main/project/prep/stores/sql/incremental/3/storedb_incremental_3.sql.

Paste the contents inside the Query Editor window. Select Run. Notice that order_mode shows the orders have been deleted:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_172_0.png)

Figure 5.60 – Deleted data in the stores_orders table

- 2. In the next iteration, the pipeline will only pull incremental data from the sales database. When the next hour arrives, rerun the pipeline using the following command:

```
PIPELINENAME="electroniz_batch_ingestion_pipeline"
DATAFACTORYNAME="trainingdatafactory100"
RESOURCEGROUPNAME="training_rg"
az datafactory pipeline create-run --factory-name $DATAFACTORYNAME --name $PIPELINENAME --resource-group $RESOURCEGROUPNAME
```

3. Now click on **Pipeline runs**. After some time, **Status** will change to **Succeeded**. This indicates a successful run:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_173_0.png)

Figure 5.61 – The status of the Electroniz batch ingestion pipeline for the fourth iteration

4. Let’s validate whether the incremental data was correctly ingested. From the Azure portal, navigate to **Home | All resources | traininglakehouse.**

Navigate to **Containers | Bronze | sales**. Browse to the newly created folder from the last hour.

You should expect a new hour folder in the **[dbo].[store_orders]** folder, as follows:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_173_1.png)

Figure 5.62 – New hourly folder for the fourth iteration

5. Click on the new hour folder for **store_orders**. It should contain a new file that has five rows. Notice that each row has been marked as **DELETE**, signaling an updated row:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_174_0.png)

Figure 5.63 – The results of the incremental data run of the batch ingestion pipeline

Everything seems to have worked as expected. It is a good idea to rebuild and test the pipeline a few times to build up your practice. However, there is one last thing remaining before you start to build the **Electroniz** streaming ingestion pipeline. Once you are comfortable that **electroniz_batch_ingestion_pipeline** is working as expected, export the code of the pipeline from **Azure Data Factory** so that you can check it in your source control system, such as **Git**.

6. Go back to the **Azure data factory** window. Using the top menu, click on **Save as template.**

Now click on **Export template**. This will download a file named **electroniz_batch_ingestion_pipeline.zip** to your computer. You can check this file inside your source control system. I have provided a sample of the same file in the training Git repository:

```
https://github.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/tree/main/project/ingestion/batch
```
