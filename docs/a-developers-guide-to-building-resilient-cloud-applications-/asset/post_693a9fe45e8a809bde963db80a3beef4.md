
## Exploring Azure Files

Azure Files storage provides cloud-based file shares, which can be accessed from the cloud or on-premises.

Azure Files storage offers shared storage for applications using the industry-standard **Server Message Block (SMB)** protocol and the **Network File System (NFS)** protocol. Azure Virtual Machines and cloud services can share file data across application components via mounted shares. On-premises applications can also access file data under shared repositories. That means multiple virtual machines can share the same files with both read and write access. You can also read the files using the **Representational State Transfer (REST)** API interface or the storage client libraries. Applications running in Azure Virtual Machines, Windows, Linux, or other cloud services can mount a file storage share to access file data, just as an application would mount a typical SMB or NFS share. Any number of Azure Virtual Machines or roles can mount and access the file storage share simultaneously. Azure Files SMB file shares are accessible from Windows, Linux, and macOS clients. NFS file shares are accessible from Linux and macOS clients.

### The common uses of file storage

Azure Files can be used to completely replace traditional on-premises file servers or **Network Attached Storage (NAS)** devices. Popular operating systems, such as Windows, macOS, and Linux, can directly mount Azure file shares wherever they are in any location. Azure Files makes it easy to lift and shift applications to the cloud that expect a file share to store file applications or user data. Azure file shares can also be replicated with Azure File Sync to Windows servers, either on-premises or on the cloud. To ensure performance and distributed caching of data where it is used, shared application settings are stored, for example, in configuration files, and diagnostic data such as logs, metrics, and crash dumps to a shared location. We store the tools and utilities needed to develop or administer Azure virtual machines or cloud services. That has covered all the common use cases of file storage for Azure Files.

### Adding Azure Files in the Azure portal

Now, we will see how we add Azure Files in the Azure portal:

1. Go back to **Storage browser** and select **File shares**:

### Storage account metrics

The data provided is regularly updated about 2-4 times a day and published hourly. If your account has extremely large objects, it may be over a day between updates.

Figure 8.24 – Storage browser | File shares

2. Select Add file share and add a name. Select Tier, then Transaction optimized or Hot or Cool:

   - **Transaction optimized** is used to enable transaction-heavy workloads. We use it for applications requiring file storage as a backend store.
   - **Hot** is the optimized option for some file-sharing scenarios; for example, if a team is sharing files, Azure Files will synchronize them.
   - **Cool** is used for archive storage scenarios.

Once created, we can explore the different settings, as presented in the following screenshot:

![](img/66aceec30d3e7dead2f71bb68a887688_202_0.png)

Figure 8.25 – Azure Files settings

3. If we select **Connect**, we can configure the connection to the Azure file share from Windows, Linux, or macOS. To secure the connection, select **Active Directory** or **Storage account key** for Windows. We can change **Tier** from **Transaction optimized** to **Cold**, for example, or from **Cold** to **Hot**.

Sometimes, it’s not easy to decide when to use Azure file shares instead of blobs or disk shares in your solution. If we compare the different features, we can decide which solution is suitable. If we compare Azure Files to Azure Blob side by side, we see that Azure Files provides an SMB and NFS interface, client libraries, and the REST interface, which allows access from anywhere to store files. Azure Blob provides client libraries and the REST interface, which allows unstructured data with flat namespaces to be stored and accessed at a massive scale in the blobs.

### When to use Azure files versus blobs

You can use Azure files to lift and shift an application to the cloud, which already uses the native filesystem APIs to share data between it and other applications running in Azure. You want to store development and debugging tools that need to be accessed from many virtual machines. Azure blobs, on the other hand, can be used if you want your application to support streaming and random-access scenarios and you want to be able to access application data from anywhere. There are a few other distinguishing features on when to select Azure files over Azure blobs. Azure files are true directory objects, while Azure blobs are a flat namespace. Azure files can be accessed through file shares. However, Azure blobs are accessed through a container. Azure files provide shared access across multiple virtual machines, while Azure disks are exclusive to a single virtual machine.

When you select which storage feature to use, you should also consider pricing and perform a cost-benefit analysis.

Azure Storage is accessible via a REST API and designed for Microsoft Azure Virtual Machines, as illustrated in the following diagram:

![](img/66aceec30d3e7dead2f71bb68a887688_203_0.png)

Figure 8.26 – Azure Storage

The Microsoft Azure Storage services REST API provides programmatic access to blob, queue, table, and file services in your Azure or development environment through a storage emulator. You can see more details about these APIs at this link: https://learn.microsoft.com/en-us/rest/api/storageservices/.

### Summary

In this chapter, we presented the different storage options that are available in Azure Storage services. We started by creating an Azure Storage account, which includes different types of storage. We explored Azure Table Storage, Azure Blob Storage, Azure Disk Storage, and, in the end, Azure Files.

In the next chapter, we will cover designing and implementing cloud-native applications using Microsoft Azure Cosmos DB.

### Further reading

For more information, you can check out the Microsoft documentation at this link: https://learn.microsoft.com/en-us/azure/storage/. If you need to follow an example using Azure Storage with .NET library, use this link: https://learn.microsoft.com/en-us/azure/storage/common/storage-samples-dotnet.

### Questions

1. What are the common uses of file storage?
2. When do you use Azure files versus blobs?

## Working with Azure Cosmos DB to Manage Database Services

We store data in relational tables if we are using relational databases. Unstructured data can be stored in a storage solution, but sometimes, the structure of a relational database can be too rigid and generally leads to poor performance in certain cases, requiring the implementation of specific and detailed tuning. There are several models, known as NoSQL databases, that present solutions for certain scenarios, such as documents, graphs, column family stores, and key-value stores.

NoSQL databases are defined by multiple characteristics: they are non-relational, have a JavaScript Object Notation (JSON) schema, and are designed for scaling out.

This chapter will cover designing and implementing cloud-native applications using a multi-model NoSQL database management system, which is Microsoft Azure Cosmos DB.

In this chapter, we’re going to cover the following main topics:

- NoSQL databases
- Exercise 1 – creating an Azure Cosmos DB account using the Azure portal
- Exploring the Azure Cosmos DB SQL API
- Exercise 2 – creating an Azure Cosmos DB SQL API account
- Exercise 3 – connecting to the Azure Cosmos DB SQL API with the SDK

### NoSQL databases

Organizations now handle more than structured data. NoSQL databases store unstructured data in documents rather than relational tables, where we store structured data. So, we categorize them as “more than just SQL” and decompose them into various flexible data models. NoSQL database types include document-only databases, key-value stores, wide-column databases, and graph databases.
