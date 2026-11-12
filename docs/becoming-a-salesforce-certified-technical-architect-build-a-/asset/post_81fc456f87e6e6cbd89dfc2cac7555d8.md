
Behind the scenes, Salesforce stores its standard and custom objects using two separate tables and joins them on the fly whenever a user queries that object. One of these tables is used for standard fields, while the other is used for custom fields. Skinny tables combine both types of fields in one table. This removes the need for joining the two tables and, thus, makes skinny tables ideal for reporting. However, due to its nature, it cannot contain fields from more than one Salesforce object, and it cannot have more than 100 fields.

Salesforce will automatically switch between original tables and skinny tables when appropriate; this is not something you have control over.

The following diagram explains the anatomy of skinny tables:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_239_0.png)

Figure 7.2 – The anatomy of skinny tables

> **Note**
> You can learn more about skinny tables at the following link: https://pact.link/nKZKd.
> Make sure you fully understand the limitations and use cases of skinny tables.

It is worth mentioning that skinny tables do not contain soft-deleted records. Soft-deleted records are simply flagged as deleted but not completely removed from the platform yet and can be accessed and restored via the Salesforce Recycle Bin. Hard-deleting records of a specific object can reduce the LDV impact on that object, mainly when there is a massive set of soft-deleted records.

## Standard and Custom Indexes

Just like any database, you can run performant queries if you use indexes in your query criteria. Just like any other databases, **primary keys** (the object's ID field in Salesforce), **foreign keys** (the lookup and master/detail relationships in Salesforce), and **alternate keys** (unique external keys in Salesforce) are indexed by default.

> **Note**
> In addition, there is a set of standard indexed fields such as RecordTypeId, Division, CreatedDate, Systemmodstamp (LastModifiedDate), Name, and Email (for contacts and leads). You can find more details at the following link: https://pact.link/esgWV.

You can also request enabling custom indexes on some other fields. The queries that utilize indexed fields will perform much better as long as the query is returning a specific percentage of records out of the full dataset. Poor LDV query performance can still occur despite using indexed fields. In many cases, this is due to a misunderstanding of how the Salesforce **Query Optimizer** works. Make sure you fully understand the special cases where a query will not utilize the index. More details can be found at the previous link.

## Divisions

Divisions act like normal database partitions. They let you segment your Salesforce instance's data into multiple logical partitions. This effectively turns one massive dataset into a subset of smaller datasets, which makes searches, list views, and reports more performant. Divisions are not meant for security as they do not restrict access to data.

Divisions are typically done by geography. Once enabled, they impact several features in Salesforce, such as reports and list views.

> **Note**
> You can find out more details about divisions at the following link: https://pact.link/tkU62.

## Off-Platform Data Storage

One of the most effective and efficient mechanisms to handle LDVs is to analyze and reconsider whether a specific dataset needs to be stored on-platform (which is the term used to describe storing data on the Salesforce Core Cloud, or what was previously known as Force.com) or off-platform and retrieved on the fly when needed.

There are several mechanisms for fetching data on the fly from external data sources/applications and displaying it in Salesforce, including APIs and mashups.

Data can be stored on a cloud-based or on-premises-hosted platform. One of the platforms commonly used for this is Heroku. Heroku comes with an out-of-the-box connector to the Salesforce Core Cloud called **Heroku Connect**. This can be used to sync data between the two platforms without consuming any of the Salesforce org’s governance limits.

Other platforms include AWS, Microsoft Azure, Google Cloud, and Rackspace. Storing some data off-platform could take place as part of a data design decision or as part of a data archiving strategy, which will be explained in the next section.

## Data Archiving

Archiving is a very effective strategy used to handle LDVs. It relies on identifying data that does not necessarily need to be kept on-platform and can be moved to off-platform storage. Sometimes, you can even expose back the archived data using tools such as APIs or mashups. Salesforce also provides a tool that is compatible with the **OData** standard called **Salesforce Connect**. It allows you to expose external data tables as **Salesforce External Objects**. External Objects are similar to custom objects but with specific limitations and considerations.

> **Note**
> You can find out more details about External Objects at the following link: https://pact.link/zF5TQ.

Now, have a closer look at an example of using an off-platform data storage/archiving mitigation strategy. Assume that you have a Salesforce org with heavy use of the standard case object. Usually, more than 10 million cases are created every month. This qualifies the case object as an LDV. List views, reports, as well as other CRUD-related operations would all experience performance challenges accordingly. One way to mitigate the LDV impact would be to offload the data into another data storage platform (such as a PostgreSQL database on Heroku, but for this example, you can pick **Azure Data Lake**). The data offloading process involves the following steps:

- 1. Cases closed for more than a month are copied from Salesforce to Azure on a regular basis (an ETL batch job could do the trick). Copied data is then purged from Salesforce (and the Recycle Bin is emptied). This ensures that the case object remains slim. However, accessing the off-loaded/archived data is going to be a bit challenging and time-consuming for the users.

- 2. To enhance the user experience, you can develop a custom **Lightning component** (or **Visualforce page**) that invokes APIs developed on top of Azure to retrieve the stored data. Then embed that component into the standard case page layout. However, this means you need to develop a custom Lightning component and probably a set of customized APIs to retrieve the data from Azure in a flexible way, which is both time- and resource-consuming.

- 3. To speed things up, you can use an adapter (such as the one offered by CData.com) to expose the data stored in Azure as **OData** and then utilize **Salesforce Connect** to consume it. This will generate an **External Object** for the archived case object in the target Salesforce org. It could be named something such as `Case__x`. **External Objects** are easy to link with other Salesforce objects and can be easily added to page layouts with few clicks, thus saving time and enabling a faster functionality release.

> **Note**
> You can read more about the OData standard at the following link: https://pact.link/TU4YW.

To develop a solid archiving strategy, you need to get answers to the following key questions:

- How much data/records must be kept on-platform?
- What is the data retention period for the archived object?
- What other systems are involved in this strategy? Which system would the data flow to?
- Which objects will be archived, and how will that impact child/related objects?

You can archive the data into an off-platform storage container, such as Heroku, or use **Salesforce Big Objects**. Big Objects are designed to store billions of records on-platform. But as with any other technology, it has its strong and weak spots. The following summarized table can help you determine the right technology to use:

| | Salesforce Big Objects | Off-platform storage |
|---|---|---|
| Benefits | Store and manage billions of records<br>Relatively low-cost data storage<br>On-platform storage, no need for additional technologies<br>Fewer platforms to maintain, as it is part of the Salesforce Core Cloud | Store and manage billions of records<br>Data storage cost is usually low<br>You can select a target archiving platform that supports both data archiving and reporting<br>Several options to choose from, including self-hosted solutions<br>More suitable for larger organizations, particularly when a central data warehouse is used<br>Assuming the platform supports OData, the archived data could be exposed back to Salesforce using Salesforce Connect |
