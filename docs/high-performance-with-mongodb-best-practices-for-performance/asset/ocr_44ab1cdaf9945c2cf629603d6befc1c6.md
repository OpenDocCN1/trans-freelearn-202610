# Understanding multi-document ACID transactions

Let’s dig into MongoDB’s multi-document ACID transactions, a capability that marked a significant milestone in MongoDB’s evolution from a basic document-oriented database to a platform capable of handling complex, mission-critical workloads. While MongoDB has always provided powerful document-level atomicity, multi-document transactions extend these guarantees across multiple documents, collections, and databases.

Even though many applications can utilize a rich document structure to ensure related writes happen together, there are still use cases that require the coordination of writes between multiple different documents that may be in the same or different collections. Because of this, it’s important to understand what MongoDB ACID multi-document transactions guarantee and how to best use them.

In this section, we’ll first explore the history and evolution of transaction support in MongoDB, from single-document atomicity to distributed transactions across sharded clusters. Then, we’ll break down the core ACID properties, that is, **atomicity**, **consistency**, **isolation**, and **durability**, and how they apply within MongoDB’s transaction model. Finally, we’ll compare document-level atomicity with multi-document transactions, helping you understand when each approach is appropriate and how to balance data integrity with performance.

# History and evolution of transactions in MongoDB

MongoDB’s journey with transaction support tells an interesting story of how the database has evolved to meet increasingly complex application requirements:

+   - **The single-document era (pre-MongoDB 4.0)**: From its inception, MongoDB provided atomicity at the single-document level. This was sufficient for many use cases, especially when leveraging MongoDB’s flexible document model to embed related data within a single document. Developers would often design schemas specifically to take advantage of this single-document atomicity.
+   - **The replica set milestone (MongoDB 4.0, 2018)**: This release introduced multi-document ACID transactions for replica sets, a game-changer that brought MongoDB into parity with relational databases for applications requiring strong consistency across multiple documents. This initial implementation supported transactions within a single replica set.
+   - **The distributed transactions breakthrough (MongoDB 4.2, 2019)**: Building on the previous release, MongoDB 4.2 extended multi-document transaction support to sharded clusters. This significant advancement enabled distributed transactions across shards, providing a comprehensive solution for complex, large-scale applications.