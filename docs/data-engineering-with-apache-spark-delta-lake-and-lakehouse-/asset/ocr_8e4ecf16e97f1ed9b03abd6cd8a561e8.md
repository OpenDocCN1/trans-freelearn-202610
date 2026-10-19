The batch pipeline will need to be created in such a way that it can load changed data between an old watermark and a new watermark. At every ingestion run, a new watermark will be created that will be used as the marker for the next run:

![](bbox=[0.155, 0.158, 0.845, 0.36])

Figure 5.23 – The watermark approach for incremental data ingestion

With the incremental ingestion approach in mind, let's start building the batch ingestion pipeline:

+   1. We will start with the creation of the Azure data factory. Browse to Home on the Azure portal. Using the search box at the top of the screen, search for data factories. Click on Data factories underneath Services:

![](bbox=[0.255, 0.516, 0.745, 0.688])

Figure 5.24 – Searching for the Data Factory service on the Azure portal

+   2. Click on the Create data factory button and input the following:

+   - Resource group: training_rg
+   - Region: East US