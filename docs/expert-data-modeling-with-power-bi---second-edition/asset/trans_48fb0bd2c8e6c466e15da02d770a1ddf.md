

## Power Query 编辑器中的星型模式准备

在上一章中，我们学习了一些常见的数据准备步骤，包括数据类型转换、拆分列、合并列、添加自定义列以及筛选行。我们还学习了如何使用“分组依据”功能创建汇总表、追加数据、合并查询以及透视表。

本章使用了过去几章中讨论的许多主题，并帮助您学习如何在 Power Query 编辑器中准备星型模式。Power BI 中的数据建模始于星型模式的准备。在本章中，我们使用 `Chapter 6, Sales Data.xlsx` 文件，其中包含扁平化数据。您可以从此处下载该文件：

[https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de8200/Samples/Chapter%2006/Chapter%206,%20Sales%20Data.xlsx](https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de8200/Samples/Chapter%2006/Chapter%206,%20Sales%20Data.xlsx)

一个常见的日常场景是，我们获得了一组从源系统导出的文件数据，并且需要构建报表来回答业务问题。因此，具备在扁平化设计之上构建星型模式所需技能就显得非常实用。

在本章中，我们涵盖以下主题：

- 识别维度与事实
- 创建维度表
- 创建事实表

图 6.1：Power Query 编辑器已连接到 Chapter 6, Sales Data.xlsx 示例数据

我们必须通过查看示例数据本身以及使用 Power Query 编辑器来研究数据，以了解以下内容：

- 数据源中的表数量
- 现有表之间的关联
- 日期和时间的最低所需粒度

上述几点是我们必须在初始发现研讨会上向业务提出的最直接的初始要点。这些简单问题有助于我们了解工作的规模和复杂性。在接下来的几个部分中，我们将更详细地研究上述要点。

### 数据源中的表数量

我们的示例文件包含两个工作表，转换为两个**基础表**：**Sales** 和 **Customer**。我们从基础表中提取维度与事实；因此，它们将不会以原始形状加载到模型中。从建模角度来看，两个表之间必须存在关联，因此我们需要向业务提出此问题，研究数据并查看是否能找到关联。源系统中表的名称（在本例中为 Excel 文件）在以下图像中突出显示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_314_0.png)

- 产品数据之间的关联，例如 **Product Category**、**Product Subcategory** 和 **Product**
- 销售订单数据之间的关联，例如 **Sales Order** 和 **Sales Order Line**
- `Date` 与 `Time` 之间的关联，例如 **Order Date**、**Due Date** 和 **Ship Date**

### 查找日期和时间的最低所需粒度

在大多数现实世界案例中，如果不是全部，业务都必须在多个维度上分析数据，例如 **Product**、**Customer**、**Date** 或 **Time**，或同时 **Date** 和 **Time**。我们必须找到支持业务需求所需的最低详细粒度。你经常会遇到以下要求：

> *作为销售经理，我希望按分支、位置、日期和时间分析销售数据。*

在上述用户故事中，事实是业务希望分析的事件。因此，销售是一个事实。另一方面，维度是事实被分析所依据的一切。因此，在前述用户故事中，**分支**、**位置**、**日期**和**时间**是维度。我们还拥有一个 **Person** 维度来覆盖*销售经理*。在我们识别事实和维度时，识别数据的粒度也是一个至关重要的任务。数据的粒度，也称为颗粒度，乍一看并不明显。例如，在前述用户故事中，销售数据对于以下维度必须在什么详细程度上进行分析？

- **Location:** `Continent, Country, City, County, Area, Postcode, and Street number`
- **Date:** `Year, Quarter, Month, Week, and Day`
- **Time:** `Hour, Minute, and Second`

在现实世界场景中，尤其是在大型项目中，我们必须向业务提出相关问题以识别粒度。在小型项目中，我们可以通过仔细研究数据来识别粒度。让我们来看一下我们的场景。

在我们的示例中，业务需要在日期和时间上分析数据。但我们必须更详细地说明业务需要分析数据的日期和时间的层级。通过研究数据，我们发现 Sales 和 Customer 表中都有包含 Date 或 DateTime 数据类型的列。Sales 表中有三列包含 DateTime 数据类型。Power Query 编辑器会自动检测列的数据类型。更精确地研究数据表明，两个 DateTime 列中的值的时间元素始终为 12:00:00 AM。因此，很可能 DueDate 和 ShipDate 列的数据类型是 Date，而不是 DateTime。这意味着唯一具有 DateTime 数据类型的列是 OrderDate 列。以下图像显示 OrderDate 列中的数据以秒为单位存储：

![](img/5d2bce74fd6fceebed14fd1488bf7088_317_0.png)

图 6.3：Sales 表中具有 Date 和 DateTime 数据类型的列

因此，OrderDate 列的粒度可以达到秒。我们保存这个问题并稍后与业务确认。

让我们也看看 Customer 表。Customer 表中也有两列具有 Date 数据类型，BirthDate 和 DateFirstPurchase，如以下图像所示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_318_0.png)

图 6.4：Customer 表中的 Date 列

作为我们在发现研讨会中的下一步，我们必须与业务澄清以下几点：

- 他们是否需要分析客户的出生日期？
- 他们是否需要在不同的日期层级（`Year`、`Quarter`、`Month` 等）拥有 **BirthDate** 数据？
- 他们是否需要分析 **DateFirstPurchase** 数据？
- 他们是否需要在各种日期层级显示 **DateFirstPurchase** 数据？
- 对于 **Sales** 表中的 **OrderDate**，所需的 `Date` 和 `Time` 层级是什么？
- 对于 **Sales** 表中的 **DueDate** 和 **ShipDate** 列，`Date` 层级是什么？

在我们假设的发现研讨会中，业务澄清他们不希望分析客户的出生日期或首次购买日期。我们还发现他们需要按 `Date` 的 `Day` 级别以及 `Time` 的 `Minutes` 级别分析 **Sales** 表中的 **OrderDate**。他们还确认 **Sales** 表中的 **DueDate** 和 **ShipDate** 处于 `Day` 级别。

根据上述发现，很明显我们需要创建一个 `Date` 维度表和一个 `Time` 维度表。

下一步是识别维度、其粒度以及事实。

### 定义维度与事实

为了识别维度与事实，我们必须与业务进行需求收集研讨会。我们需要通过提问 WWWWWH 问题来了解业务流程；即 *What*、*When*、*Where*、*Who*、*Why* 和 *How*。这是一种流行技术，也称为 5W-1H。对这些问题的回答有助于我们了解业务流程，从而有助于识别维度与事实。让我们看一些例子：

- 对 *What* 问题的回答可以是产品、项目、服务等。
- 对 *When* 问题的回答可以是日期、时间或两者，以及在什么层级，月份、日期、小时、分钟等。
- 对 *Where* 问题的回答可以是实体位置，如商店或仓库、地理位置等。

在需求收集研讨会中，我们尝试确定什么描述了业务以及业务如何衡量自身。在我们的场景中，我们想象我们已经进行了几次发现研讨会，并发现业务需要以下销售数据：

- 销售金额
- 数量
- 成本（税、运费等）

销售数据必须按以下内容进行分析：

