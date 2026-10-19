Paste the contents inside the Query Editor window. Click on Run. This will update five orders in the store_orders table:

![](bbox=[0.13, 0.15, 0.87, 0.41])

Figure 5.56 - Updating data in the stores_orders table

+   2. In the next iteration, the pipeline will only pull incremental data from the sales database. When the next hour arrives, rerun the pipeline using the following command:

```
PIPELINENAME="electroniz_batch_ingestion_pipeline"
DATAFACTORYNAME="trainingdatafactory100"
RESOURCEGROUPNAME="training_rg"
az datafactory pipeline create-run --factory-name $DATAFACTORYNAME --name $PIPELINENAME --resource-group $RESOURCEGROUPNAME
```

+   3. Now click on Pipeline runs. After some time, Status will change to Succeeded. This indicates a successful run:

![](bbox=[0.13, 0.7, 0.87, 0.88])

Figure 5.57 – The status of the Electroniz batch ingestion pipeline for the third iteration