

## 探索 Azure 文件

Azure Files 存储提供基于云的文件共享，可以从云或本地访问。

Azure Files 存储为使用行业标准的 **Server Message Block (SMB)** 协议和 **Network File System (NFS)** 协议的应用程序提供共享存储。Azure 虚拟机和云服务可以通过挂载的共享在应用程序组件之间共享文件数据。本地应用程序也可以在共享存储库下访问文件数据。这意味着多个虚拟机可以同时以读写权限共享相同的文件。您也可以使用 **Representational State Transfer (REST)** API 接口或存储客户端库读取文件。在 Azure 虚拟机、Windows、Linux 或其他云服务中运行的应用程序可以挂载文件存储共享以访问文件数据，就像应用程序挂载典型的 SMB 或 NFS 共享一样。任意数量的 Azure 虚拟机或角色可以同时挂载并访问文件存储共享。Azure Files SMB 文件共享可从 Windows、Linux 和 macOS 客户端访问。NFS 文件共享可从 Linux 和 macOS 客户端访问。

### 文件存储的常见用途

Azure Files 可用于完全替代传统的本地文件服务器或 **Network Attached Storage (NAS)** 设备。流行的操作系统，如 Windows、macOS 和 Linux，可以在任何位置直接挂载 Azure 文件共享。Azure Files 使将期望文件共享来存储文件应用程序或用户数据的应用程序轻松迁移到云变得简单。Azure 文件共享还可以通过 Azure File Sync 与本地或云上的 Windows 服务器进行复制。为了确保性能并在数据使用位置进行分布式缓存，共享应用程序设置例如存储在配置文件中，以及诊断数据如日志、指标和崩溃转储到共享位置。我们存储开发或管理 Azure 虚拟机或云服务所需的工具和实用程序。这涵盖了 Azure Files 文件存储的所有常见用例。

### 在 Azure 门户中添加 Azure Files

现在，我们将了解如何在 Azure 门户中添加 Azure Files：

1. 返回 **Storage browser** 并选择 **File shares**：

### 存储账户指标

提供的数据大约每天更新 2-4 次并每小时发布一次。如果您的账户有极大的对象，更新之间可能超过一天。

Figure 8.24 – Storage browser | File shares

2. 选择 Add file share 并添加名称。选择 Tier，然后选择 Transaction optimized 或 Hot 或 Cool：

   - **Transaction optimized** 用于启用事务繁重的负载。我们将其用于需要文件存储作为后端存储的应用程序。
   - **Hot** 是某些文件共享场景的优化选项；例如，如果团队共享文件，Azure Files 将同步它们。
   - **Cool** 用于归档存储场景。

创建后，我们可以探索不同的设置，如以下屏幕截图所示：

![](img/66aceec30d3e7dead2f71bb68a887688_202_0.png)

Figure 8.25 – Azure Files settings

3. 如果我们选择 **Connect**，我们可以配置从 Windows、Linux 或 macOS 连接到 Azure 文件共享的连接。为了保护连接，选择 **Active Directory** 或 **Storage account key** 用于 Windows。我们可以将 **Tier** 从 **Transaction optimized** 更改为 **Cold**，例如，或从 **Cold** 更改为 **Hot**。

有时，很难决定在解决方案中何时使用 Azure 文件共享而不是 blob 或磁盘共享。如果我们比较不同的功能，我们可以决定哪个解决方案合适。如果我们将 Azure Files 与 Azure Blob 并排比较，我们会看到 Azure Files 提供 SMB 和 NFS 接口、客户端库和 REST 接口，允许从任何地方访问以存储文件。Azure Blob 提供客户端库和 REST 接口，允许以扁平命名空间存储和访问无结构数据，并在 blob 中大规模访问。

### 何时使用 Azure 文件与 blob

您可以使用 Azure 文件将应用程序迁移到云，该应用程序已使用原生文件系统 API 在其与在 Azure 中运行的其他应用程序之间共享数据。您希望存储开发和调试工具，这些工具需要从许多虚拟机访问。另一方面，Azure blob 可用于如果您希望应用程序支持流式传输和随机访问场景，并且希望能够从任何地方访问应用程序数据。还有一些其他区分特性决定何时选择 Azure 文件而不是 Azure blob。Azure 文件是真正的目录对象，而 Azure blob 是扁平命名空间。Azure 文件可以通过文件共享访问。然而，Azure blob 通过容器访问。Azure 文件提供跨多个虚拟机的共享访问，而 Azure 磁盘专用于单个虚拟机。

在选择使用哪个存储功能时，您还应考虑定价并进行成本效益分析。

Azure Storage 可通过 REST API 访问，并为 Microsoft Azure 虚拟机设计，如下图所示：

![](img/66aceec30d3e7dead2f71bb68a887688_203_0.png)

Figure 8.26 – Azure Storage

Microsoft Azure Storage 服务 REST API 通过存储模拟器提供对 blob、队列、表和文件服务的编程访问，位于您的 Azure 或开发环境中。您可以在此链接查看有关这些 API 的更多详细信息：https://learn.microsoft.com/en-us/rest/api/storageservices/。

### 总结

在本章中，我们介绍了 Azure Storage 服务中可用的不同存储选项。我们首先创建了一个 Azure 存储账户，其中包含不同类型的存储。我们探索了 Azure Table Storage、Azure Blob Storage、Azure Disk Storage，最后是 Azure Files。

在下一章中，我们将介绍使用 Microsoft Azure Cosmos DB 设计和实现云原生应用程序。

### 进一步阅读

如需更多信息，您可以查看此链接的 Microsoft 文档：https://learn.microsoft.com/en-us/azure/storage/。如果您需要使用 .NET 库通过示例使用 Azure Storage，请使用此链接：https://learn.microsoft.com/en-us/azure/storage/common/storage-samples-dotnet。

### 问题

1. 文件存储的常见用途是什么？
2. 何时使用 Azure 文件与 blob？

## 使用 Azure Cosmos DB 管理数据库服务

如果我们使用关系数据库，我们将数据存储在关系表中。无结构数据可以存储在存储解决方案中，但有时关系数据库的结构可能过于僵化，通常在某些情况下导致性能不佳，需要实施特定的详细调优。有几种模型，称为 NoSQL 数据库，为某些场景提供解决方案，例如文档、图形、列族存储和键值存储。

NoSQL 数据库由多个特征定义：它们是非关系型，具有 JavaScript Object Notation (JSON) 模式，并设计用于扩展。

本章将介绍使用多模型 NoSQL 数据库管理系统 Microsoft Azure Cosmos DB 设计和实现云原生应用程序。

在本章中，我们将涵盖以下主要主题：

- NoSQL 数据库
- Exercise 1 – 使用 Azure 门户创建 Azure Cosmos DB 账户
- 探索 Azure Cosmos DB SQL API
- Exercise 2 – 创建 Azure Cosmos DB SQL API 账户
- Exercise 3 – 使用 SDK 连接到 Azure Cosmos DB SQL API

### NoSQL 数据库

组织现在处理的不只是结构化数据。NoSQL 数据库将无结构数据存储在文档中而不是关系表中，我们在其中存储结构化数据。因此，我们将它们归类为“不仅仅是 SQL”并将其分解为各种灵活的数据模型。NoSQL 数据库类型包括仅文档数据库、键值存储、宽列数据库和图数据库。

