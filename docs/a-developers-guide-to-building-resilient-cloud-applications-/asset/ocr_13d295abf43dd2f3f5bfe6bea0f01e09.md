6. We can trigger our pipeline manually. To do so, we need to publish the entities to ADF. So, select **publish** at the top. After that, select **Add Trigger** on the pipeline toolbar and then select **Trigger Now.**

7. We can automate our trigger on a schedule. In the **Author** tab, select **Add Trigger** on the pipeline toolbar. Next, select **New/Edit**. Select **Choose Trigger** on the **Add Triggers** page. After that, click on **New**. We will set up our trigger on the **New Trigger** page. After completing this configuration, we need to **Publish all** to apply the changes to ADF.

In this section, we used the Azure portal to create ADF, though we can also use the Azure CLI, Azure PowerShell, REST APIs, or a .NET application. You can read more about these options in the Microsoft documentation: https://docs.microsoft.com/en-us/azure/data-factory/.

A Data Factory is primarily a data orchestration and movement tool that calls other services such as Databricks to transform data. We will learn more about Azure Databricks in the next section.

# Exploring Azure Databricks

In this section, we will explore the different features of Azure Databricks. However, we will start by defining Azure Synapse Analytics.

# Azure Synapse Analytics

Databricks is a data, analytics, and AI company. This is a link to their platform: https://www.databricks.com/. They were the first creators of the open source versions of Apache Spark, Delta Lake, and MLflow. They used some of the architectural components of these services to compose Databricks. They tried to unify the innovation of the process within data science and data engineering.

Databricks offers an interactive workspace, helping to automate the production workflow. This entire workspace is fully managed, and Azure is the channel that Databricks uses to deliver a solution that can be deployed, regardless of the underlying infrastructure, such as servers and virtual machines.

# Azure Databricks features

Azure Databricks offers multiples features:

+   - Exploiting the Spark engine, providing features such as machine learning, SQL, DataFrames, low-latency streaming APIs, as well as graph APIs to help find relationships in a dataset
+   - Several languages, such as Scala, Python, Java, R, and SQL, can be used in Azure Databricks