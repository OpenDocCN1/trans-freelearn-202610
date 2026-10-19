# PaaS use case

Here are a couple of use cases where using the PaaS layer is warranted:

+   - An organization has on-premises Hadoop/Spark/Kafka clusters and quickly wants to migrate to the cloud.
+   - An organization has limited cloud skills, so they want to use services with low operational requirements.
+   - An organization where security regulations permit the use of multi-tenant cloud services and can tolerate variability in compute power.

Here are a few of the Azure-managed PaaS services.

# Data storage services in Microsoft Azure

During the big data wave, cloud vendors, including Microsoft Azure, started to launch storage solutions specifically targeted at storing vast volumes and varieties of data. To meet the high demand of storing large files at scale, object-level storage has become a unanimous choice among all cloud vendors. Let's take a look at the storage types in Microsoft Azure that are deemed suitable for data lakes.

# Azure Blob storage

**Azure Blob storage** is an object storage solution that is highly optimized for storing large amounts of text and binary data. Azure Blob storage was built with a huge focus on scalability, high availability, and security. It stores data in three tiers: hot, cool, and archive. The hot tier is used for frequently accessed data, cool for infrequently accessed data, and archive for rarely accessed data. Depending on usage, you can choose between two performance tiers: standard and premium.

Data in Azure Blob storage is protected by several authentication mechanisms such as **Azure Active Directory**, shared keys, and **shared access signatures (SASes)**. You can also protect your blobs using **role-based access control (RBAC)**. You may turn versioning on for extra protection against accidental changes and deletes. This feature maintains the previous version of the blob in case you need to recover it.

Blobs in Azure Blob storage are automatically encrypted and redundant. Multiple copies of data are stored – three copies are stored within a single physical location by default, and several options exist for storing copies across zones and regions.