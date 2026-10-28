

结果将与以下图所示类似，其中所有服务依赖项的状态均为已连接：

![](img/66aceec30d3e7dead2f71bb68a887688_229_0.png)

图 9.25 – 连接的服务选项卡上的服务依赖项

应用程序部署在 Azure App Service 上，所使用的数据库是 Cosmos DB，如下图所示：

![](img/66aceec30d3e7dead2f71bb68a887688_229_1.png)

图 9.26 – 主机和服务依赖项

我们可以在此处使用 Microsoft.Azure.Cosmos 包和 CosmosClient 类。然而，如果你在数据层中使用 Entity Framework 作为对象关系映射器，则可以使用 Microsoft.EntityFrameworkCore.Cosmos 包。有关更多详细信息，你可以按照此链接操作：https://learn.microsoft.com/en-us/ef/core/providers/cosmos/?tabs=dotnet-core-cli。

## Summary

在本章中，我们学习了如何创建 Azure Cosmos DB 帐户，并使用数据资源管理器创建了数据库和容器。我们探索了不同的 API 模型，例如 Core（SQL），该模型推荐用于新应用程序、Azure Cosmos DB API for MongoDB、Cassandra、Azure Table Storage 和 Gremlin（Graph）。我们使用 Microsoft .NET SDK v3 创建了资源。我们使用端点和主密钥将应用程序连接到 Azure Cosmos DB 帐户。我们使用 CosmosClient 创建数据库、创建容器并从文档中检索数据。我们还将 Azure App Service 与 Azure Cosmos DB 连接起来。

在下一章中，我们将探索大数据存储。我们将定义 Azure Data Lake Storage 并学习如何创建更安全、高性能的数据分析框架。如果你需要有关 Cosmos DB 的更多信息，可以查看此链接：https://learn.microsoft.com/en-us/azure/cosmos-db/introduction。

## Questions

在创建容器之前，应首先创建哪个 Azure Cosmos DB SQL API 资源？

## Big Data Storage Overview

使用现代系统的公司会产生大量异构数据。这些数据必须被利用于营销目的或对产品进行内部改进。这种异构数据表明，单一数据存储通常不是最佳方法。

还建议将不同类型的数据存储在不同的数据存储中，以便每个数据存储针对特定的工作负载或使用模式。如果我们使用不同数据存储技术的组合，我们使用的是所谓的 polyglot persistence。了解 Azure 作为存储数据仓库的服务所提供的内容以及我们如何使用和分析所有这些数据非常重要。

在本章中，我们将探索大数据存储并定义 Azure Data Lake Storage 可扩展性、安全性和成本优化工作。你将学习如何创建更安全、高性能的数据分析框架。

在本章中，我们将涵盖以下主要主题：
- 探索 Azure Data Lake Storage
- 探索 Azure Data Factory
- 探索 Azure Databricks
- 探索 Azure Synapse Analytics
- 探索 Azure Analysis Services

### Exploring Azure Data Lake Storage

Azure Storage 包括五个 Azure 服务：
- Azure Blob Storage，即无结构数据的可扩展存储
- Azure Queue Storage，允许我们构建可靠的消息队列
- Azure Table Storage，提供对结构化数据存储的支持
- Azure Files Storage，用于基于云的文件共享
- Azure Disks Storage，提供虚拟机磁盘的托管存储

## Big Data Storage Overview

数据湖是一种文件存储形式，通常在分布式文件系统上以实现高性能数据访问。用于处理对存储文件的查询并返回用于报告和分析的数据的常用技术是 Spark 和 Hadoop。这些系统依赖于读取模式方法，该方法为半结构化数据文件定义了表格模式，其中数据在读取时被解析，而在保存时不施加限制。数据湖非常适合支持你想要分析的混合结构化、半结构化和非结构化数据，以避免在将数据写入存储时应用模式。

Azure Data Lake Gen2 技术是在 Azure Blob Storage 之上创建的，并支持大多数 Blob Storage 功能。与 Azure Data Lake 相关的更多好处，例如支持分层命名空间，这意味着我们可以以类似文件的方式存储数据。此好处提高了目录管理操作的性能，以提供对大规模分析引擎的更好支持。

在本节中，我们将开始探索 Azure Data Lake Storage。Azure Data Lake Storage 将高性能文件系统的强大功能与大规模存储平台相结合，让你能够快速洞察数据。

Azure Data Lake Storage Gen2 基于 Azure Blob Storage 功能构建，以优化它，特别是针对分析工作负载。如果你使用过 Hadoop 分布式文件系统，我们可以以相同的方式处理数据。借助此功能，你可以将数据存储在一个位置，并通过各种 Azure 计算服务（如 Azure Databricks、HDInsight 和 SQL Data Warehouse）访问它。Data Lake Storage Gen2 支持 ACL 和 POSIX 权限。我们可以为数据湖中存储的数据在目录或文件级别设置细粒度的权限级别。Azure Data Lake Storage 通过遵循目录和子目录的层次结构来组织存储的数据，如文件系统一样，便于导航，这提高了每个目录管理操作的性能。这种组织使我们能够为大规模分析引擎提供更好的支持，使用数据分区提高查询性能，并更好地进行文件和数据管理。

将数据存储在 Azure Data Lake Storage 中的成本与将数据存储在 Azure Blob Storage 中的成本相同。数据访问的交易成本略有增加，但在许多情况下，这些成本的增加必须与由于查询更高效而减少的交易需求相平衡。总体而言，Azure Data Lake Storage 允许你构建安全、可扩展的企业级数据湖。它在提供 Azure Blob Storage 的许多好处的同时，成本仅略有增加。

Blob 存储的一个良好用例是归档不经常使用的数据或存储网站资产，例如图像和媒体。Azure Data Lake Storage Gen2 在任何 Azure 数据架构中都发挥着基础作用。这些架构包括构建现代数据仓库、对大数据的先进分析以及实时分析解决方案。

对于所有架构而言，大数据解决方案的四个处理阶段是共同的：
1. 摄取阶段确定用于摄取源数据的技术和流程。此数据可以来自文件、日志和其他类型的非结构化数据，需要放置在 Data Lake Storage 中。
2. 存储阶段确定存储记录数据的地点。在本例中，我们使用 Azure Data Lake Storage Gen2。此阶段在下图中呈现：

![](img/66aceec30d3e7dead2f71bb68a887688_233_0.png)

图 10.1 – 摄取阶段 – Azure Data Lake Storage Gen2
3. 准备和训练阶段确定用于进行、准备和建模数据科学解决方案的训练和后续评分的技术。此阶段常用的技术有 Azure Databricks、Azure HDInsight 和 Azure Machine Learning。
4. 最后，模型和部署阶段包括向用户展示数据的技术。这包括可视化工具，如 Power BI，以及数据存储，如 Azure Synapse Analytics、Azure Cosmos DB、Azure SQL Database 和 Azure Analysis Services。根据业务需求，通常会使用这些技术的组合。

Azure Data Lake 中通用存储的常见文件格式包括 Excel、XML、JSON、二进制、Avro、分隔符、Optimized Row Columnar（ORC）和 Parquet。

我们将在以下小节中使用 Azure 门户和 Azure CLI 创建新的 Azure Data Lake 解决方案。

