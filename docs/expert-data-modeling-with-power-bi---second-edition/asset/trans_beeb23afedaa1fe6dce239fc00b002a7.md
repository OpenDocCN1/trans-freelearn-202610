

values only keep up to four decimal points, no more).

因此，信息始终是使用对业务有意义且在数据模型中高效的数据类型。使用 DAX Studio 中的 VertiPaq Analyzer 有助于理解数据模型的各个方面，包括列数据类型。

作为数据建模人员，必须了解 Power Query 类型和方面如何转换为 DAX 数据类型。Power Query 和 DAX 是两种具有各自数据类型的不同表达式语言。

下表显示了 Power Query 类型与 DAX 数据类型之间的映射：

| Power Query Type Kind | Type Representation (Facet) | Mapping Datatype in DAX | xVelocity Internal Datatype |
|---|---|---|---|
| date | Date | Date/time | DateTime |
| datetime | Date/Time | Date/time | DateTime |
| datetimezone | Date/Time/Zone | Date/time | DateTime |
| duration | Duration | Decimal number | Double |
| logical | Logical | True/false | Boolean |
| number | Whole Number | Whole number | Int64 |
|  | Decimal Number | Decimal number | Double |
|  | Fixed Decimal Number | Fixed decimal number | Decimal |
|  | Percentage | Decimal number | Double |
| text | Text | Text | String |
| time | Time | Time | DateTime |
| any | Any | Text | String |

图 7.25：Power Query 到 DAX 数据类型映射

正如我们所见，数据类型转换会影响数据模型的压缩率和性能。但这还不是全部。具有未转换为更合适数据类型的任何数据类型的列可能会导致其他影响，这就引出了下一节，避免使用 any 数据类型的列。

## Avoid having columns with any data type

正如我们在第 3 章《Power Query Editor 中的数据准备》中解释的那样，Power Query 中的所有数据类型都与 any 数据类型兼容。但是，数据模型层中的 xVelocity 引擎不支持 `any` 数据类型。当数据加载到数据模型中时，`any` 数据类型会转换为 `text`，这可能正确也可能不正确。因此，如果列包含整数值，但其数据类型为 `any`，则在将数据加载到数据模型时它会变成 `text`，我们都知道 `text` 比 `number` 消耗更多内存。这并不是唯一的缺点。刷新包含 `true/false` 值的类型为 `any` 的列可能会导致意外行为。这种意外行为已在 Power BI 社区中报告，微软在其官方 Learn 网站的专门子部分中对其进行了说明，解释如下：

> 当你在 Power BI Desktop 中创建一个包含 ANY 数据类型列的报表，并且该列包含 TRUE 或 FALSE 值时，该列的值在刷新后在 Power BI Desktop 和 Power BI 服务之间可能不同。在 Power BI Desktop 中，底层引擎将布尔值转换为字符串，保留 TRUE 或 FALSE 值。在 Power BI 服务中，底层引擎将值转换为对象，然后将值转换为 -1 或 0。
> 
> 在 Power BI Desktop 中使用此类列创建的可视化效果在刷新事件之前可能按设计运行或显示，但在刷新事件之后可能会发生变化（由于 TRUE/FALSE 被转换为 -1/0）。

你可以在 Microsoft Learn 网站上的以下位置找到该文章：

https://learn.microsoft.com/en-gb/power-bi/connect-data/refresh-troubleshooting-refresh-scenarios?WT.mc_id=DP-MVP-5003466

这是避免使用 any 数据类型列的又一证据。

## Include the data type conversion in the step when possible

一些 Power BI 开发人员和数据建模人员忽略的一个关键点是，一些最常用的 Power Query 函数具有可选的操作数来强制函数的输出类型。如果我们忽略这一点，我们就需要至少添加一个额外步骤来进行数据类型转换；转换步骤越多，数据刷新越慢。以下函数具有可选的 `columnType` 操作数，我们可以使用它来强制输出数据类型，从而可能避免为类型转换添加额外步骤：

- `Table.AddColumn(table as table, newColumnName as text, columnGenerator as function, optional columnType as nullable type)`
- `Table.DuplicateColumn(table as table, columnName as text, newColumnName as text, optional columnType as nullable type)`
- `Table.AddIndexColumn(table as table, newColumnName as text, optional initialValue as nullable number, optional increment as nullable number, optional columnType as nullable type)`

