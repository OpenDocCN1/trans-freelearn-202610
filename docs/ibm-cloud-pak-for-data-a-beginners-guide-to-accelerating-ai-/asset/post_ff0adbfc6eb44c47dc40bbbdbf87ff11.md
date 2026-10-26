
Now that we have covered enterprise data architecture, let's move on to NoSQL data stores.

## NoSQL data stores – key categories

NoSQL data stores can be broadly classified into four categories based on their architecture and usage. They are as follows:

- **Document databases** map each key with a complex data structure, called a document. Documents can be key-array pairs, key-value pairs, or even nested documents. Some of the most popular choices for document databases include MongoDB, CouchDB, Raven DB, IBM Domino, and MarkLogic.
- **Key-value stores** are the *simplest* form of NoSQL database. Every row is stored as a key-value pair. Popular examples include Redis, Memcached, and Riak.
- **Wide-column stores**, also known as columnar data stores, are optimized for making queries over large datasets, as well as to store columns of data together instead of as rows. Examples include Cassandra and HBase.
- **Graph stores** store information about graphs; networks, such as social connections; road maps; and transport links. Neo4j is the most popular graph store.

Next, we'll review some of the capabilities of Cloud Pak for Data that enable it to address some of the challenges and one of them is Data virtualization.

## Data virtualization – accessing data anywhere

Historically, enterprises have consolidated data from multiple sources into central data stores, such as *data marts*, *data warehouses*, and *data lakes*, for analysis. While this is still very relevant for certain use cases, the time, money, and resources required make it prohibitive to scale every time a business user or data scientist needs new data. Extracting, transforming, and consolidating data is resource-intensive, expensive, and time-consuming and can be avoided through data virtualization.

**Data virtualization** enables users to tap into data at the source, removing complexity and the manual processes of data governance and security, as well as incremental storage requirements. This also helps simplify application development and infuses agility. **Extract, Transform, and Load (ETL)**, on the other hand, is helpful for complex transformational processes and nicely complements data virtualization, which allows users to bypass many of the early rounds of data movement and transformation, thus providing an integrated, business-friendly view in near real time.

The following diagram shows how data virtualization connects data sources and data consumers, thereby enabling a single pane of glass to access distributed datasets across the enterprise:

![](img/50e1dac0750b69f2b8e8efff000eb13b_66_0.png)

Figure 3.8 – Data virtualization – managing all your data, regardless of where it resides

There are very few vendors that offer data virtualization. According to Tech Target, the top five vendors in the market for data virtualization are as follows:

- Actifio Sky
- Denodo
- IBM Cloud Pak for Data
- Informatica PowerCenter
- Tibco Data Virtualization

Among these, IBM stands out for its integrated approach and scale. Its focus on a unified experience of bringing data management, data governance, and data analysis into a single platform resonates with today's enterprise needs, while its unique IP enables IBM's data virtualization to scale both horizontally and vertically. Among other things, IBM leverages push-down optimization to tap into the resources of the data sources, enabling it to scale without constraints.

A common scenario in distributed data systems is that many databases store data in a common schema. For example, you may have multiple databases storing sales data or transactional data, each for a set of tenants or a region. Data virtualization in Cloud Pak for Data can automatically detect common schemas across systems and allow them to appear as a single schema in data virtualization – a process known as **schema folding**. For example, a SALES table that exists in each of the 20 databases can now appear as a single SALES table and can be queried through **Structured Query Language (SQL)** as one virtual table.

## Data virtualization versus ETL – when to use what?

Historically, data warehouses and data lakes are built by moving data in bulk using ETL. One of the leading ETL products in the market happens to be from IBM and is called IBM DataStage. So, it begs the question as to when someone should use data virtualization versus an ETL offering. The answer depends on the use case. If the intent is to explore and analyze small sets of data in real time and where data can change every few minutes or hours, data virtualization is recommended. Please note that the reference to small sets of data alludes to the actual data that's transferred, not the dataset that a query is performed on. On the flip side, if the use case requires processing huge datasets across multiple sources and where data is more or less static over time (historical datasets), an ETL-based solution is highly recommended.

## Platform connections – streamlining data connectivity

Cloud Pak for Data also includes the concept of **platform connections**, which enable enterprises to define data source connections universally. These can then be shared by all the different services in the platform. This not only simplifies administration but enables users across different services to easily find and connect to the data sources. Cloud Pak for Data also offers flexibility to define connections at the service level to override platform connections. Finally, administrators can opt to either set user credentials for a given connection or force individual users to enter their respective credentials. The following is a screenshot of the platform connections capability of **Cloud Pak for Data v4.0**, which was released on June 23, 2021:

While tapping into data without moving it is a great starting point, Cloud Pak for Data also enables customers to persist data on its platform. You can easily deploy, provision, and scale a variety of data stores on Cloud Pak for Data. The supported databases on Cloud Pak for Data are as follows:

- Db2 (both OLTP and analytical workloads)
- Db2 Event Store
- MongoDB
- Enterprise DB PostgreSQL
- Cockroach DB
- Crunchy DB PostgreSQL
- Support for object storage – separation of compute and storage

Cloud Pak for Data enjoys a vibrant and open ecosystem, and more data sources are scheduled to be onboarded over the next 1-2 years. This offers customers the freedom to embrace the data stores of their choice while continuing to tap into their existing data landscape. Finally, it's worth mentioning that all the data stores that are available on the platform are containerized and cloud-native by design, allowing customers to easily provision, upgrade, scale, and manage their data stores. Also, customers can deploy these data stores on any private or public cloud, which enables portability. This is critical in situations where data gravity and the co-location of data and analytics is critical.

## Summary

Data is the world's most valuable resource and to be successful, enterprises need to become data-centric and leverage their data as a key differentiator. However, increasing data volumes, evolving data stores, and distributed datasets are making it difficult for enterprises and business users to easily find and access the data they need. Today's enterprise data architecture is fairly complex, with a plethora of data stores optimized for specific workloads. Data virtualization offers the silver bullet to address this unique challenge, and IBM's Cloud Pak for Data is one of the key vendors in the market today. It is differentiated for its ability to scale and its integrated approach, which addresses data management, data organization, and data analysis requirements.

Finally, IBM's Cloud Pak for Data complements its data virtualization service with several containerized data stores that allow customers to persist data on the platform, while also allowing them to access their existing data without moving it.

In the next chapter, you will learn how to create a trusted analytics foundation and organize the data you collected across your data stores.
