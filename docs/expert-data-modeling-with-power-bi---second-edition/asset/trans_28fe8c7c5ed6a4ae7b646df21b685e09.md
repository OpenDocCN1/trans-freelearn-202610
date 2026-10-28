

![](img/5d2bce74fd6fceebed14fd1488bf7088_232_0.png)

## Figure 4.57: Connecting to Dataverse

要了解更多关于 Dataverse 的信息，请访问 Microsoft 文档中的以下页面：

https://docs.microsoft.com/en-nz/power-apps/maker/data-platform/data-platform-intro?WT.mc_id=DP-MVP-5003466

## Understanding data source certification

虽然数据源认证更偏向于 Power BI 治理，但了解它是什么以及它如何影响数据建模者至关重要。数据源认证更多地关乎数据的质量以及我们能够在不同数据源中可用的数据上建立的信任程度。通过数据源认证，我们将数据源分为三个或更多类别。不同的组织使用不同的术语来指代其数据质量。本节使用 Bronze、Silver 和 Gold/Platinum 类别，表示数据源中包含的数据质量。您可以在您的数据源认证中使用不同的术语。

## Gold/Platinum

**Gold** 或 **Platinum** 数据源在数据和业务两个方面都经过完全整理。数据质量问题极少。数据已处于最佳状态，可在 Power BI 等分析和报表工具中进行分析和可视化。典型的 Gold 数据源可以是托管在 SSAS 中的语义模型，无论是多维模型还是表格模型，**SAP Business Warehouse (SAP BW)**、数据仓库等等。当我们处理 Gold/Platinum 数据源时，我们更有信心数据是正确的。我们期望从开发角度来看几乎为零的数据准备和数据建模工作。

## Working with connection modes

当我们连接到数据源时，查询连接模式属于以下三种不同类别之一：

- Data Import
- DirectQuery
- Connect Live

在 **Power Query Editor** 中连接到一个或多个数据源的每个查询都有上述连接模式之一，除了 Connect Live。当连接模式为 Connect Live 时，Power BI 模型目前不能同时连接到多个 SSAS 或 AAS 实例，或 Power BI 数据集。如前所述，在 2020 年 12 月，Microsoft 发布了复合模型新版本的预览版，在该版本中，我们可以使用 DirectQuery 模式连接到多个 Power BI 数据集或 AAS/SSAS Tabular 2022 模型。我们将在 *Chapter 14, DirectQuery Connections to Power BI Datasets and Analysis Services in Composite Models.* 中更详细地讨论复合模型。

本节介绍连接模式、其应用和局限性。

## Data Import

这是 Power BI Desktop 中最常见的连接模式。许多连接，例如基于文件的连接，只有此选项。正如其名称所示，**Data Import** 模式将数据从源导入到 Power BI 数据模型中。数据已在 Power Query 层中准备和转换；然后将其导入到数据模型中。我们可以在 Power BI Desktop 中手动刷新数据，或在发布到 Power BI 服务后自动刷新。

## Applications

**Data Import** 模式的主要应用是将来自不同来源的数据整合到单一事实来源中。此外，它还为我们提供了数据建模能力，以便在 Power BI 中创建语义模型。我们可以全天频繁刷新数据。所有这些优点都伴随着一些限制，如下所述。

## Limitations

Power BI 数据集在 **Data Import** 模式下具有基于许可层级的有限存储大小和自动数据刷新。

每个数据集的存储限制如下：

- 1 GB per dataset under the **free** and **Pro** licensing plans
- 100 GB for datasets published to workspaces backed with a **PPU** capacity
- 400 GB for datasets published to workspaces backed with a **Premium** capacity

根据我们的许可层级，我们目前可以从 Power BI 服务安排数据刷新，限制如下：

Dual: The tables in this mode can get data from the cache or the source system. For instance, depending on the level of detail into which the user drills down in an aggregation setup, the query results may come from the cached data or directly from the source. We discuss aggregations in Chapter 9, Star Schema and Data Modeling Common Best Practices.

Note

All DirectQuery limitations described in the Working with connection modes section also apply to the tables in the Dual storage model setting.

