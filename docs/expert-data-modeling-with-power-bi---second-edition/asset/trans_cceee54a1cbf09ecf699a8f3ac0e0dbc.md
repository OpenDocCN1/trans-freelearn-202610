

## 确定潜在事实

要识别事实，我们查看来自发现研讨会的发现结果。凭借我们对业务的现有了解，我们对事实有了一个大致概念。让我们在 Power Query 编辑器中查看数据，并找到数据类型为 Number 的列。尽管如此，并非所有数据类型为 Number 的列都包含事实。事实必须与之前步骤中确定的业务流程相符。考虑到这一点，在我们的练习中，以下列表显示了潜在的事实：

| Potential Fact | Derived From | Fact Columns |
|---|---|---|
| Sales | Sales | OrderQuantity, ExtendedAmount, TotalProductCost, SalesAmount, TaxAmt, Freight |

Figure 6.6: Potential facts derived from existing tables

> Note
>
> 在实际场景中，我们会与主题专家（SMEs）进行发现研讨会以识别维度和事实。但业务提供一组源文件并要求我们创建分析报告的情况也很常见。

现在我们已经识别出潜在的维度和事实，是时候开始准备星型模式了。第一步是将 Sales 和 Customer 表卸载，因为它们当前的扁平形状不适合星型模式。我们在第 1 章《Power BI 中的数据建模简介》的“理解非规范化”部分中解释了如何卸载表。

下一步是更改列的数据类型。如果在“已应用步骤”中自动添加了“更改类型”步骤，我们会检查检测到的数据类型以确保数据类型正确。这在以下图像中显示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_322_0.png)

Figure 6.7: Both the Sales and Customer tables unloaded

Power Query 会自动检测列标题，并在 **Type Detection** 设置不是 **Never detect column types and headers for unstructured sources** 时为每个表生成一个 **Changed Type** 步骤。**Type Detection** 设置可从 Power Query 编辑器（或 Power BI Desktop）中配置，路径为 **Options and settings | Options**，在 **Type Detection** 下。

Power Query 的自动检测数据类型行为可在 **Global** 和 **Current File** 两个级别进行配置。通过更改 **Global** 部分下的设置，更改将应用于 Power BI Desktop 的当前文件以及在 Power BI Desktop 中打开的未来文件。但更改 **Current File** 部分下的设置仅适用于当前文件。

以下步骤展示如何在 **Global** 级别更改此设置：

- 1. 点击 **File** 菜单。
- 2. 点击 **Options and settings**。
- 3. 点击 **Options**。
- 4. 点击 **Data Load**。
- 5. 在 **Type Detection** 下，您可以选择以下其中一项：

  - **Always detect column types and headers for unstructured sources**
  - **Detect column types and headers for unstructured sources according to each file’s settings**

## Geography

已识别的业务需求显示，我们必须有一个保存地理数据的维度。查看数据时，我们可以在 Sales 表中看到与地理相关的列。我们可以从 Sales 表派生出一个单独的 Geography 维度。然而，这可能无法覆盖所有业务需求。

如下图所示，Customer 表中也有一些与地理相关的列。我们必须在数据中寻找共同点，并考虑将两个表中的数据合并到单个 Geography 维度的可能性。使用 Column Distribution 显示 CustomerKey 列是 Customer 表的主键：

![](img/5d2bce74fd6fceebed14fd1488bf7088_325_0.png)

Figure 6.10: Column Distribution shows that CustomerKey is the primary key of the Customer table

启用和使用 Column Distribution 已在第 3 章《Power Query 编辑器中的数据准备》中进行了解释。

查看 Customer 表中的 **SalesContinent**、**SalesCountry** 和 **SalesRegion** 列以及 Sales 表中的 **SalesTerritoryGroup**、**SalesTerritoryCountry** 和 **SalesTerritoryRegion** 的 **Column Distribution** 显示，Customer 表中每个列的唯一值数量与 Sales 表中对应列的唯一值数量相匹配，如下图所示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_326_0.png)

Figure 6.11: Comparing Column Distribution for geography-related columns from the Customer table and the Sales table

为确保 Customer 表中的 **SalesContinent**、**SalesCountry** 和 **SalesRegion** 列与 Sales 表中的 **SalesTerritoryGroup**、**SalesTerritoryCountry** 和 **SalesTerritoryRegion** 列的值匹配，我们进行以下测试流程：

- 1. 引用 **Customer** 表。
- 2. 将表重命名为 **CustomerGeoTest**。
- 3. 卸载该表。
- 4. 通过选择 **Remove Other Columns** 保留 **SalesContinent**、**SalesCountry** 和 **SalesRegion** 列。

我们必须对 Sales 表中的以下列进行相同的流程以删除重复项：

  - SalesTerritoryGroup
  - SalesTerritoryCountry
  - SalesTerritoryRegion

下图显示了后者的结果与从 Customer 表中删除 **SalesContinent**、**SalesCountry** 和 **SalesRegion** 列的重复项的结果并排显示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_328_0.png)

Figure 6.14: Comparing the results of the CustomerGeoTest table and the SalesGeoTest table

如前面的屏幕截图所示，两者之间的唯一区别是列的排序顺序，这并不重要。基于前面的练习，我们不需要创建 **Geography** 维度，因为 Sales 表中的 SalesTerritoryGroup、SalesTerritoryCountry 和 SalesTerritoryRegion 列与 Customer 表中的 SalesContinent、SalesCountry 和 SalesRegion 列相比是冗余的，而 Customer 表中的地理相关列提供了更高级别的细节。

## Sales order

在 Sales 表中，SalesOrderNumber 和 SalesOrderLineNumber 列包含销售订单的描述性数据。我们是否需要为此创建维度？让我们查看数据：

- 1. 将列分析更改为基于整个数据集。
- 2. 查看这两个列的 Column Distribution 显示 SalesOrderNumber 有 27,659 个唯一值，而 SalesOrderLineNumber 只有 8 个唯一值：

![](img/5d2bce74fd6fceebed14fd1488bf7088_329_0.png)

Figure 6.15: Looking at the Column Distribution data shows that there are a lot of distinct values in SalesOrderNumber

SalesOrderNumber 列包含大量唯一值，这降低了创建新的 Sales Order 维度是一个好主意的可能性。因此，我们需要找到更多证据以避免创建 Sales Order 维度。如果我们合并这两个列，我们就能更好地了解如果决定创建它，新维度中将有多少行。

- 3. 选择 **SalesOrderNumber** 和 **SalesOrderLineNumber** 两列。
- 4. 从 **Transformation** 选项卡点击 **Merge Columns**。
- 5. 在 **Merge Columns** 窗口中，保持默认设置并点击 **OK**。

![](img/5d2bce74fd6fceebed14fd1488bf7088_330_0.png)

*Figure 6.16: Merging the SalesOrderNumber and SalesOrderLineNumber columns*

新的 **Merged** 列包含 **60,398 distinct** 和 **60,398 unique values**，这意味着 **SalesOrderNumber** 和 **SalesOrderLineNumber** 的组合是 **Sales table** 的 **primary key**，如下图所示。因此，即使我们创建单独的维度，我们得到的行数也与事实表相同。此外，我们无法想象与任何其他维度的任何关联。因此，将这两个列保留在 **Sales** 表中是最好的。这些维度由于其数据特征而无法移出事实表。它们也没有任何其他属性或与其它维度的有意义的链接。这类维度称为 **degenerate dimensions**。