例如，我在四个步骤中添加了四个新列，如以下屏幕截图所示。然后，我添加了另一个步骤以在单个步骤中更改新列的类型：

![](img/5d2bce74fd6fceebed14fd1488bf7088_392_0.png)

图 7.26：在不为每个转换步骤添加列数据类型的情況下添加新列

DateKey Added 表达式如下所示：

```
Table.AddColumn(#"Converted to Table", "DateKey", each Int64.From(Text.Combine({Date.ToText([Date], "yyyy"), Date.ToText([Date], "MM"), Date.ToText([Date], "dd")})))
```

以下屏幕截图显示 DateKey Added 表达式的输出类型为 any：

![](img/5d2bce74fd6fceebed14fd1488bf7088_392_1.png)

图 7.27：DateKey 列的类型为 any

![](img/5d2bce74fd6fceebed14fd1488bf7088_394_0.png)

图 7.28：将数据类型转换步骤合并为一个数据类型转换步骤

正如前面的屏幕截图所示，我们可以通过仅保留一个 **Changed Type** 作为最后一个转换步骤来省略过多的 **Changed Type** 步骤。

## Optimize query size

本节讨论其他数据准备最佳实践，以改进我们的模型。优化查询大小可以减少数据刷新时间。优化大小的模型在将数据导入数据模型后性能更好。在以下小节中，我们将介绍一些有助于优化查询的技术。

### Remove unnecessary columns and rows

在实际场景中，我们可能会处理包含数亿行和数百列的大型表。一些 Power BI 开发人员从所有数据源导入所有列和行，导致报表性能不佳。如前所述，Power BI 使用 **xVelocity** 引擎，该引擎基于列基数使用内存中数据处理进行数据分析。因此，列越少，直接转化为内存消耗越少，从而数据模型性能越好。在许多实际场景中，我们需要业务批准才能从表中删除不需要的列。

同样，行数直接影响数据刷新和数据模型性能。Power BI 开发人员/建模人员通常忽略的另一个方面是忘记 **Power BI Desktop Report (PBIX)** 文件大小限制。Power BI 服务上也有一些资源限制。这些限制因我们持有的许可计划而异。因此，在不考虑大小限制的情况下，通过将数据源中的所有数据导入 Power BI 模型来创建报表是一个坏主意，会导致超出大小限制的错误。我们在 *第 1 章，Power BI 中的数据建模简介* 中讨论了不同的 Power BI 许可模式。下表显示了适用于我们持有的许可层级的 PBIX 文件大小限制：

| Power BI License | Power BI Report (PBIX) File Size Limit |
|---|---|
| Free | Up to 1 GB |
| Pro | Up to 1 GB |
| Report Server | Up to 2 GB |
| P1/A4 | Up to 3 GB |
| P2/A5 | Up to 6 GB |
| P3/A6, P4/A7, and P5/A8 | Up to 10 GB |

图 7.29：Power BI 许可上的 PBIX 文件大小限制

虽然 Power BI Premium Capacity 可以在 Power BI 服务中支持高达 400 GB 的模型大小，但前表中 PBIX 大小限制保持不变。

在许多情况下，我们可能不需要将完整的数据历史导入 Power BI 数据模型。因此，请务必在信息收集研讨会上查明所需的数据量并过滤掉不必要的数据。

### Summarization (Group by)

在许多情况下，我们需要创建报表来显示汇总数据。那么为什么我们需要以最低粒度将所有数据导入数据模型呢？一个很好的理由是构建支持尽可能多业务需求的面向未来的解决方案。我们应努力在 Power BI 中创建服务于多个业务流程的集中式数据模型。但创建集中式数据模型并不总是那么简单。其中一个原因就是 **PBIX 文件** 限制和 Power BI 服务中的资源限制。我们讨论了删除不必要的列和过滤行作为优化模型大小的有效技术。

汇总表是实现模型大小优化、从而获得更高性能数据模型的另一种有效方法。通过汇总数据，我们将数据的粒度更改为更高级别。因此，汇总级别是在与业务进行信息收集研讨会时需要确定的另一个因素。

在 Power Query 中，我们使用 `Group by` 功能来汇总表。你可以在 *第 5 章，常见数据准备步骤* 的 *常见表操作* 部分研究 `Group by` 功能。我们在讨论 *第 10 章，高级数据建模技术* 中的聚合时将详细重温此技术。但将其作为数据准备最佳实践提及是值得的。

