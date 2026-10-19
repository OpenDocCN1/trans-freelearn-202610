
![](img/dae8a2243a3c3cbcd9c56ff3d2217672_45_0.png)

Figure 2.4 – The three pillars of data security

During a brief period of my career, I was heavily focused on data security audits. I still remember something that one of the customers jokingly said over 15 years ago, as his words have resonated several times since. He held a sales report in his hand and said:

> "There will come a time when people will care less about the correctness of data but will care more that it was processed securely."

Quite honestly, the sentence did not make too much sense to me at the time, but today, I can fully relate to it.

Important note
For any organization that deals with data, security is no longer a luxury—it is a core requirement.

As per the CIA triad, the key pillars of security are integrity, confidentiality, and availability. Following these principles, security in a data lake should be implemented in layers: the greater number of layers, the more protection. Data lakes can comply with the core requirements of most compliance frameworks using the following techniques to cover the key pillars of the CIA triad:

- Perimeter security—Access to data lake resources can be limited using a combination of site-to-site virtual private networks (VPNs), filtering Internet Protocol (IP) addresses, and blocking access to the internet.
- Access control—User access in a data lake can be controlled by a combination of role-based access control (RBAC), policies, access control lists (ACLs), multi-factor authentication (MFA), and single-sign-on (SSO).
- **Encryption**—A key method of protecting **data-in-motion** (traveling from one point to another) and **data-at-rest** (in storage) is using encryption. Encryption can be implemented not only at the file level in the storage, but also any **personally identifiable information (PII)** data within a file can be encrypted.
- **Authorization**—Extreme caution is desired during data distribution. The task becomes even more complex if data distribution includes external users. A data lake can comply with authorization needs by implementing a role-based access mechanism (RBAC) using a combination of table-, row-, and column-based security and data masking.

> **Important note**
> The CIA triad is simply a guideline for an organization's security program.
> The strength of security is directly proportional to the number of layers implemented within it.

Next, we'll see how storage and compute can be segregated within a data lake.

## Segregating storage and compute in a data lake

We saved the best for last. The ability of a data lake to segregate (decouple) the storage and compute layers is perhaps one of its most desirable features. But why is decoupling storage and computation such a big deal? To understand why, we should first understand the storage and compute layers.

## Storage

The fundamental function of storage is to store data. Briefly, this is how storage has evolved over the last few years:

- **Direct-attached storage (DAS)**—As the name suggests, a storage device (**hard disk drive/solid-state drive**, or **HDD/SSD**) is physically attached to the computer. This type of storage mostly exists in personal computers and laptops.
- **Network-attached storage (NAS)**—Centralized storage using multiple hard drives connected as an array. NAS allows quick and easy sharing of data between computers using regular ethernet.
- **Storage area network (SAN)**—High-performance shared storage using fiber-optic connectivity. A SAN allows for the fast sharing of data between computers.
- **Cloud storage**—Cloud storage is a relatively recent addition to this list. In this case, the storage physically exists on the cloud-provider servers. Cloud storage allows users the flexibility to access it from anywhere, as well as an assurance that data is highly available and safe because multiple copies are maintained on the backend.

> **Important note**
> Azure Blob storage, Amazon Simple Storage Service (S3), and Google Cloud Storage are some of the best-known cloud storage solutions.

## Block storage

All storage types in the preceding list (except cloud storage) are categorized as file and block storage. This means data is stored on the storage device in small-sized blocks, usually 512 bytes in size. A group of contiguous blocks is deemed a file, and that is how humans relate to data.

## Object storage

Cloud storage is categorized as object storage. Object storage does not work like a traditional filesystem. Unlike block storage, every object contains the following:

- **Identifier (id)**—A globally unique identifier (GUID) for each object in storage
- **Data**—Stored as blobs all in one location (not broken into multiple blocks)
- **Metadata**—Data about the data, such as author, permissions, and timestamps

The differences between block and object storage are better illustrated as follows:

### Block Storage

### Object Storage

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_47_0.png)

Figure 2.5 – Block storage versus object storage

Object storage is quickly becoming the standard for data engineering and analytics operations due to certain key advantages, listed here:

- Object storage is highly scalable, which means the storage can grow as per the growing data demands within an organization.
- You save money because there is no requirement to book storage capacity in advance.
- The feature of embedding metadata within the object facilitates data classification and analytics workloads.
- Data can easily be accessed via **HyperText Transfer Protocol (HTTP)** or APIs.
- Very suitable for storing large files such as video, audio, images, and backups of data.
- Suitable for write-once, read-many-times scenarios such as data analytics.

However, there are a few drawbacks as well, as outlined here:

- Performance of object storage is slower compared to block storage, particularly writing.
- Data cannot be **appended** to objects; any change to data simply amounts to deleting the existing object and recreating it.
- Suitable for workloads that require relatively static data.

**Important note**
Since each change to an object amounts to deleting it and creating a new version, object storage is unsuitable for **online transaction processing (OLTP)** operations where data changes are very frequent. However, it is very suitable for **online analytical processing (OLAP)** operations because data changes infrequently.

## Compute

In the context of a data lake, compute is the ability to process (read and write) data for analytical operations such as wrangling, joining, and aggregating. In analytical computations, two resources are very critical for smooth operations, outlined as follows:

- **Central processing unit (CPU)**—On any given computer, all calculations are performed in the CPU. The speed of the CPU is measured in clock speed—2.8 **gigahertz (GHz)**, for example. This means that this CPU can perform 2.8 billion computations per second. The greater the CPU speed, the more the computational power. In the world of virtualization, this term is referred to as a **virtual CPU (vCPU)**.
- **Random-access memory (RAM)**—RAM stores active data that is required for any currently ongoing computations. This is done to prevent repeated read and write operations from hard drives, which are typically a lot slower (a ratio of 1:50) to respond compared to RAM. Therefore, it is generally preferred to possess as much RAM as possible.

## Containers

In analytical processing terms, resources are allocated to jobs in container units. The idea of a container was introduced in **Yet Another Resource Negotiator (YARN)**, the distributed processing framework in Hadoop.

In simple terms, a container is a logical grouping of the two key physical resources—a vCPU and RAM. Since YARN works in a cluster, it maintains the total tally of RAM and the vCPU across the cluster.

At processing time, YARN allocates containers based on the requirements of the job(s). If a cluster has enough resources to meet the demand of the job, it starts it immediately, else it puts it in wait mode until the resources free up. The different-sized containers are depicted in the following screenshot:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_49_0.png)

Figure 2.6 – Varying container sizes
