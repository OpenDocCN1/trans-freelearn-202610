

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_379_0.png)

图 10.59 – 为 trainingcatalog 授予角色

点击 Save。

6. 现在，返回到 Azure Purview Studio：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_379_1.png)

图 10.60 – 调用新的扫描

7. 要开始扫描，请点击 New Scan 图标。提供以下值：

- Name: Scan Data
- Connect via integration runtime: 使用默认值

+   - **Credential:** 使用默认值：

# Scan "AzureDataLakeStorage-UG8"

Name *

Scan-Data

Connect via integration runtime * ⓘ

Azure AutoResolveIntegrationRuntime

Credential *

Purview MSI

> 💡 在设置扫描之前，您必须授予 Purview 账户托管标识连接到 Azure Data Lake Storage Gen2 的权限。了解更多

Continue

Connection successful

Test connection

Cancel

图 10.61 – 新扫描详情

点击 Test Connection，然后点击 Continue。

8. 在 Scope your scan 屏幕上，选择 bronze，如下所示：

# Scope your scan

Refresh

所有未来位于某个父项下的资产，如果父项被完全或部分选中，将自动被选中。

Search

AzureDataLakeStorage-UG8

bronze

gold

silver

Continue

Back

Cancel

图 10.62 – 为新扫描选择 bronze 层

点击 **Continue。**

> **Important**
> bronze 层是湖屋中的数据第一入口。因此，它是扫描可能包含分类信息（例如 PII）数据的良好候选。

9. 对于 **Select a scan rule set**，选择 **AdlsGen2**。AdlsGen2 是默认扫描规则集，包含所有支持的用于模式提取和分类的文件类型，以及所有支持的系统分类规则：

# Select a scan rule set

+ New scan rule set  ↻ Refresh

选择一个扫描规则集供您的扫描使用。

**AdlsGen2** [SYSTEM DEFAULT]
Microsoft 默认扫描规则集，包含所有支持的用于模式提取和分类的文件类型，以及所有支持的系统分类规则 View detail

Continue   Back                                   Cancel

图 10.63 – 为新扫描选择扫描规则

10. 点击 **Continue。** 在 **Set a scan trigger** 屏幕上，您可以设置重复运行或仅运行一次的扫描：

# Set a scan trigger

设置扫描触发器以在特定日期和时间运行扫描。如果选择一次，扫描将在设置完成后开始。如果选择重复，扫描将在您选择的日期和时间开始。初始扫描为全量扫描，后续每次扫描为增量扫描。

○ Recurring    ● Once

Continue   Back                                   Cancel

图 10.64 – 为新扫描设置扫描触发器

现在，我们选择 **Once**。点击 **Continue**：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_382_0.png)

图 10.65 – 查看新扫描的设置

11. 点击 **Save and Run**。您现在应该回到 **Sources** 屏幕：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_382_1.png)

图 10.66 – 查看新扫描详情

12. 此时，bronze 层中的数据正在被扫描。等待几分钟让扫描完成。要检查运行状态，请点击 View Details：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_383_0.png)

图 10.67 – 新扫描状态

扫描完成后，其状态将为 Completed。

> Important
> 
> 在生产环境中，每次运行摄取管道时您都可能收到新数据。因此，建议使用重复选项，以便定期扫描增量数据。

13. 我们现在准备浏览之前扫描的资产。使用左侧菜单，点击 Home：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_384_0.png)

图 10.68 – 浏览已扫描的资产

14. 点击 Browse Assets，然后点击 Azure Data Lake Storage Gen2。

15. 通过进入 traininglakehouse > bronze > sales > [dbo].[store_customers] > T Z 来浏览目录结构。

您将看到 Schema classifications，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_384_1.png)

图 10.69 – 检查分类

注意 Azure Purview 中的扫描如何自动为两个列发现了 **Classifications**：**Email Address** 和 **Credit Card Number**。正如您在前几章中回忆的那样，我们在 store customer 表中有这些列。那么，为什么对数据进行分类如此重要呢？

**数据分类**是满足合规要求和数据治理的重要第一步。基于数据的敏感性定义适当的标签、标记和元数据有助于在查询时定义数据保密性。

> **Important**
> 根据业务数据的类型，分类通常是基于数据内容、上下文和用户级别执行的。

# Cleaning up Azure resources

为节省成本，您可能希望清理以下 Azure 资源：

+   1. 删除 Azure Purview 账户；即 `trainingcatalog`。
+   2. 删除 Azure Data Share 账户；即 `trainingshare`。

现在，让我们总结本章。

# Summary

本章因以下几个原因非常重要。通过几个示例，我们了解了数据工程领域常见的挑战。处理诸如模式演变和治理等挑战对于现代数据工程项目至关重要。这些挑战随着时间的推移而演变，同样使数据工程师的工作变得更轻松的技术和工具也在不断发展。

在下一章中，我们将了解数据工程师的 DevOps 基础知识。DevOps 正在迅速成为数据工程师必备的附加技能，因此我们将学习如何使用 **基础设施即代码 (IaC)** 范式自动配置云资源。利用 IaC 的强大功能，数据工程师可以快速、可重复且安全地启动使用云资源的数据管道。

# 11
Infrastructure Provisioning

随着数据分析需求的增长，数据工程师在市场上正变得昂贵且难以找到。与此同时，雇用数据工程师的组织正在寻找创新方法以用更少资源做更多事情，从而证明高资源成本的合理性。在最近的趋势中，大多数这些组织开始使用自动化基础设施配置来简化云部署。直到最近，基础设施配置工作通常由 DevOps 团队处理，但现在不再如此。

在上一章中，我们谈到了数据工程师工作角色的动态性。现代数据工程师需要跟上这一最新趋势，并在 DevOps 技能方面进行培训。本章和下一章旨在教授数据工程师一些关键的 DevOps 技能。

> **Important Note**
> 在当今的就业市场中，精通 DevOps 的数据工程师是致命的组合。现代组织更愿意雇用一个同时具备这两种技能的资源，而不是雇用两个。

在本章中，我们将涵盖以下主题：

+   - Infrastructure as code
+   - Deploying infrastructure using Azure Resource Manager
+   - Deploying multiple environments using IaC

# Infrastructure as code

直到最近，基础设施部署一直是一个极其耗时且昂贵的过程。在典型的组织中，系统管理员团队负责使用一系列手动执行步骤在多个环境中部署硬件和软件。由于其手动性质，该过程存在主要缺陷：

+   - **Unique settings per environment**: 每个独立环境都可能具有独特的设置。为满足这些要求，手动步骤需要为每个环境记录。
+   - **Time-consuming**: 手动步骤的记录和执行非常耗时。
+   - **Inconsistent and error-prone**: 遵循手动步骤进行基础设施部署可能导致不一致和错误。

**基础设施即代码 (IaC)** 是一种使用源代码部署和管理基础设施组件的做法。使用 IaC，系统管理团队（也称为 DevOps）不再受限于手动实践和记录流程。相反，他们现在可以使用配置文件执行自动化和可重复的部署。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_388_0.png)

图 11.1 – 使用 IaC 的好处

