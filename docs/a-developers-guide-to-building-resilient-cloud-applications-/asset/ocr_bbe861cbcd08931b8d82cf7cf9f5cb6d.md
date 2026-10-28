+   - **Geo-zone-redundant storage (GZRS)**: This is the best option for critical data scenarios because it offers optimal data protection, including a combination of the features of GRS and ZRS.

In the following figure, you can see the options for configuring redundancy:

![](bbox=[0.14, 0.19, 0.87, 0.35])

Figure 8.4 – Create a storage account – Redundancy configuration

Here, we have the classic deployment model, but if we need to create a legacy storage account type in **Instance details**, we will click on **here**, as presented in the following figure:

![](bbox=[0.2, 0.48, 0.8, 0.55])

Figure 8.5 – Select a legacy storage type

A new line will be added that is related to the storage account. If you need more details, you can follow this link: https://docs.microsoft.com/en-ca/azure/storage/common/storage-account-create?tabs=azure-portal. This is used to choose an account type that matches your storage needs and optimizes your costs.

A storage account contains all Azure Storage data objects, such as blobs, files, and disks.

In the next section, we will start exploring Azure Table Storage.

# Exploring Azure Table Storage

Azure Table Storage provides support for storing structured data. It implements a NoSQL key-value model, which means that it doesn’t have any concept of relationships, stored procedures, secondary indexes, or foreign keys.