
3. If you have no storage created, you will be prompted with a message that says that we require storage to persist files and we can create it there directly.
4. Once the storage has been created, we have access to the Azure Cloud Shell in which we can create a virtual environment and install the Databricks CLI in the same way that we would do in a local terminal.

Once you complete these steps, you can start using the Databricks CLI from Azure Cloud Shell.

## Summary

In this chapter, we explored the possibilities of creating an Azure Databricks service either through the UI or the ARM templates, and explored the options in terms of enforcing access control on resources. We also reviewed the different authentication methods, the use of VNets to have a consistent approach when dealing with access policies throughout Azure resources, and how we use the Databricks CLI to create and manage clusters, jobs, and other assets. This knowledge will allow us to efficiently deploy the required resources to work with data in Azure Databricks while maintaining control over how these assets access and transform data.

In the next chapter, we will apply this and the previous concepts to run more advanced notebooks, create ETLs, data science experiments, and more. We'll start with ETL pipelines in Chapter 3, Creating ETL Operations with Azure Databricks.

## Section 2:
Data Pipelines
with Databricks

This section will help you work with data and learn how to manage different sources of data, transform it, and use it in the interactions of the different options that Azure has in order to create entire ETL pipelines.

This section contains the following chapters:

- Chapter 3, Creating ETL Operations with Azure Databricks
- Chapter 4, Delta Lake with Azure Databricks
- Chapter 5, Introducing Delta Engine
- Chapter 6, Introducing Structured Streaming

## 3

## Creating ETL Operations with Azure Databricks

In this chapter, we will learn how to set up different connections to use external sources of data such as **Simple Storage Service (S3)**, set up our Azure Storage account, and use Azure Databricks notebooks to create **extract, transform, and load (ETL)** operations that clean and transform data. We will leverage **Azure Data Factory (ADF)**, and finally, we will look at an example of designing an ETL operation that is event-driven. By exploring the sections in this chapter, you will be able to have a high-level understanding of how data can be loaded from external sources and then transformed into data pipelines, constructed and orchestrated using Azure Databricks. Let's start with a brief overview of **Azure Data Lake Storage Gen2 (ADLS Gen2)** and how to use it in Azure Databricks.

In this chapter, we will look into the following topics:

- Using ADLS Gen2
- Using S3 with Azure Databricks
- Using Azure Blob storage with Azure Databricks
- Transforming and cleaning data
- Orchestrating jobs with Azure Databricks
- Scheduling jobs with Azure Databricks

### Technical requirements

To follow the examples given in this chapter, you need to have the following:

- An Azure subscription
- An Azure Databricks notebook attached to a running container
- An Amazon Web Services (AWS) S3 bucket with the files that we want to access, along with access credentials with the proper permissions to read files in the bucket

### Using ADLS Gen2

To persist data in Azure Databricks, we need a data lake. We will use ADLS Gen2, so our first step is to set up an account. This will allow us to store permanent data and use it to run ETL pipelines, get analytics, or use it to build machine learning (ML) models.

### Setting up a basic ADLS Gen2 data lake

To set up an ADLS Gen2 subscription, we first need to create a new resource in our Azure portal. To do this, follow these next steps:

1. Search for Storage accounts and select Create a new Storage account.
2. Attach it to a resource group, set up a name, set the Account kind field to StorageV2 (general-purpose v2) and, finally, set Replication to Locally-redundant storage (LRS), as illustrated in the following screenshot:

### Create storage account

Home > Storage accounts >

Basics Networking Data protection Advanced Tags Review + create

Azure Storage is a Microsoft-managed service providing cloud storage that is highly available, secure, durable, scalable, and redundant. Azure Storage includes Azure Blobs (objects), Azure Data Lake Storage Gen2, Azure Files, Azure Queues, and Azure Tables. The cost of your storage account depends on the usage and the options you choose below. Learn more about Azure storage accounts

# Project details

Select the subscription to manage deployed resources and costs. Use resource groups like folders to organize and manage all your resources.

Subscription *
Pay-As-You-Go

Resource group *
Create new

# Instance details

The default deployment model is Resource Manager, which supports the latest Azure features. You may choose to deploy using the classic deployment model instead. Choose classic deployment model

Storage account name *
databricksstorage

Location *
(US) East US

Performance
Standard Premium

Account kind
StorageV2 (general purpose v2)

Replication
Locally-redundant storage (LRS)

Review + create
< Previous
Next : Networking >

Figure 3.1 – Creating an ADLS Gen2 subscription

3. Before finalizing, in the Advanced tab, set the Hierarchical namespace option to Enabled so that we can use ADLS Gen2 from our notebooks. The following screenshot illustrates this:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_88_0.png)

Figure 3.2 – Enabling the Hierarchical namespace option

4. After this, you can click on the Review + create button. Your resource will be validated and you can then start the deployment, which usually takes a couple of minutes. Finally, we will have our ADLS Gen2 storage account set up. Now, we can upload files to it, for which we will use Storage Explorer.

Next, we'll add in the data.

### Uploading data to ADLS Gen2

To upload the file that we will use in our demo, search for **Storage Explorer** in the Azure portal, and then follow these steps:

1. Select a subscription from the left tab—this will show all the containers related to that subscription. Right-click on the container in the ADLS Gen2 storage account and create a new filesystem that will contain our data. The process is illustrated in the following screenshot:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_89_0.png)

Figure 3.3 – Creating a filesystem

2. After the filesystem has been created, select the **Upload** button from the options bar at the top of the main panel, as illustrated in the following screenshot. This will prompt us with a message to download **Storage Explorer**, which is a program that allows us to upload files stored locally and manage our storage account:

![](img/7e45bd08916c2a17d4e75cbb898f41e8_90_0.png)

Figure 3.4 – Selecting the Upload button to download Storage Explorer

3. Follow the steps to install **Storage Explorer** on your local machine.

4. Once it is installed, log in to your Azure account and navigate to the left tab until you find the subscription in which we have created our ADLS Gen2 account. There, we will see the filesystem that we have created, and from there, we can upload local files using the **Upload** button in the top-left bar, as illustrated in the following screenshot:

# Figure 3.5 – Azure Storage Explorer

The data that we will be using is the 2020 US General Election Turnout rates data, which comprises the turnout rates for the voting-eligible population in the 2020 United States (US) election between Donald Trump and Joe Biden. The voter turnout is the percentage of eligible voters who voted in an election, and the US government has been publishing this data since the beginning of elections in the early days of US independence.

The data is from the Census Bureau's Current Population Survey (CPS); the November Voting and Registration Supplement can be downloaded from the following link: https://data.world/government/vep-turnout.

Now that we have set up our storage account and uploaded the files, we need to access them from our Azure Databricks notebook. Let's see how we can do this.
