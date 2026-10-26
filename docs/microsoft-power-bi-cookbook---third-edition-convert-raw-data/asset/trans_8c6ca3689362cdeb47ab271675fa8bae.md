

## 准备工作

要准备本配方，请按照以下步骤操作：

- 1. 打开 Power BI Desktop。
- 2. 创建一个名为 AdventureWorksDW2022 的导入模式数据源查询。参见本章《实施语义模型》配方中的准备工作部分作为参考。
- 3. 将此查询隔离到名为 Data Sources 的查询组中并禁用加载。
- 4. 添加一个名为 Reseller Sales Intermediate 的查询，该查询引用 AdventureWorksDW2022 查询，导入 FactResellerSales 表，并添加一个从 1 开始的 Index 列：
- 5. 禁用 Reseller Sales Intermediate 查询的加载。
- 6. 添加一个名为 Reseller Sales 的查询，该查询引用 Reseller Sales Intermediate 查询并删除 ProductStandardCost 和 TotalProductCost 列：
- 7. 添加一个名为 Reseller Costs 的查询，该查询引用 Reseller Sales Intermediate 查询并选择 ProductTotalCost、TotalProductCost 和 Index 列。

```
let
    Source = AdventureWorksDW2022,
    dbo_FactResellerSales = Source{[Schema="dbo",Item="FactResellerSales"]}[Data],
    Index =
        Table.AddIndexColumn(dbo_FactResellerSales, "Index", 1, 1, Int64.Type)
in
    Index
```

```
let
    Source = #"Reseller Sales Intermediate",
    RemoveColumns =
        Table.RemoveColumns(Source,{"ProductStandardCost", "TotalProductCost"})
in
    RemoveColumns
```

```
let
    Source = #"Reseller Sales Intermediate",
    SelectColumns =
        Table.SelectColumns(
            Source,{"ProductStandardCost", "TotalProductCost", "Index"})
in
    SelectColumns
```

DAX RLS 规则 ISBLANK( [Index] ) 会过滤掉 Reseller Costs 表中 Sales Person 角色的所有行，因为 Index 列永远不会为空。请注意，虽然值被隐藏，但受保护列的元数据在模型中仍然可用。

## 还有更多...

列级安全性有替代方法。其中一种方法是使用单个事实表并使用度量来实现列安全性。在这种方法中，会创建一个包含单列和单行的虚拟 Security 表，其中包含一个值。会为诸如 ISBLANK( [Column] ) 的角色创建类似的 DAX RLS 规则。此方法会将模型中需要安全性的所有列隐藏起来，并用度量替换它们，语法类似于以下内容：

```
IF(ISBLANK(COUNTROWS('Security')),BLANK(),SUM('Reseller Sales'[TotalProductCost]))
```

这种方法可以通过在 Security 表中为每个角色设置一行，并使用诸如 [Column] = "Sales Person"、[Column] = "Marketing" 和 [Column] = "Accounting" 等相应的 RLS 规则，来扩展以处理需要不同列安全性的多个角色。DAX 度量将根据需要进行调整。请注意，虽然值被隐藏，但受保护列的元数据在模型中仍然可用。

虽然无法通过 Power BI Desktop 实现 OLS，但可以使用 Tabular Editor 代替来编辑表或列的权限。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_187_0.png)

图 3.43：为表分配 OLS

- 4. 对 Dates 表重复步骤 3。
- 5. 打开 SQL Server Management Studio 并连接到源数据库。
- 6. 从 SQL Server Management Studio 的 Tools 菜单打开 SQL Server Profiler 并运行跟踪。
- 7. 创建一个表可视化，使用 Customers 表中的 EnglishEducation 列和 Internet Sales 表中的 SalesAmount 列。请注意，跟踪中没有记录任何内容。
- 8. 创建一个聚类柱形图，使用 Dates 表中的 CalendarYear 列作为轴，使用 Internet Sales 表中的 SalesAmount 列作为值。同样，没有记录任何内容。

