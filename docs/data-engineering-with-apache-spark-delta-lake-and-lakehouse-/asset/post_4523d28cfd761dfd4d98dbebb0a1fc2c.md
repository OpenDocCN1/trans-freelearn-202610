
17. Now, click on Datasets. Notice that the data share is not mapped to a storage account on the recipient's side:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_367_0.png)

Figure 10.42 – Checking the received shares in the consumer's subscription

18. Click on Map to target and supply the following values:

- Target data type: Azure data lake storage Gen2
- Subscription: Free Tier or Paid Subscription
- Resource group: training_rg
- Storage accounts: <Consumers Storage Account>

The consumer is free to choose any storage account in their subscription to receive the shared data.

- Path: goldshared:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_368_0.png)

Figure 10.43 – Mapping datasets to the target in the consumer's subscription

19. Click on Map to target. The confirmation screen should look like this:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_368_1.png)

Figure 10.44 – Confirmation of data mapping in the consumer's subscription

20. Now, we are all set to **trigger** a full snapshot, which will copy the shared data from the **provider** account to the **consumer** storage account.

Before we trigger a **full snapshot**, we need to grant a role to the storage account.

Using a new browser tab, navigate to the Azure portal.

Click on **Home > All Resources > <Consumers Storage Account>**. 

Then, using the menu on the left, click on **Access Control (IAM)**.

From here, click on **Add role assignment**.

Now, choose **Owner** and click on **Next**.

Choose **User, group, or service principal**, and then click on **Select members**.

Choose the Azure **principal** where the name starts with the same account name that the user logs into the Azure portal with. Click **Select** and then **Next**:

# Add role assignment

Role
Members
Review + assign

**Selected role**
Owner

**Assign access to**
● User, group, or service principal
○ Managed identity

**Members**
+ Select members

| Name | Object ID | Type |
|------|-----------|------|
| [redacted] | 39ea52c6-0ad8-4736-b35a-da3222e579... | User |

Figure 10.45 – Adding a role assignment to the consumer's subscription

Finally, click **Review + assign**.

21. Navigate back to the Data share tab. Click on Trigger snapshot and choose Full copy:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_370_0.png)

Figure 10.46 – Triggering a new snapshot in the consumer's subscription

22. This will trigger a full snapshot copy from the provider account to the consumer account. Notice the messages in the notification area. This area should display the status of the snapshot being copied:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_370_1.png)

Figure 10.47 – Checking notifications for snapshot creation in the consumer's subscription

23. After a few minutes, navigate back to the storage account. You should see a new container called goldshared in the storage account, as follows:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_371_0.png)

Figure 10.48 – Checking the new snapshot in the consumer's subscription

24. Now, if you click on goldshared, you should see the following folders:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_371_1.png)

Figure 10.49 – Checking the contents of the data share in the consumer's subscription

25. Now, click on any folder and check that the files have been successfully copied, as follows:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_372_0.png)

Figure 10.50 – Checking the blobs in the consumer's subscription

Here, we were able to successfully share data between the provider and the consumer with simplicity, security, and accountability. Next, we will look at another critical challenge that's faced by organizations dealing with sensitive data.

## Data governance

I started this book by stating "Every byte of data has a story to tell. The real question is whether the story is being narrated accurately, securely, and efficiently." While organizations are busy harnessing the true power of data, data governance and security frequently get neglected. But it cannot be neglected for too long. Regulations such as GDPR and many others are enforcing legal accountability and strict penalties on organizations failing to meet governance policies related to data privacy, retention, and portability.

An effective path to data governance often starts with an effective method for data discovery, classification, and tracking lineage. This can be a daunting task, considering the vast variety, volumes, and velocity of data. Azure Purview is a new, unified governance service that automates tasks such as discovery, classification, and lineage.

Using Azure Purview, the registered data sources can be automatically discovered once or regularly on a schedule. The discovered data is classified based on predefined or custom rules. Once the data has been cataloged, users can manually enhance the metadata by providing descriptions, tags, and access rules. Let's explore how Azure Purview can effectively handle this.

## Preparing the Azure resources

We will start by preparing the Azure Purview account:

- 1. On the Azure portal search, for `Purview accounts` and navigate to it.
- 2. On the **Purview accounts** page, click on **Create Purview account** and provide the following values:
  - **Subscription:** `Free Tier or Paid Subscription`
  - **Resource group:** `training_rg`
  - **Purview account name:** `trainingcatalog`
  - **Location:** `East US 2:`

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_373_0.png)

Figure 10.51 – Creating a new Purview account

Click on Review + Create:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_374_0.png)

Figure 10.52 – Specifying the parameters for the new Purview account

Click on Create. Once the deployment has finished, click on Go to Resource:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_374_1.png)

Figure 10.53 – Home screen of the new Purview account

Now that we have an Azure Purview account, we are ready to create the data catalog.

## Creating a data catalog

A data catalog is a collection of data assets and their relationships. It promotes effective governance by enabling end users to discover relevant data using terms, classifications, and sensitivity labels. Follow these steps to create one:

- 1. Click on **Open Purview Studio:**

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_375_0.png)

Figure 10.54 – Purview Studio home screen for trainingcatalog

2. To register a data source, click on Register Sources, then Register:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_376_0.png)

Figure 10.55 – Choosing Azure Data Lake Storage Gen2 in trainingcatalog

Select Azure Data Lake Storage Gen2.

Click Continue and provide the following values:

- Name: trainingsource
- Subscription: Free Tier or Paid Subscription
- Storage account name: traininglakehouse
- Endpoint: Automatically populated based on previous choices

- Select a collection: None:

# Register sources (Azure Data Lake Storage Gen2)

Name *
trainingsource

Azure subscription
Azure subscription 1 (0b35426c-85f3-45e8-9f96-577a13b738d5)

Storage account name *
traininglakehouse

Endpoint
https://traininglakehouse.dfs.core.windows.net/

Select a collection
None

Figure 10.56 – Registering a new source in trainingcatalog

3. Click on Register:

# Sources

Register   New collection   Refresh   Map view   Table view

Filter by keyword

Showing 0 collections, 1 source

trainingsource
Azure Data Lake Storage Gen2
View details

Figure 10.57 – Confirming the creation of trainingsource

4. We are now ready to scan the data source we have created. Before proceeding, we need to make sure that trainingsource has the appropriate permissions to scan traininglakehouse.

Using the Azure portal, navigate to Home > All resources > traininglakehouse.

5. Using the menu on the left, click on Access Control (IAM):

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_378_0.png)

Figure 10.58 – Access Control (IAM) for traininglakehouse

Click on + Add, and then Add role assignment. Provide the following values:

- Role: Storage Blob Data Reader
- Assign access to: User, group, or service principal
- Select: trainingcatalog:
