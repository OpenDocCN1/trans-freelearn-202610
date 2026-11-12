Standard storage accounts are backed by magnetic drives and provide the lowest cost per GB. They're best for applications that require bulk storage or where data is accessed infrequently. Premium storage accounts are backed by solid state drives and offer consistent, low-latency performance. They can only be used with Azure virtual machine disks, and are best for I/O-intensive applications, like databases. Learn more about storage account performance.

Performance Standard Premium This setting cannot be changed after the storage account is created.

# Figure 8.3 – Storage account type setting

You must create a new storage account of the performance type you need and then move the data to the created storage account.

This section looked at storage types, accounts, redundancy, and access. In the next section, we will look at implementing Azure Files.

# Implementing Azure Files

The most common use case for Azure Files is to provide a replacement for on-premises file servers in a lift-and-shift manner to an Azure scenario. This will require you to provide access to network file shares via **Server Message Block (SMB)**, **Network File System** (NFS), and **HTTP(S)** protocols. Client connections can be made by *Windows*, *Linux*, and *macOS* devices when the appropriate connectivity and access permissions are implemented.

Azure Files can also be used for backup and disaster recovery scenarios for the on-premise locations of file servers . Azure File Sync can be used to replicate file shares from on-premise file servers, providing a cloud tiering capability.

Azure Files is based on a serverless deployment, meaning there are no file servers to manage; this layer has been abstracted so that you can focus on creating shares and managing their access.

Azure Files uses the standard SMB 3.x protocol and can provide granular file permissions with support for NTFS when domain services are used to provide access. Data redundancy, performance tiering, encryption, versioning, backup, malware protection through Microsoft Defender for Storage, public and private endpoint access, and monitoring can all be configured as part of the service.

Azure Files supports a maximum file size of 4 TB and a maximum file share size of 100 TB (at the time of writing).