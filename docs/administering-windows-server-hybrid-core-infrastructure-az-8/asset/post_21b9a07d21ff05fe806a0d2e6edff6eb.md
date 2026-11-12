
In addition, Read Access GRS (RA-GRS) and Read Access GZRS (RA-GZRS) are two other storage types that are available that support read access to the data in the secondary region; this can support a use case where you want to serve a copy of the data in a region closer to users, while the primary region is up. Note that not all storage types are available in all regions.

## Storage encryption

Encrypting data at rest in a storage account is provided by all Azure Storage services and is enabled by default. As the data is written, it is automatically encrypted and transparently decrypted when accessed. By default, Microsoft-managed keys (MMK) are used. This can be seen in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_295_0.png)

Figure 8.5 – Storage encryption type

As an alternative, customer-managed keys (CMK) can be used to encrypt the data; they can be stored in an Azure Key Vault as the key store. All storage accounts have encryption-in-transit enabled by default, as shown here:

![](img/cf35f46a8dd5f11e58349c1d81611b75_295_1.png)

Figure 8.6 – Encryption-in-transit setting

In the default configuration of Secure transfer required being Enabled, HTTP connections are not allowed and will be rejected. Connections will fail if encryption is disabled, even in the case of SMB 3.0.

## Storage protection

The following settings are enabled by default when creating a storage account:

- Soft delete for blobs: Retained for 7 days
- Soft delete for containers: Retained for 7 days
- Soft delete for file shares: Retained for 7 days

These options can be seen in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_296_0.png)

In addition, snapshots can be taken of file shares for general backup operations. Snapshots are incremental, and a share can contain up to 200.

## Storage network access

When a storage account is created, the default networking configuration allows public access from all networks. You can change this to one of the following:

- Enable public access from selected virtual networks and IP addresses
- Disable public access and use private access

These options are shown in the following screenshot:

## Network connectivity

You can connect to your storage account either publicly, via public IP addresses or service endpoints, or privately, using a private endpoint.

- Network access *
  - Enable public access from all networks
  - Enable public access from selected virtual networks and IP addresses
  - Disable public access and use private access

Enabling public access from all networks might make this resource available publicly. Unless public access is required, we recommend using a more restricted access type. Learn more

Figure 8.8 – Storage network access options

When you select the **Disable public access and use private access** option, you will be presented with the option to add a private endpoint. A private endpoint assigns a private IP to the storage account from the VNet. This can be seen in the following screenshot:

Disable public access and use private access

## Private endpoint

Create a private endpoint to allow a private connection to this resource. Additional private endpoint connections can be created within the storage account or private link center.

- Add private endpoint

| Name | Subscription | Resource g... | Region | Target sub-... | Subnet | Private DN... | Private |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |

Click on add to create a private endpoint

Figure 8.9 – Private endpoint

Once a private endpoint has been added, the endpoint for the storage account is only accessible from the VNet.

## Storage authentication

Access to Azure Files must be authenticated; there is no support for anonymous access. The supported authentication methods for Azure Files are **identity-based**, **access key**, and **shared access signature (SAS)**.

## Identity-based

This access method provides Kerberos authentication using identities from Active Directory, Azure AD Domain Services, and Azure AD Kerberos. These options can be seen in the following screenshot:

Choose the Active Directory source that contains the user accounts that will access a share in this storage account. You can set up identity-based access control for user accounts located in either one of these three domain services.

- Active Directory domain controller you host on a Windows Server (generally referred to as “on-premises AD” even though you might host these servers in Azure)
- Azure Active Directory Domain Services (Azure AD DS), a platform as a service, hosted directory service and domain controller in Azure
- Azure AD Kerberos allows using Kerberos authentication from Azure AD-joined clients. In order to use Azure AD Kerberos, user accounts must be hybrid identities.

![](img/cf35f46a8dd5f11e58349c1d81611b75_298_0.png)

Figure 8.10 – Identity-based access

At the time of writing, cloud-only identities – that is, those that solely exist in Azure AD – are not supported. User accounts must be hybrid identities. Azure AD is a directory service only; it provides no direct domain controller functionality.

## Access key

This static authentication method uses a key, not a password. It provides the owner full access to the file shares and bypasses all access control. This can be seen in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_299_0.png)

Figure 8.11 – Access keys

This access should be controlled and governed. It shouldn't be used by users, only by service owners and admins.

## Shared access signature (SAS)

SAS provides a Uniform Resource Identifier (URI) that is generated dynamically. It provides access based on the storage key and can restrict access based on IP addresses, allowed protocols, permissions, and a set expiry time. These options can be seen in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_300_0.png)

Figure 8.12 – SAS

The SAS method use case is for API access.

## File share permissions

Role-based access control (RBAC) can manage file share permissions when configuring identity-based authentication. The following are the Azure File Share-specific built-in roles that can be assigned to users:

- **Storage File Data SMB Share Contributor**: This role allows users to read, write, and delete access to file shares
- **Storage File Data SMB Share Elevated Contributor**: This role allows users to read, write, delete, and modify NTFS permissions access for file shares
- **Storage File Data SMB Share Reader**: This role only gives users read access to file shares

These roles can be seen in the following screenshot:

| Name | Description |
|---|---|
| Storage File Data SMB Share Contributor | Allows for read, write, and delete access in Azure Storage file shares over SMB |
| Storage File Data SMB Share Elevated Cont... | Allows for read, write, delete and modify NTFS permission access in Azure Storage file shares over SMB |
| Storage File Data SMB Share Reader | Allows for read access to Azure File Share over SMB |

Figure 8.13 – File share roles

Once users have access to a file share, NTFS permissions control the next level of access to folders and files, the same as any on-premise SMB file share.

In this section, we concluded the topic of implementing Azure Files. In the next section, we will look at implementing Azure File Sync.

## Introduction to Azure File Sync

Azure File Sync uses Azure Files to keep file shares centralized in Azure but retains on-premise file servers to maintain existing operations, compatibility, and performance. Azure file shares can also be cached locally to on-premise file servers so that they are closest to the location where they are required. The file share data is still accessible locally via SMB, NFS, and File Transfer Protocol (FTP).

Azure File Sync is comprised of the following components:

- **Storage Sync Service**: This is the service resource that runs in Azure
- **Sync group**: A sync group contains endpoints that are to be kept in sync with each other
- **Registered server**: This is a trust relationship object between the Storage Sync Service and the server to sync to Azure
- **Azure File Sync agent**: This is a downloadable package that provides the sync process to Azure
- **Server endpoint**: This represents the folder or volume to sync on Windows Server – that is, the source of the sync
- **Cloud endpoint**: This represents the Azure file share, which is part of the sync group. It is the target for the sync

The use cases and capabilities of Azure File Sync for on-premise file servers are as follows:

- **Multi-site sync**: Multi-master replication is used to sync file shares across multiple sites.
