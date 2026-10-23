
## AZs

Within each AWS region, you will find multiple AZs, which are metropolitan areas housing one or more data center facilities in each region. Each AZ will host hardware components such as servers, storage, and network equipment, all fitted with redundant power, connectivity, cooling, and security controls.

The primary purpose of having multiple AZs in each region is to enable customers to host their applications and workloads in a manner that offers high availability, fault tolerance, and scalability. With multiple AZs, you can host copies or replica application resources across these AZs, which ultimately means that you can continue to serve your customers even if there is an outage of one AZ in the given region.

This is all possible because, although each AZ operates independently, they are still connected over high-speed, high-bandwidth, low network latency, and fully redundant, dedicated metro fiber connectivity.

> **Note**
> Concerning our company, TodoPlus, their initial choice of region to host their application will be set to the US-East-1 (North Virginia) region. This region is selected because the company will start promoting its new productivity application in local markets. However, they plan to make the application available to customers across the US and later globally.

## Edge Locations and Regional Edge Caches

The AWS Global Infrastructure also comprises edge locations and Regional Edge Caches. **Edge locations** or **points of presence (POPs)** offer massive amounts of storage, high-bandwidth networking equipment, and edge computing services that enable data to be accessed, processed, and analyzed closer to the end customers’ physical location.

These edge locations are connected to AWS regions through the AWS backbone network. This comprises fully redundant, multiple 100-**Gigabit Ethernet (GbE)** parallel fiber connections that substantially improve throughput and offer low-latency connectivity. You can review the current list and types of edge locations (POPs) at https://aws.amazon.com/cloudfront/features/.

Edge locations are different from standard regions and AZs. You **cannot** connect directly to a given edge location to set up resources. Instead, you consume certain AWS services that use these edge locations’ storage, caching, and high network connectivity. One service that uses these edge locations is Amazon CloudFront.

Regional Edge Caches are like edge locations. However, they are strategically placed and have a larger storage capacity to hold cache data longer than individual edge locations. Individual edge locations have a shorter time-to-live than Regional Edge Caches, ensuring that stale data isn’t hosted too long. If the same cache data is later accessed, an attempt is made to see whether it is still available at regional edge caches before sending a request to the origin.

Amazon CloudFront is a **content delivery network (CDN)** service that enables you to efficiently distribute content to end users in a manner that reduces overall latency. With CloudFront, regularly accessed content is cached in the edge location and in regional edge caches, which offer the lowest latency to end users who attempt to access your content. This means those users do not have to fetch frequently accessed content from the origin if it resides in the cache.

> **Note**
> 
> In the case of our fictitious company, TodoPlus Limited, the application will be hosted in the N.Virginia (us-east-1) region. CloudFront can be used to cache static content such as images, videos, and user guides to help reduce the latency for end users as they access the application from various parts of the US and, ultimately, from across continents when our client expands its offering globally.

Edge locations can allow customers to upload data to AWS storage services such as Amazon S3 over the AWS backbone network, offering low latency and high-bandwidth throughput using a service known as **S3 Transfer Acceleration (S3TA)**.

The AWS Global Infrastructure also comprises other infrastructure services, including the following:

- **Local zones** are special zones designed to bring compute, storage, database, and other select AWS services closer to end customers’ physical locations. This is particularly useful if you require very low latency access to cloud services. Regarding TodoPlus Limited, this will not be necessary as its customer base is geographically dispersed across the US and potentially across the globe.
- **Wavelength zones** are zones where AWS has deployed infrastructure services such as compute and storage services within 5G network providers to help optimize mobile edge computing applications.
- **Direct Connect locations** are designed to establish high bandwidth network connections between clients’ data center facilities and the AWS cloud. TodoPlus Limited may wish to set up a Direct Connection to the AWS cloud. This will improve data transfer speeds between on-premises applications and the cloud due to the higher bandwidth capability.
- **Outposts** enable true hybrid cloud computing design by extending AWS infrastructure services, APIs, and tools to customers’ on-premises locations. If TodoPlus Limited plans to continue with a hybrid cloud model, an Outpost configuration will enable them to access AWS services locally, allowing low-latency access to certain applications that cannot be hosted in the cloud. If they have applications that need to follow strict compliance or regulatory requirements, then, again, an Outpost setup will help achieve this as all the data will be held locally on-premises.

This section examined the AWS Global Infrastructure and identified some of its core components. Understanding how the Global Infrastructure is architected will enable you to design applications for high availability, scalability, security, and cost-effectiveness.

In the next section, we will look at how you can access the vast array of AWS services via an AWS account as a customer.

## Overview of an AWS Account

AWS, a global public cloud provider, offers a comprehensive suite of infrastructure, platform, and software services. An AWS account is necessary to utilize these services, enabling customers to create a wide range of resources and host their applications on AWS.

Each customer needs to have a secure environment within which to create and manage IT resources; this is where the concept of an AWS account comes in. An AWS account, by its very nature, offers an **isolated resource container** that grants secure access to AWS services and enables customers to configure necessary resources to host their applications.

While setting up an AWS account, you must provide an email address and password. These credentials are used to create the primary owner, who has full control over the account. On AWS, we call this owner the **root user**. Every AWS account created will have its dedicated root user. This root user can perform all account and billing-related activities and close the account if no longer required. Each AWS account also provides a natural billing boundary for costs incurred in that account and is associated with a specified billing method. Overall, an AWS account offers a means to securely administer all your resources and is only accessible to you and any entity you choose to grant access to.

In the following section, you'll discover why many companies choose to set up multiple AWS accounts for handling diverse workloads. You'll also learn why this practice of a multi-account strategy is considered a best practice in the industry.

## When One AWS Account Is Not Enough

While hosting all your different applications, workloads, and environments in a single account is possible, this can easily become difficult to manage. Each application requires a different environment to support the various stages of its lifecycle, such as development, testing, and production. Hosting multiple environments for multiple applications in a single account is possible, but this can quickly turn complicated. This complexity arises for various reasons, including the need to ensure separation and isolation of workloads and environments for security reasons or budget and cost allocation.

Furthermore, hosting all your application workloads and environments in a single AWS account increases the risk of significant mishaps, which could wipe out all your applications and their different stages of development.
