Azure Purview supports a variety of data sources, including Azure Blob, Azure Synapse, Hive, MySQL, Oracle, and many more.

# Azure Data Share

In Chapter 1, The Story of Data Engineering and Analytics, we had a section called The monetary power of data. In that section, we talked about modern methods of revenue diversification, including data monetization through sharing data. An organization can realize this dream by using Azure Data Share. Azure Data Share facilitates securely sharing data to other parties, including customers and partners. Traditionally, data sharing securely using mediums such as SFTP or APIs has been very expensive.

Using Azure Data Share, organizations can add datasets and invite customers/partners to safely access your shared data. Azure Data Share can be performed in a couple of ways:

+   - **Snapshot-based sharing**: Once shared, data moves to the consumer's Azure subscription.
+   - **In-place sharing**: Once shared, a data link is created that can be accessed by the consumer.

Azure Data Share access is protected with managed identities using Azure Active Directory. In addition to that, data shares are protected by usual data-at-rest and data-in-motion mechanisms.

> **Important Note**
> For organizations that are migrating to the cloud, the sudden idea of adopting SaaS is sometimes quite far-fetched. Not everyone likes to lose control over the infrastructure and one-off customizations.

# Opening a free account with Microsoft Azure

In the upcoming chapters, we will be using the services you have just been reading about to build a data lake using the lakehouse architecture. Therefore, it is time to open a free Azure account that gives you 12 months of free services, plus a one-time $260 credit. Please note that not all – but most – services are free. To open a free account and browse through the free services, please visit the following link: https://azure.microsoft.com/en-ca/free/.