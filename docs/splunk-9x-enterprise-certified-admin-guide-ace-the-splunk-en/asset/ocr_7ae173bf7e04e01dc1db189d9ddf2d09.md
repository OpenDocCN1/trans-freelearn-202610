In addition to the cluster of search head instances and the captaincy functionality, the provided setup includes a separate instance called the **search head deployer**. The search head deployer serves as a centralized hub for deployment apps that are pushed out to the search head cluster when needed by administrators.

The search head deployer instance contains the deployment apps, which are configurations, settings, and customizations that need to be distributed across the search head cluster. These apps may include saved searches, dashboards, reports, field extractions, and other objects that enhance the functionality and customization of the search head instances.

Administrators can make changes to the deployment apps on the search head deployer and then push those changes to the search head cluster. This centralized approach simplifies the management and distribution of configurations across multiple search head instances, ensuring consistency in deploying and maintaining apps.

# Indexer clustering

An indexer cluster contains a group of Splunk Enterprise instances that are configured to work as indexers (search peers) and they all share the same index configurations and apps. A separate instance is required to manage the indexer cluster activities; that is called a **cluster manager**.

An indexer cluster is required to meet the demands of high availability, fault tolerance, and scalability when dealing with substantial data volumes and concurrent search queries. By utilizing an indexer cluster, organizations can guarantee uninterrupted data access, mitigate the effects of indexer failure, and achieve optimized data indexing and search performance.

Indexer clusters provide several key benefits:

+   - **Data availability**: By distributing and replicating data across multiple indexers, an indexer cluster ensures that data remains accessible even if one indexer goes down or becomes unavailable.
+   - **Data recovery**: In the event of an indexer failure, the cluster retains copies of indexed data on other indexers. This redundancy enables automatic retrieval of the relevant data from the remaining indexers when a search request is issued.
+   - **Data fidelity**: Indexer clusters maintain data fidelity by replicating and distributing data across multiple indexers. This replication strategy safeguards data integrity and consistency, reducing the risk of data loss and ensuring that indexed data remains accurate and reliable.

Let’s understand how searching works in the context of indexers. When a search query is issued by a user, the search head distributes the query to the indexers in the cluster. Each indexer performs a local search on its searchable copy of the data that resides locally in its storage. The search head then aggregates and merges the results from all indexers to generate the final search result set.