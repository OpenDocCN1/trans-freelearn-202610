
## ADF

ADF is the Azure cloud platform for the integration of serverless data transformation and aggregation processes. It can integrate many services and provides a codeless **user interface (UI)** in order to create our ETLs.

## Creating an ADF resource

The first step is to create an ADF resource. To do this, follow these steps:

- 1. In the Azure portal, search for **Azure Data Factory** and select **Create Resource**.
- 2. Select a name for our ADF resource and attach it to the resource group in which you're using Azure Databricks, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_108_0.png)

Figure 3.22 – Creating an ADF resource

3. Before creating the resource, go to the **Git configuration** tab and select **Configure Git later**, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_109_0.png)

Figure 3.23 – Configure Git later option

4. After this is set up, we can click on the **Review + create** button, wait for it to be validated, and deploy it.

## Creating an ETL in ADF

The steps are as follows:

- 1. Once it's deployed, go to the ADF resource and click on **Author & Monitor** in the main panel, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_110_0.png)

Figure 3.24 – Going to Author & Monitor in ADF

- 2. This will take us to the **Author & Monitor** panel, from which we can create and monitor ETL pipelines. On the left tab, select the **Author** button to create our ETL pipeline, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_111_0.png)

Figure 3.25 – Going to the Author button in the left tab

- 3. This will take us to the canvas UI, in which we can create our pipelines. Select Pipelines from the panel at the left of the window and set up a name and a description.

- 4. Then, click on Search for the Databricks panel at the left of the canvas, which will show our notebook in the canvas, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_111_1.png)

Figure 3.26 – Creating a pipeline and adding a Databricks service

5. Select the Azure Databricks notebook in the canvas, and from the options at the bottom of the canvas, click on the + New button on Databricks linked service to link our Azure Databricks workspace to ADF. In order to do this, we will need an authentication token, as illustrated in the following screenshot:

# New linked service (Azure Databricks)

Figure 3.27 – Linking the Azure Databricks service with an access token

6. To generate the token, go to the Azure Databricks workspace and click on the User Settings option in your profile to take you to the screen shown in the following screenshot. Once there, generate a new token and paste it into the ADF Link new service dialog box, and finally, click on the Create button:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_113_0.png)

Figure 3.28 – Creating an Azure Databricks access token

7. Now, we can select our Azure Databricks workspace from the Linked services list and test the connection. If everything went well, the connection will be successful, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_113_1.png)

Figure 3.29 – Testing the Azure Databricks service connection

8. Once our Azure Databricks notebook has been correctly set up in our pipeline, we must establish a connection between it and ADF. We will use a variable that we will use when the ETL is triggered, and this will be the file that we want to process and persist on a table.

If we click in the blank space of the canvas, at the bottom we will see a **Parameters** option. We will create a new parameter, which will be a string named `input_file`, and have as the default value the location of the file that we want to process, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_114_0.png)

Figure 3.30 – Creating a parameter for the ETL pipeline

9. After that, click on the Azure Databricks notebook on the canvas and go to the **Settings** tab. There, we will browse and set up the notebook that we will use and add a base parameter that will point to the variable we have set up, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_115_0.png)

Figure 3.31 – Using the parameter as a variable in the Azure Databricks job

10. Until this point, we haven't discussed how this variable will be consumed in the notebook. To do this, we will add a piece of code into our notebook. We will specify that this variable is the name of the file that we need to process and will use it to format the full path of the file to be read. The code that we will use is shown in the following snippet:

```
# Creating widgets for leveraging parameters
dbutils.widgets.text("input", "", "")
input_file = dbutils.widgets.get("input_file")
print ("Param -'input':")
print (input_file)
```

This way, we are using dbutils to create a widget that will look for a variable named input_file, which is the one we have specified in ADF.

11. We will use this variable to format the path of the file that we will read and process in our notebook. The way in which we do this in our notebook is shown in the following code snippet:

```
#set the data lake file location:
file_location = f'abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/{input_file}'

#read in the data to dataframe df
df = spark.read.format("csv").option("inferSchema", "true").option("header", "true").option("delimiter",",").load(file_location)
```

12. After this step, we will apply the transformations that we consider necessary, and finally drop the existing table and save our transformed dataframe as a permanent table. The drop of the table can be done in SQL, using the following code:

```
%sql
DROP TABLE voting_data.voting_turnout_2020
And then we can save our spark dataframe as a table.
df.write.format("parquet").saveAsTable("voting_data.voting_turnout_2020")
```

13. After our notebook is ready, as well as our ADF ETL, we can trigger this by clicking on the Add Trigger button, and then Trigger Now. ADF will prompt a dialog box in which we can specify the file and directory as the variable that we have established as a parameter previously. After this is set, we can click on Run and our pipeline will be triggered, as illustrated in the following screenshot:

# Pipeline run

![](img/7e45bd08916c2a17d4e75cbb898f41e8_116_0.png)

Figure 3.32 – Running the pipeline

14. We can check the status of our pipeline by clicking on the **Monitor** tab in ADF, which will show us the status of our pipeline, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_117_0.png)

Figure 3.33 – Seeing the pipeline logs in the Monitor tab

15. As we can see, our pipeline was successfully executed, which means the file that we have specified was read, processed, and used to update our table.

Next, we'll see how to use Databricks to schedule jobs.

## Scheduling jobs with Azure Databricks

If we already know that the file we want to process will be delivered to the blob storage, we can directly schedule the notebook to run periodically. To do this, we can use Azure Databricks jobs, which is an easy way to schedule the runs of our notebooks. We will suppose now that the file path of the file we will consume is fixed.

## Scheduling a notebook as a job

The steps are as follows:

- 1. To schedule a new job, click on the **Jobs** tab in the left ribbon of our workspace and then click on **Create Job**, as illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_118_0.png)

Figure 3.34 – Creating an Azure Databricks job

- 2. After this, the rest is quite straightforward. We will be required to specify which notebook we will use, set up an execution schedule, and specify the computational resources we will use to execute the job. In this case, we have chosen to run the job in an existing cluster, but we can create a dedicated cluster for specific executions. We select the notebook that will execute the job, as illustrated in the following screenshot:
