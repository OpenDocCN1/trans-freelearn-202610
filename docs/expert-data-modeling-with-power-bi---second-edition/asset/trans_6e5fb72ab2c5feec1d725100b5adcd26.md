

## 使用聚合感知

用户定义的聚合或 **Agg Awareness** 是 Power BI 中最具重要性的可扩展性和数据建模功能之一，它解锁了大数据在 Power BI 中的使用。正如我们所知，当查询连接模式为 `Import mode` 时，Power BI 会将数据压缩并缓存到内存中。想象一个场景，我们有一个相当大的数据源，包含数十亿行数据，位于 Azure Synapse 服务器中（以前称为 Azure SQL Data Warehouse），并且我们希望在 Power BI 中创建一个企业级分析解决方案。

如前所述，Power BI 有存储限制，尤其是在我们使用免费版或专业版许可层时。即使我们拥有 Premium 容量，我们仍然受限于 Premium 容量中可用的专用内存。当然，使用 Power BI Premium Gen 2，内存限制将不再那么令人担忧。但如果我们的数据模型设计不够完善，数据处理和内存消耗很快就会成为瓶颈。这时使用聚合就派上用场了。正如我们在前几节中解释的那样，**Agg Awareness（管理聚合）** 功能仅适用于源自支持 DirectQuery 模式的数据源的明细表。

支持 DirectQuery 模式是 Power BI Desktop 中数据建模的关键点。正如在 *第 4 章，从各种来源获取数据* 中 *连接模式* 部分所讨论的那样，当查询处于 **Import** 模式时，我们无法从 Power BI Desktop 将其切换到 DirectQuery 模式或 **Dual** 模式。因此，我们必须在开始实施数据模型之前就考虑在设计时实现聚合；否则，我们最终将不得不从头开始创建数据模型，这并不理想。

我们可以采用以下三种不同的方法来实现聚合。根据数据源的不同，我们决定哪种方法最合适：

- **基于关系的聚合**：在此方法中，托管明细表的数据源采用星型模式，因此事实表和维度表之间存在关系。因此，我们在聚合表与其维度之间创建关系。
- **基于 GroupBy 的聚合：** 在此方法中，数据源是避免大型表之间关系的大数据模型，因此维度属性在事实表中被非规范化。在这种情况下，我们使用 GroupBy 列来实现聚合。
- **混合聚合：** 在此方法中，无论数据源的架构如何，我们都在聚合表中非规范化一些维度属性。聚合表还与其他维度具有关系。因此，我们在一个实现中混合了 GroupBy 和基于关系的聚合。

考虑到这一点，以下步骤解释了在 Power BI Desktop 中实现用户定义聚合的过程：

- 1. 创建聚合表。
- 2. 以 DirectQuery 模式加载所有必要的表。
- 3. 为基于关系和混合聚合在聚合表与其维度之间创建关系。
- 4. 设置聚合表及其相关维度的存储模型。
- 5. 配置 **管理聚合**。
- 6. 测试聚合以确保我们的查询命中聚合。

上述过程可能因我们的数据平台架构而异，但原则保持不变。

让我们继续一个场景。AdventureWorks 组织在 Power BI 中有一个现有的 *销售分析* 解决方案，性能不佳。业务部门抱怨 Power BI 服务上的自动数据刷新不断失败，并出现超时错误。该组织在 SQL Server 中有一个名为 AdventureWorksDW2019 的数据仓库。Power BI 中现有的解决方案包含一个大型事实表 **FactInternetSales**，处于 DirectQuery 模式。我们希望实现一个概念验证，以展示聚合如何提升性能。我们使用 **FactInternetSales** 表并使用以下聚合：

- **FactInternetSales** 行数
- **SalesAmount**
- **OrderQuantity**
- **TaxAmount**
- **Freight**