我们可以通过 Power BI Desktop 左侧窗格中的模型视图查看或更改存储模式属性，如下所示：

- 1. Click the Model view.
- 2. Select a table from the Fields pane.
- 3. Expand Advanced from the Properties pane.
- 4. Select a Storage mode from the drop-down list.

前面的步骤在以下图像中突出显示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_239_0.png)

Figure 4.58: Changing the table’s Storage mode property

> Note
> 
> We cannot change a table’s storage mode from **Import** mode to either **DirectQuery** or **Dual** mode because of the features that are not available in DirectQuery mode.

本节讨论了表的存储模式。在下一节中，我们将了解数据集存储模式。

## Understanding dataset storage modes

如您可能已经猜到的，数据集存储模式是指数据集中的数据是否缓存在内存中。考虑到这一点，从数据集的角度来看，有三种不同的模式，如下所述：

- **Import**: When the whole data is cached in memory. In this mode, all tables are in the **Import** storage mode setting.
- **DirectQuery**: When the data is not cached in memory. In this mode, all tables are in the **DirectQuery** storage mode setting.
- **Composite (Mixed)**: When a portion of data is cached in memory, while the rest is not. In this mode, some tables are in the **Import** storage mode setting; other tables are in **DirectQuery** storage mode or the **Dual** storage mode setting.

要在 Power BI Desktop 中查看和编辑数据集存储模式，请查看状态栏右侧，如以下图像所示：

## Join us on Discord!

Join The Big Data and Analytics Community on the Packt Discord Server!

Hang out with 558 other members and enjoy free voice and text chat.

https://packt.link/ips2H

![](img/5d2bce74fd6fceebed14fd1488bf7088_242_0.png)

## Common Data Preparation Steps

在上一章中，我们讨论了一些在 Power BI 中经常使用的数据源。我们还介绍了数据源认证以及各种连接模式、存储模式和数据集模式之间的差异。本章介绍常见的数据准备步骤，例如表、文本、`Date`、`DateTime` 和 `DateTimeZone` 操作。

我们通过提供真实世界的场景来查看每一种，这些场景可以帮助应对日常数据准备挑战。在 Power Query Editor 中，数据准备活动分为三个单独的选项卡，如以下屏幕截图所示：

- 1. **Home**: Contains more generic actions, such as creating a new query, creating or managing query parameters, and performing common data preparation steps such as split column, Group By, and more.
- 2. **Transform**: Contains more transformation functionalities that can be performed through the UI.
- 3. **Add Column**: Contains data preparation steps related to adding a new column through the UI.

![](img/5d2bce74fd6fceebed14fd1488bf7088_243_0.png)

Figure 5.1: Data preparation functionalities available via Power Query Editor UI

接下来的几个部分描述了在前一图像中显示的选项卡下可用的一些功能，以及在数据准备阶段常用的但不通过 UI 可用的功能。

本章从 *Chapter 5, Common Data Preparation Steps, Raw.pbix* 示例文件开始。继续前进，我们将继续开发此文件。要使用示例文件，请在 Power BI Desktop 中打开它，然后更改以下查询参数的值以指向您本地的示例源文件副本：

- Adventure Works DW Excel path to point to `AdventureWorksDW2017.xlsx`
- Internet Sales in Time Excel path to point to `Internet Sales in Time Level.xlsx`

确保您应用了功能区下方出现的黄色警告消息中的更改。

您可以通过以下链接从 GitHub 找到源代码和示例文件：

https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/tree/main/Samples/Chapter%2005

在 Power BI 中，我们可以从各种来源以不同格式获取数据。无论数据源的格式如何，数据质量都很重要。在某些情况下，我们必须处理已经转换过的数据，但在现实中，有更多的情况需要我们转换数据并为其在数据模型中进行分析做好准备。一个典型的例子是当数据源是 Excel 或 CSV 中的数据透视表时。因此，我们必须在将其加载到数据模型之前准备数据。本章更详细地介绍了 Power Query 中最常用的数据转换和数据操作功能，包括以下内容：

- Data type conversion
- Splitting a column by delimiter

