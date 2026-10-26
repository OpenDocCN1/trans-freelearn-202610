

在 Power Platform 管理中心创建的数据集成项目利用了与 Power BI 和数据流相同的 Power Query 技术。可以通过选择本食谱中创建的数据集成项目中的任务，然后选择高级查询和筛选来看到这一点。熟悉的 Power Query 编辑器界面会显示出来，包括访问高级编辑器以直接插入 M 代码。此高级查询和筛选界面可用于进一步优化源数据查询，例如删除不必要的列，以提高数据源查询的整体效率或执行所需的转换。

## 还有更多…

使用数据集成项目只是将数据从 D365 F&SCM 环境提取到 Dataverse 或其他 Power BI 容易访问的环境的一种方式。使用生命周期服务，可以启用双写功能并导出到 Azure Data Lake：

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_547_0.png)

图 12.49：生命周期服务中的双写和导出到数据湖

双写技术可在 D365 F&SCM 应用与 Dataverse 之间实现近实时、双向集成。D365 F&SCM 应用中的数据更改会触发对 Dataverse 的写入操作，反之亦然。

导出到数据湖会将信息从 D365 F&SCM 表和实体复制到低成本的 Azure Data Lake Storage Gen2。随着 D365 F&SCM 中的数据发生变化，更改、删除和新增会在几分钟内处理。

此外，Power BI 瓦片、仪表板和报表可以在 D365 F&SCM 环境中进行一次性集成后，固定到应用程序工作区中。

最后，Power BI 报表可以通过生命周期服务部署。此方法利用了 Power BI 嵌入功能。

## 另请参阅

- *Administer Microsoft Power Platform:* https://bit.ly/3bqaKTQ
- *Integrate data into Microsoft Dataverse:* https://bit.ly/3bpQykV
- *Connect finance and operations apps with a new Microsoft Dataverse instance:* https://bit.ly/4bvTNox
- *Dual-write overview:* https://bit.ly/3I242mN
- *Configure PowerBI.com integration:* https://bit.ly/3wbukAH
- *Analytical Workspaces (using Power BI Embedded):* https://bit.ly/3SCAh0j

## 摘要

本章重点介绍了 Power BI、报表服务器、PowerPoint、Power Apps、Power Automate、Dataverse 和 Dynamics 365 之间强大的集成点。这包括利用 DAX 作为查询语言来支持 Excel 中的自定义报表，将报表服务视觉对象固定到 Power BI 仪表板，以及利用立方体公式构建模板或记分卡报表布局。此外，我们还提供了一个使用 Power Automate 设计的自动化工作流示例，用于将数据从关系数据库推送到 Power BI 服务中的流式语义模型，从而通过常见的 Power BI 可视化和数据警报功能实时显示源数据更改。最后，我们提供了演示如何将 Power BI 与 Dataverse 和 Dynamics 365 集成的食谱。

在本书的下一章也是最后一章中，我们将探讨利用 Microsoft Fabric（以前称为 Power BI Premium）独特功能的场景。

## 加入我们的 Discord 社区

加入我们社区的 Discord 空间，与作者和其他读者进行讨论：

https://discord.gg/ebXZqyrxJb

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_548_0.png)

## 13

## 使用 Premium 和 Microsoft Fabric

Power BI 并不是微软拥有历史的数据分析平台。在持续投资 Power BI 的同时，微软还寻求提供一套整体且集成的功能，针对包括数据科学、数据工程以及多种数据湖、数据仓库和湖仓架构在内的现代用例。特别是，微软利用其在 SQL Server 方面的经验，开发了名为 Parallel Data Warehouse（PDW）的基于大规模并行处理（MPP）的本地设备。后来，该设备被重新命名为 Analytics Platform System（APS）。随着工作负载向云端迁移，微软于 2016 年基于相同技术创建了 Azure SQL Data Warehouse（SQL DW）。随着数据分析领域复杂性和竞争的增加，微软增加了更多功能并将整个平台重新命名为 Synapse Analytics。SQL DW 只是该新平台的一个小部分，因此被重新命名为专用 SQL 池。

在过去十年左右的时间里，Power BI 和现在称为 Azure Synapse Analytics 的两个平台之间只有最少的集成。这一切在 2023 年发生了变化，当时微软宣布了 Microsoft Fabric。Microsoft Fabric 基本上在 Power BI 服务中展示了 Azure Synapse Analytics 工作负载，通过有效地合并微软的两个数据分析平台，创建了一个单一的数据分析平台。Microsoft Fabric 现在是所有未来开发的单一统一平台，而 Power BI 只是该更大框架的一个组件。

随着向 Microsoft Fabric 的转变，微软创建了一种新的许可机制，即 Fabric 容量（F SKU）。与 Power BI Premium 不同，后者以单一月价包含容量、网络、Premium 功能和存储，F SKU 按使用小时计费，而存储和网络则单独计费。Power BI Premium 容量（P SKU）自动继承了所有 Microsoft Fabric 工作负载，其中 F64 SKU 等同于 P1 SKU。

本章中的食谱涵盖了特定于 Power BI Premium 容量和 Microsoft Fabric 的功能和功能，包括数据仓库、湖仓和机器学习：

- 创建数据仓库
- 使用湖仓进行数据工程
- 备份语义模型
- 利用 Direct Lake
- 应用数据科学

到本章结束时，您将对 Microsoft Fabric 和 Power BI Premium 的功能和特性有坚实的理解。

## 技术要求

完成本章食谱所需的条件如下：

- Power BI Desktop。
- Microsoft Fabric 或 Power BI Premium 容量或试用版。
- SQL Server 2022 或更新版本，并安装了 AdventureWorksDW2022 数据库。有关安装 SQL Server 和 AdventureWorks 数据库，请参阅第 1 章《安装和许可 Power BI 工具》中的《安装 SQL Server 和 AdventureWorks 数据库》食谱。
- 本章的文件可以从以下 GitHub 仓库下载：https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition。

## 创建数据仓库

Gartner 对数据仓库的定义如下：

> “数据仓库是一种存储架构，旨在保存从交易系统、操作数据存储和外部来源提取的数据。然后仓库以聚合、汇总的形式组合这些数据，适合企业范围内的数据分析和报告，以满足预定义的业务需求。”

由于数据仓库能够为各种应用程序和用例提供集中且单一的事实来源，因此它们多年来一直是商业智能架构的主流。通过在强大的上游工具（如 Azure Data Factory 管道）中处理数据质量和集成需求，并通过熟悉的 SQL 查询和对历史跟踪的支持生成具有代理键用于关系/连接的合并维度和事实表，一个设计良好且管理的良好的数据仓库仍然是一项极具价值的资产。

无论数据仓库这一术语的具体历史如何，今天数据仓库都是当今企业业务系统的支柱，也是许多组织的商业智能和报告的核心组件。Microsoft Fabric 通过基于 SQL Server 引擎、分布式处理以及 OneLake 中的集中式存储的云端、湖中心技术提供数据仓库功能，OneLake 是 Fabric 的逻辑集中式数据湖。

本食谱提供了如何在 Microsoft Fabric 中创建和使用数据仓库的指导。