我们需要在 **EnglishProductName**、**CalendarYear** 和 **EnglishMonthName** 层级上使用上述聚合。我们还需要 **DimCustomer** 表用于客户分析，以演示当查询未命中聚合时的性能。

为了实现概念验证，我们必须考虑能够满足需求的聚合方法。让我们逐步分析：

- 基于关系的聚合：业务需要按年和月级别分析聚合。因此，要采用基于关系的方法实现聚合，我们需要创建包含 **ProductKey** 和 **DateKey** 的聚合表，按年和月级别，并包含场景中提到的所有聚合列。我们还必须创建一个额外的 **Date** 表，包含 **DateKey** 和按年和月级别的其他日期属性。然后我们在聚合表、新的 **Date** 表和 **DimProduct** 表之间创建关系。听起来有点太多了。所以基于关系的聚合不是可行的方案。
- `GroupBy` 聚合：在此方法中，我们必须在聚合表中非规范化 **CalendarYear**、**EnglishMonthName** 和 **EnglishProductName** 属性，这是可以的。我们 `GroupBy` 这些列对聚合列进行分组。但如果用户想按不同的产品属性分析销售呢？在这种情况下，聚合表不会被命中，而是查询在明细表上运行，这可能导致性能不佳。因此，此方法也不可行。
- 混合聚合：在此方法中，我们非规范化 **CalendarYear** 和 **EnglishMonthName** 属性，并在聚合表中使用 **ProductKey** 列。因此，我们按 **CalendarYear** 和 **EnglishMonthName** 属性对聚合列进行 `GroupBy`，并使用 **ProductKey** 列在聚合表和 **DimProduct** 表之间创建关系。这种方法看起来是最优雅的。

现在我们已经选择了实现方法，让我们继续开发解决方案。

要将 AdventureWorksDW2019 SQL Server 数据库用作源，我们必须从 Microsoft 网站下载 SQL Server 备份文件：

https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver15&WT.mc_id=DP-MVP-5003466

下载数据库备份后，我们需要在 SQL Server 实例中还原它。了解如何还原数据库备份：

https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver15&tabs=ssms&WT.mc_id=DP-MVP-5003466#restore-to-sql-server

还原数据库后，我们从 Power BI Desktop 连接到它。

## 创建聚合表

根据我们整体的数据平台架构，我们有几种创建聚合表的选项，如下所示：

- 在 SQL Server 侧创建数据库视图或表。这是最高效的选项，因为我们可以平衡 Power BI 中缓存数据与 SQL Server 之间的查询负载。我们将在本章的 *实现多个聚合* 部分更详细地讨论这一点。
- 在从 Power Query 连接到 SQL Server 实例时使用 T-SQL 语句。此选项也不错，但不如第一个灵活。
- 在 Power Query 编辑器中创建聚合表。虽然这是最不理想的选项，但在某些情况下前两个选项不可行时，可以使用。最好确保查询完全折叠。

### Note

聚合表就像任何其他表一样，可以处于 **DirectQuery** 或 **Import** 模式。根据场景和我们的数据平台架构，我们必须决定使用哪种存储模式。如果我们使用 `DirectQuery` 模式，最好使用 **列存储索引** 优化源数据库中的底层表。了解有关 **列存储索引** 的更多信息：

https://learn.microsoft.com/en-us/sql/relational-databases/indexes/columnstore-indexes-overview?view=sql-server-ver16&WT.mc_id=DP-MVP-5003466

需要注意的是，聚合表中列的数据类型必须与明细表中对应列的数据类型相同；否则，我们将无法稍后配置聚合。

我们将继续使用第一种方法实现。我们使用以下 T-SQL 脚本在 SQL Server 侧创建聚合表作为数据库视图：

```
CREATE VIEW vw_Sales_Agg AS
SELECT dp.ProductKey
    , dd.CalendarYear
    , dd.EnglishMonthName
    , COUNT(1) Sales_Count
    , SUM(fis.SalesAmount)  AS Sales_Sum
```
