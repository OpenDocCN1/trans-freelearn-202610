
2. In the Basics tab, fill in all the information needed to create a SQL elastic pool. Under Project details, select the subscription and the resource group. Under Elastic pool details, we will introduce the required settings for this pool, select a logical server, and then configure compute and storage resources. We have the following screen to add the elastic pool name and server:

## Elastic pool details

Enter required settings for this pool, including picking a logical server and configuring the compute and storage resources.

![](img/66aceec30d3e7dead2f71bb68a887688_173_0.png)

Figure 7.15 – Elastic pool details in a SQL elastic pool

3. In Compute + storage, we need to configure the elastic pool to select multiple databases. Click on Configure elastic pool. We have the Pool settings tab, which includes different settings such as the service tier, the compute hardware, and the number of vCores. We can enable elastic pool zone redundancy. In the Databases tab, we will add the database. Click on Add databases. Another window will be displayed, which will include all single databases. You can select one or more databases to add to the elastic pool. They will share the same compute resources:

![](img/66aceec30d3e7dead2f71bb68a887688_173_1.png)

Figure 7.16 – Add databases to the elastic pool

4. Click on Apply, then select Review + create, then Create, and the elastic pool will be created.

5. Once created, select Go to resource, and in Overview, we can check the resource consumption and identify which database is consuming the most resources. It is a monitoring tool that helps us to run diagnostics on the performance issues of each database. We can also use the Monitoring feature, under Database Resource Utilization, to display the performance of every database within the pool.

6. We can also configure our elastic pool by selecting Configure:

![](img/66aceec30d3e7dead2f71bb68a887688_174_0.png)

Figure 7.17 – Configure an elastic pool

7. If we need to update the elastic pool configuration by increasing or decreasing the allocated resources, we can adjust the pool size, the service tier, or the resources of every database. We can add or remove the databases inside the pool:

![](img/66aceec30d3e7dead2f71bb68a887688_174_1.png)

Figure 7.18 – Pool settings in an elastic pool

If you have a large number of databases to migrate using a specific pattern, Azure SQL Database elastic pools are the best solution. In the next section, we will deploy a SQL Managed Instance.

## Exercise 3 – deploying SQL Managed Instance

If we go back to the SQL deployment page shown in Figure 7.2, we can now select **SQL managed instances**. Leave **Resource type** as **Single instance** and click on **Create**:

![](img/66aceec30d3e7dead2f71bb68a887688_175_0.png)

Figure 7.19 – Create a SQL managed instance

In the **Basics** tab, we will introduce the information related to the project details, the managed instance details, and authentication; it is similar to a SQL database.

In the **Networking** tab, we will configure the virtual network and public endpoint. You can leave the default configuration; it depends on your internal configuration.

In the **Security** tab, we need to enable Microsoft Defender for SQL. We can configure system-assigned and user-assigned managed identities to enable central access management between this database and other Azure resources.

We have more tabs, such as **Additional settings**, which are used to customize additional configuration parameters, including geo-replication, time zone, and collation.

Then, we click on the **Review + create** button.

Azure SQL Managed Instance is used for an easy lift and shift because it is compatible with on-premises servers. It is a fully managed PaaS and easy to use and configure for modern databases.

Let’s explore how we can connect Azure SQL Database to an ASP.NET application.

Figure 7.21 – Connect dialog

We will follow these steps to configure the database connection:

- 1. In the **Connect** dialog, expand the **Azure** node. All Azure SQL Database instances will be listed.
- 2. We will select the database that we will use with our application.
- 3. The information related to the database will be filled in at the bottom, but we have to enter the administrator password after we click on the **Connect** button.
- 4. The **Create a new firewall rule** dialog is displayed. To connect to your database from outside Azure, you have to create a server-level firewall rule. A firewall rule allows the public IP address of the local computer.

6. Go back to our application. Open the appsettings.json file and add the connection string:

![](img/66aceec30d3e7dead2f71bb68a887688_179_0.png)

Figure 7.24 – Connection string of the database in Visual Studio 2022

7. We can use the Database.Migrate() call to help us run the database in Azure SQL Database.

If you use a local database for development and Azure SQL Database for production, you need to configure each environment in Program.cs.

## Summary

In this chapter, we explored the different services to deploy SQL Server in Azure. We started by deploying a single SQL database. We have multiple options, such as the serverless database option, that permit us to create a low-cost database. Serverless is a compute tier for single databases in Azure SQL Database that automatically scales compute based on workload demand and bills for the amount of compute used per second. After we deployed the Azure SQL Database elastic pool, which includes the serverless option, we deployed a SQL Managed Instance and, in the end, we connected an Azure SQL database to an existing ASP.NET application.

In the next chapter, we will talk about the different storage options that are available in the Azure Storage services, and the scenarios in which each storage option is appropriate.

## Further reading

If you want more information related to the Azure database service, you can check out these links:

- https://learn.microsoft.com/en-us/azure/azure-sql/database/sql-database-paas-overview?view=azuresql
- https://learn.microsoft.com/en-us/azure/azure-sql/database/?view=azuresql

## Questions

- 1. What is Azure SQL Database?
- 2. How do we connect Azure SQL Database to an ASP.NET app?

## Working with Azure Storage

Azure Storage is a Microsoft cloud storage solution for more than just data storage. Azure Storage offers a scalable and secure object store for a variety of data objects, a filesystem service in the cloud, a messaging store for relevant messaging, and an SQL store.

This chapter will cover the different storage options that are available in Azure Storage and the scenarios in which each storage option is appropriate.

In this chapter, we’re going to cover the following main topics:

- Azure Storage account
- Exploring Azure Table storage
- Exploring Azure Blob Storage
- Exploring Azure Disk Storage
- Exploring Azure Files

## Azure Storage account

In the healthcare solution, we have a platform for patient medical records. It includes unstructured data such as documents, messages, notes, and images related to a patient, for example, diagnoses documents, annual exams, and disease surveillance. All these documents are stored on a virtual machine using a custom solution to extract or load the unstructured data.

Azure Storage is a service that can be used to store files, messages, tables, and other types of data. It’s scalable, secure, and easy to manage and can be accessed from anywhere. Azure Storage is following two cloud models: infrastructure as a service by using virtual machines and platform as a service by using native cloud services. Azure Storage includes four data services, which can be accessed through a storage account.

Each type supports different features and has a unique URL address to access. The data and your Azure Storage account are always replicated to ensure durability and high availability.
