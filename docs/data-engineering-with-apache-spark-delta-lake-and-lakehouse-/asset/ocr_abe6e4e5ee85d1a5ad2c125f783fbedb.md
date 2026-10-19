Azure Blob storage organizes data as follows:

+   - **Storage account**: The storage account holds your data. A unique namespace for your account is allocated in Azure. An example namespace address is `https://mydatanalytics.blob.core.windows.net`.
+   - **Containers**: A container organizes a group of blobs. A container should have a valid DNS name such as `https://mydatanalytics.blob.core.windows.net/mydatacontainer`.
+   - **Blobs**: Actual data that is stored in the containers:

![](bbox=[0.13, 0.3, 0.78, 0.56])

Figure 3.2 – Storing blobs in Azure Blob storage

> **Important Note**
> A particular drawback of object storage is its slow search performance.
> In Azure Blob storage, you can speed up searches using index tags.

# Azure Data Lake Storage Gen2

Azure Data Lake Storage offers a significant edge over Azure Blob storage when it comes to suitability for analytical workloads. But before we proceed with the reasons why, let me make one clarification: Azure Data Lake Storage is a layer built on top of Azure Blob storage.