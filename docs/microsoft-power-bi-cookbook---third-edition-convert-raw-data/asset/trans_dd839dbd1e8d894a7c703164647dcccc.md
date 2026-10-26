
尽管物理关系在性能和管理性方面是首选的长期解决方案，但在物理关系不可行时，虚拟关系提供了一种有吸引力的替代方案。在本方案中，我们提供了使用自定义性能分段表和聚合表的虚拟关系示例。

## 准备工作

要为该方案做准备，请执行以下步骤：

- 1. 打开 Power BI Desktop。
- 2. 创建一个名为 AdventureWorksDW2022 的导入模式数据源查询。有关参考，请参阅本章中“实现语义模型”方案的“准备工作”部分。
- 3. 将此查询隔离到名为“Data Sources”的查询组中，并禁用加载。
- 4. 右键单击 AdventureWorksDW2022，然后选择“引用”。
- 5. 选择 FactInternetSales 表，并将此查询命名为“Internet Sales”。
- 6. 重复步骤4和5，创建一个导入 DimDates 的“Dates”查询。
- 7. 创建一个“Customers”查询。有关参考，请参阅本章中“配置模型元数据”方案的“准备工作”中的步骤7。
- 8. 基于 DimProduct、DimProductCategory 和 DimProductSubcategory 表创建一个“Products”查询。有关参考，请参阅本章中“创建关系”方案的“准备工作”中的步骤7。
- 9. 将“Internet Sales”查询移动到名为“Facts”的查询组，并将“Dates”、“Products”和“Customers”查询移动到名为“Dimensions”的查询组。
- 10. 使用“输入数据”查询在“Other Queries”组中创建一个“GrowthTiers”查询。

## 创建表

|  | 增长层级键 | 增长层级 | 最小值 | 最大值 | + |
|---|---|---|---|---|---|
| 1 | 1 | 问题 | -1 | -.25 |  |
| 2 | 2 | 表现不佳 | -.25 | 0 |  |
| 3 | 3 | 平均 | 0 | .25 |  |
| 4 | 4 | 表现优异 | .25 | 10 |  |
| + |  |  |  |  |  |

图3.33：销售增长分段表，GrowthTiers

11. 在 Other Queries 组中创建一个 SubCatPlan 查询，代码如下：

```
let
    Source = Products,
    Sales =
        Table.ExpandTableColumn(
            Source, "FactInternetSales",
            {"SalesAmount", "OrderDate"}, {"SalesAmount", "OrderDate"}
        ),
    RemoveNulls = Table.SelectRows(Sales, each [SalesAmount] <> null),
    Year =
        Table.AddColumn(
            RemoveNulls, "Calendar Year", each Date.Year([OrderDate])
        ),
    Month =
        Table.AddColumn(
            Year, "Calendar Month", each Date.MonthName([OrderDate])
        ),
    Group =
        Table.Group(
            Month,
            {"Calendar Year", "Calendar Month", "Product Subcategory"},
            {{"SalesAmount", each List.Sum([SalesAmount]), type nullable number}}
        ),
    Index = Table.AddIndexColumn(Group, "Index", 1, 1, Int64.Type),
    Plan =
        Table.AddColumn(
            Index, "Plan Amt",
            each if Number.IsEven([Index])
                then 1/Number.RandomBetween(5,10) * [SalesAmount] + [SalesAmount]
                else (-1 * 1/Number.RandomBetween(5,10) * [SalesAmount]) + [SalesAmount]
        ),
    Remove = Table.RemoveColumns(Plan,{"SalesAmount", "Index"}),
    Currency = Table.TransformColumnTypes(Remove,{{"Plan Amt", Currency.Type}})
    in
    Currency
```

12. 在 Other Queries 组中创建一个名为“Calculations”的空白查询。

13. 单击“开始”选项卡功能区中的“关闭并应用”，将表加载到语义模型。

14. 创建如图3.34所示的单一交叉筛选关系；请注意，GrowthTiers 或 SubCatPlan 表没有关系：

| 启用 | 从：表（列） | 到：表（列） |
| --- | --- | --- |
| ☑ | Internet Sales (CustomerKey) | Customers (CustomerKey) |
| ☐ | Internet Sales (DueDateKey) | Dates (DateKey) |
| ☑ | Internet Sales (OrderDateKey) | Dates (DateKey) |
| ☑ | Internet Sales (ProductKey) | Products (ProductKey) |
| ☐ | Internet Sales (ShipDateKey) | Dates (DateKey) |

图3.34：模型关系

15. 切换到“表”视图，并隐藏“Calculations”表中的单列。

16. 在“Calculations”表中创建以下三个度量：

```
Sales Current Year =
    CALCULATE(SUM('Internet Sales'[SalesAmount]),
        FILTER(ALL EXCEPT('Internet Sales','Customers'),
            YEAR('Internet Sales'[OrderDate]) = 2012 ) )
Sales Previous Year =
    CALCULATE(
        SUM('Internet Sales'[SalesAmount]),
        FILTER(ALL EXCEPT('Internet Sales','Customers'),
            YEAR('Internet Sales'[OrderDate]) = 2011 )
Sales Growth =
    DIVIDE([Sales Current Year] - [Sales Previous Year],
        [Sales Previous Year], BLANK() )
```

17. 右键单击“Date”表，然后选择“标记为日期表”，使用 FullDateAlternateKey 作为日期列。

## 如何支持虚拟表关系

要实现本方案，请使用以下步骤：

- 1. 在表视图中，在“Calculations”表中创建“Sales Growth Tier”度量：