## 其工作原理

核心事实表 Internet Sales 必须设置为 DirectQuery 存储模式至关重要，因为聚合表仅适用于设置为 DirectQuery 存储模式的明细表。聚合表本身 Sales Agg 必须设置为 Import 存储模式。与 Internet Sales 表和 Sales Agg 表都有关系的二维表 Customers 和 Dates，应设置为 Dual 存储模式，以便这些表可以在任一模式下最佳运行。配置表的聚合会自动在语义模型中隐藏该表，因为聚合表必须被隐藏。

必须在维度表和聚合表之间存在关系。这是因为聚合表在可能的情况下用作详细 DirectQuery 模式事实表的替代，因此聚合表应与事实表对维度表具有相同的关系。

在配置聚合时，还必须考虑列数据类型。这是因为 Manage aggregations 对话框强制执行 Detail Column 必须与 Aggregation Column 具有相同数据类型的规则。该规则的唯一例外是 Count 和 Count table rows 的汇总。Count 和 Count table rows 需要整数聚合列，但不需要 Detail Column 的数据类型匹配。

其他限制包括链式聚合、使用 DAX USERELATIONSHIP 函数支持的不活动关系，以及使用相同的 Summarization、Detail Table 和 Detail Column 的重复聚合。都不支持这些场景。

## 还有更多…

为了正常运行，RLS 表达式应同时过滤明细表和聚合表。事实上，一旦对表设置了聚合，仅过滤聚合表而不过滤关联明细表的 RLS 表达式将被 Manage roles 对话框禁止。

Manage aggregations 对话框包括设置 Precedence 的功能。这意味着您可以拥有不同粒度级别的多个聚合表，并设置这些聚合表的 Precedence 以允许它们被单个子查询考虑。例如，如果 Products 表包含在此模型中，您可能会为 Product Category 创建一个聚合表，并为 Product Subcategory 创建一个单独的聚合表。

## 准备工作

要准备本配方，请按照以下步骤操作：

- 1. 打开 Power BI Desktop。
- 2. 点击功能区 Home 选项卡上的 Transform data。
- 3. 在 Power Query Editor 中，点击功能区 Home 选项卡上的 Manage parameters。
- 4. 创建一个 Type 为 Date/Time 且 Current Value 为 1/1/2013 12:00:00 AM 的 RangeStart 参数。
- 5. 创建一个 Type 为 Date/Time 且 Current Value 为 12/31/2014 12:59:59 PM 的 RangeEnd 参数。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_193_0.png)

图 3.48：Manage Parameters 对话框

- 6. 创建一个名为 Dates 的 DirectQuery 模式查询，从 AdventureWorksDW2022 数据库加载 DimDate 表。
- 7. 创建一个名为 Internet Sales 的 Import 模式查询，从 AdventureWorksDW2022 数据库加载 FactInternetSales 表。

8. 对于 Internet Sales 查询，导航到 OrderDate 列，点击列标题中的下拉箭头，然后选择 Date/Time Filters 和 Custom Filter...。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_194_0.png)

图 3.49：创建自定义筛选器

9. 按图 3.50 所示配置 Filter Rows 对话框并点击 OK 按钮：

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_195_0.png)

图 3.50：Filter Rows 对话框

10. 点击功能区 Home 选项卡上的 Close & Apply 按钮以加载数据。

在 Table view 中，确认您的 Internet Sales 表中有一部分行——您应该只有 54,771 行数据，而不是完整的 60,398 行。

## 如何利用混合表

要实施本配方，请使用以下步骤：

- 1. 切换到 Model view。
- 2. 注意 Dates 表与 Internet Sales 表之间指示的弱关系，关系末端已断开。这是因为 Dates 表处于 DirectQuery 存储模式，而 Internet Sales 表处于 Import 存储模式。
- 3. 右键点击 Internet Sales 表并选择 Incremental refresh。

