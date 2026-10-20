# Job logs

If everything went well, we will see the status marked as **Succeeded**, as illustrated in the following screenshot. Otherwise, it will be marked as **Failed**, and we will need to check the logs of that specific run to figure out what went wrong:

![](bbox=[0.13, 0.19, 0.87, 0.43])

Figure 3.37 – Ongoing and completed runs for the job

Luckily, our job was successful, which means our file will be processed and our table will be updated every hour.

# Summary

In this chapter, we have applied the concepts learned previously to create and connect to Azure Databricks resources to store our data, such as ADLS Gen2, AWS S3, and Azure Blob storage. We have also learned how to ingest data from the storage to a Spark dataframe, transform it using Spark methods or using SQL scripts, and then persist the data into tables. Finally, we have seen how to schedule our jobs with Azure Databricks jobs, and how to create a pipeline in ADF and trigger it using a defined variable.

In the next chapter, we will learn about Azure Delta Lake and how to use it to create reliable ETL pipelines using Azure Databricks.