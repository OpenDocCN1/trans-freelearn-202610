

2. 在 Basics 选项卡中，填写创建 SQL 弹性池所需的所有信息。在 Project details 下，选择订阅和资源组。在 Elastic pool details 下，我们将介绍此池的必需设置，选择一个逻辑服务器，然后配置计算和存储资源。我们有以下屏幕来添加弹性池名称和服务器：

## Elastic pool details

输入此池的必需设置，包括选择逻辑服务器和配置计算和存储资源。

![](img/66aceec30d3e7dead2f71bb68a887688_173_0.png)

图 7.15 – SQL 弹性池中的弹性池详细信息

3. 在 Compute + storage 中，我们需要配置弹性池以选择多个数据库。点击 Configure elastic pool。我们有 Pool settings 选项卡，其中包含不同的设置，例如服务层、计算硬件和 vCore 数量。我们可以启用弹性池区域冗余。在 Databases 选项卡中，我们将添加数据库。点击 Add databases。另一个窗口将被显示，其中将包含所有单数据库。您可以选择一个或多个数据库添加到弹性池中。它们将共享相同的计算资源：

![](img/66aceec30d3e7dead2f71bb68a887688_173_1.png)

图 7.16 – 向弹性池添加数据库

4. 点击 Apply，然后选择 Review + create，然后点击 Create，弹性池将被创建。

5. 创建后，选择 Go to resource，在 Overview 中，我们可以检查资源消耗并识别哪个数据库消耗了最多的资源。这是一个监控工具，可帮助我们对每个数据库的性能问题进行诊断。我们还可以使用 Monitoring 功能，在 Database Resource Utilization 下，显示池中每个数据库的性能。

6. 我们也可以通过选择 Configure 来配置我们的弹性池：

![](img/66aceec30d3e7dead2f71bb68a887688_174_0.png)

图 7.17 – 配置弹性池

7. 如果我们需要通过增加或减少分配的资源来更新弹性池配置，我们可以调整池大小、服务层或每个数据库的资源。我们可以添加或删除池内的数据库：

![](img/66aceec30d3e7dead2f71bb68a887688_174_1.png)

图 7.18 – 弹性池中的池设置

如果您有大量数据库需要使用特定模式迁移，Azure SQL Database 弹性池是最好的解决方案。在下一节中，我们将部署 SQL Managed Instance。

## Exercise 3 – deploying SQL Managed Instance

如果我们回到图 7.2 所示的 SQL 部署页面，我们现在可以選擇 **SQL managed instances**。将 **Resource type** 保留为 **Single instance** 并点击 **Create**：

![](img/66aceec30d3e7dead2f71bb68a887688_175_0.png)

图 7.19 – 创建 SQL 托管实例

在 **Basics** 选项卡中，我们将介绍与项目详细信息、托管实例详细信息和身份验证相关的信息；它与 SQL 数据库类似。

在 **Networking** 选项卡中，我们将配置虚拟网络和公共端点。您可以保留默认配置；这取决于您的内部配置。

在 **Security** 选项卡中，我们需要启用 Microsoft Defender for SQL。我们可以配置系统分配和用户分配托管标识，以在该数据库与其他 Azure 资源之间启用集中访问管理。

我们还有更多选项卡，例如 **Additional settings**，用于自定义额外的配置参数，包括地理复制、时区和排序规则。

然后，我们点击 **Review + create** 按钮。

Azure SQL Managed Instance 用于轻松迁移，因为它与本地服务器兼容。它是一个完全托管的 PaaS，易于使用和配置现代数据库。

让我们探索如何将 Azure SQL Database 连接到 ASP.NET 应用程序。

图 7.21 – 连接对话框

我们将按照以下步骤配置数据库连接：

- 1. 在 **Connect** 对话框中，展开 **Azure** 节点。所有 Azure SQL Database 实例将被列出。
- 2. 我们将选择将与我们的应用程序一起使用的数据库。
- 3. 数据库相关的信息将在底部填写，但我们在点击 **Connect** 按钮后必须输入管理员密码。
- 4. **Create a new firewall rule** 对话框显示。要从 Azure 外部连接到数据库，您必须创建服务器级防火墙规则。防火墙规则允许本地计算机的公共 IP 地址。

6. 返回我们的应用程序。打开 appsettings.json 文件并添加连接字符串：

![](img/66aceec30d3e7dead2f71bb68a887688_179_0.png)

图 7.24 – Visual Studio 2022 中数据库的连接字符串

7. 我们可以使用 Database.Migrate() 调用帮助我们在 Azure SQL Database 中运行数据库。

如果您使用本地数据库进行开发并使用 Azure SQL Database 进行生产，您需要在 Program.cs 中配置每个环境。

## Summary

在本章中，我们探索了在 Azure 中部署 SQL Server 的不同服务。我们首先部署了单个 SQL 数据库。我们有多种选项，例如无服务器数据库选项，允许我们创建低成本数据库。无服务器是 Azure SQL Database 中单数据库的计算层，可根据工作负载需求自动扩展计算，并按每秒使用的计算量计费。在我们部署了包含无服务器选项的 Azure SQL Database 弹性池之后，我们部署了 SQL Managed Instance，最后将 Azure SQL 数据库连接到现有的 ASP.NET 应用程序。

在下一章中，我们将讨论 Azure Storage 服务中可用的不同存储选项，以及每种存储选项合适的场景。

## Further reading

如果您想了解有关 Azure 数据库服务的更多信息，可以查看以下链接：

- https://learn.microsoft.com/en-us/azure/azure-sql/database/sql-database-paas-overview?view=azuresql
- https://learn.microsoft.com/en-us/azure/azure-sql/database/?view=azuresql

## Questions

- 1. 什么是 Azure SQL Database？
- 2. 我们如何将 Azure SQL Database 连接到 ASP.NET 应用？

## Working with Azure Storage

Azure Storage 是 Microsoft 的云存储解决方案，远不止数据存储。Azure Storage 为各种数据对象提供可扩展且安全的对象存储、云中的文件系统服务、相关的消息存储和 SQL 存储。

本章将介绍 Azure Storage 中可用的不同存储选项以及每种存储选项合适的场景。

在本章中，我们将涵盖以下主要主题：

- Azure Storage account
- Exploring Azure Table storage
- Exploring Azure Blob Storage
- Exploring Azure Disk Storage
- Exploring Azure Files

## Azure Storage account

在医疗保健解决方案中，我们有一个用于患者医疗记录的平台。它包括与患者相关的非结构化数据，例如文档、消息、笔记和图像，例如诊断文档、年度检查和疾病监测。所有这些文档都存储在虚拟机上，使用自定义解决方案来提取或加载非结构化数据。

Azure Storage 是一项服务，可用于存储文件、消息、表和其他类型的数据。它可扩展、安全、易于管理，并且可以从任何地方访问。Azure Storage 遵循两种云模型：通过使用虚拟机的基础设施即服务，以及通过使用原生云服务的平台即服务。Azure Storage 包括四个数据服务，可以通过存储帐户访问。

每种类型都支持不同的功能，并且具有唯一的 URL 地址以供访问。数据和您的 Azure Storage 帐户始终会复制以确保耐用性和高可用性。