```
Sales Growth Tier =
    VAR Growth = [Sales Growth]
    VAR Level1 =
        CALCULATE(MIN('GrowthTiers'[Max]), 'GrowthTiers'[GrowthTierKey] = 1)
    VAR Level2 =
```

## 更多内容...

DAX TREATAS 函数于 2017 年初加入，与涉及 INTERSECT 或 FILTER 使用 CONTAINS 函数参数的替代虚拟关系方法相比，它提供了更简单的语法和更好的性能。我们可以使用 TREATAS 函数将汇总表 SubCatPlan 集成到 Power BI 语义模型中。当业务想要通过其经常使用的相同 Products 和 Dates 表筛选计划数据，并创建实际与计划对比报告时，这非常有用。SubCatPlan 表的粒度是按日历年和日历月的产品子类别；如图 3.37 所示。

| 产品子类别 | 日历年份 | 日历月份 | 计划金额 |
|---|---|---|---|
| 山地自行车 | 2013 | 一月 | $313,596.6261 |
| 水壶和水壶架 | 2013 | 一月 | $2,284.3156 |

图3.37：用于虚拟关系的计划汇总表

由于多对多关系，无法直接创建从 SubCatPlan 表到 Date 和 Product 表的关系。Power BI 中的每个物理关系都基于每个表中的单个列，其中一个关系列唯一标识其宿主表的所有行。这与 SSAS 表格模型和 Excel 的 Power Pivot 模型一致。

要使用 DAX 创建虚拟表关系，请执行以下步骤：

- 1. 在“Calculations”表中创建“Subcat Plan Amt”度量。此度量通过 Products 表和 Dates 表筛选子类别计划度量：

```
Subcat Plan Amt =
    VAR ProductSubCats = VALUES('Products'[Product Subcategory])
    VAR DateTbl =
        SUMMARIZE('Dates','Dates'[CalendarYear],'Dates'[EnglishMonthName])
    RETURN
        CALCULATE(
            SUM('SubCatPlan'[Plan Amt]),
            TREATAS(ProductSubCats,'SubCatPlan'[Product Subcategory]),
            TREATAS(DateTbl,'SubCatPlan'[Calendar Year],'SubCatPlan'[Calendar Month]))
```

- 2. 创建一个“Sales vs Plan”度量：

```
Sales vs Plan = SUM('Internet Sales'[SalesAmount]) - [Subcat Plan Amt]
```

- 3. 创建一个矩阵可视化，以 Products 表中的“Product Subcategory”列作为行，Dates 表中的“EnglishMonthName”列作为列，并将“Subcat Plan Amt”度量、“Sales vs Plan”度量以及 Internet Sales 表中“SalesAmount”列的总和作为值。

| EnglishMonthName | January |  | February |  |
| 产品子类别 | Subcat Plan Amt | SalesAmount | Sales vs Plan | Subcat Plan Amt | SalesAmount |
| 自行车挂架 | $1,362 | $1,560 | $198 | $2,200 | $2,520 |
| 自行车支架 | $1,111 | $1,272 | $161 | $2,946 | $2,544 |

*图3.38：子类别计划与实际对比的矩阵可视化*

在“Subcat Plan Amt”度量中，变量用于存储代表 Products 和 Dates 维度表筛选值的表。TREATAS 函数将这些变量传递到相应的计划列，从而筛选 SubCatPlan 表。任何粒度与 Product Subcategory、Calendar Year 和 Calendar Month 列相同（或更低）的列都可以使用。来自其他表的列或没有虚拟关系的列不会筛选 SubCatPlan 表。

在这种情况下，可以创建桥接表来支持与 Products 和 Dates 表之间的物理关系。这两个桥接表将包含唯一的产品子类别和月份值，一对多关系将桥接表链接到 SubCatPlan、Product 和 Date 表。SubCatPlan 和桥接表可以从报表视图中隐藏，并且会在桥接表与 Products 和 Dates 表之间配置双向关系。为了获得更好的性能和管理性，建议使用物理关系而不是虚拟关系。

## 另请参阅

- DAX 函数参考：http://bit.ly/3rrQF1t
- DAX 中的物理和虚拟关系：http://bit.ly/2oFpe8T

## 设计列级安全性

虽然不像行级安全性（RLS）那么常见，但有时你可能希望向一组报表查看者隐藏语义模型中的特定列，而不向其他查看者隐藏。

虽然在 Power BI Desktop 中不可用，但 Analysis Services 表格语义模型确实支持对象级安全性（OLS）来保护表和列。这种保护对象的方法目前需要第三方工具，例如 Tabular Editor。此外，此方法还有一项优势，即连对象元数据也一并保护，这样没有访问权限的报表查看者甚至不会知道模型中存在这些表和列。

然而，OLS 也有严重的缺点。例如，将受 OLS 保护的列包含在视觉对象或度量中，对于无法访问该 OLS 保护列的用户来说，该视觉对象或度量将无法使用。因此，要为有权和无权访问某些表和列的各种用户创建可用的报表，即使并非不可能，也会变得非常困难。这个问题如此严重，以至于你很可能得出结论：不值得为此花费时间和精力，而报表确实应该是独立的语义模型。

本方案演示了一种在 Power BI Desktop 中实现列级安全性的方法，该方法不需要 OLS，并且可在任何许可级别下在 Power BI Desktop 中原生运行。虽然受保护列的元数据对报表查看者可见，而无需访问受保护列的值，但此方法的优点是更容易编写度量和报表视觉对象，这些视觉对象可以在模型内的任何安全级别下正常工作。
