

请注意，Rob Walters 的两行记录是由于源数据库中应用了缓慢变化维度（SCD）类型 2 过程所致。有关 SCD 的更多信息，请参阅此维基百科文章：https://bit.ly/3yIQeI5。

## 还有更多…

与其创建单独的查找/连接暂存查询，不如将这些表达式合并为单个 let...in M 表达式。例如，以下单个查询返回的结果与 Sales 2011、Sales 2012、Sales 2013 和 Internet Sales 2011–2013 查询完全相同：

```
let
    Source = AdventureWorksDW2022,
    Sales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
    Sales2011Rows =
        Table.SelectRows(
            Sales, each
                [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and
                [OrderDate] <= #datetime(2011, 12, 31, 0, 0, 0)
        ),
    Sales2012Rows =
        Table.SelectRows(
            Sales, each
                [OrderDate] >= #datetime(2012, 1, 1, 0, 0, 0) and
                [OrderDate] <= #datetime(2012, 12, 31, 0, 0, 0)
        ),
    Sales2013Rows =
        Table.SelectRows(
            Sales, each
                [OrderDate] >= #datetime(2013, 1, 1, 0, 0, 0) and
                [OrderDate] <= #datetime(2013, 12, 31, 0, 0, 0)
        ),
    Append = Table.Combine( {Sales2011Rows, Sales2012Rows, Sales2013Rows} )
in
    Append
```

内联查询方法有助于限制查询数量，但你会失去组文件夹和查询依赖关系视图提供的管理优势。图形化支持使得解释和快速排查数据检索过程变得更容易，而无需使用单个但复杂的 M 表达式。

暂存查询建议用于除最简单项目和检索过程之外的所有情况。暂存查询通常不应加载到数据模型中，因为暂存表可能会让用户感到困惑，并且会要求数据模型使用额外的资源来处理和存储额外的数据。

同样，合并查询也可以合并为单个查询。下表分解了可在 `Table.NestedJoin` 和 `Table.Join` 函数中指定的六种不同连接类型。参数和参数值都可以使用，尽管本书中的配方使用参数，因为这样表达式更容易理解。

| Join type | Parameter | Parameter value |
|---|---|---|
| Inner | `JoinKind.Inner` | 0 |
| Left Outer | `JoinKind.LeftOuter` | 1 |
| Right Outer | `JoinKind.RightOuter` | 2 |
| Full Outer | `JoinKind.FullOuter` | 3 |
| Left Anti | `JoinKind.LeftAnti` | 4 |
| Right Anti | `JoinKind.RightAnti` | 5 |

*Table 2.3: Power Query (M) join types, parameters, and parameter values*

最后一点需要注意的是，对于存储在同一网络目录文件夹中且结构相同的源数据文件，Power BI 提供了 Combine Binaries 转换，可与文本、CSV、Excel、JSON 和其他文件格式一起使用。此功能可在创建文件夹查询时使用。Combine Binaries 功能会自动创建示例查询和与该查询关联的函数，以便可将对源文件的任何必要修改应用于所有文件，并且可以轻松修改文件源位置。

## 另请参阅

- `Table.Combine`: http://bit.ly/3c6L2o0
- `Table.NestedJoin`: http://bit.ly/30Z03jZ
- `Table.Join`: http://bit.ly/31xMRh7
- `Table.ExpandTableColumn`: http://bit.ly/3eY6u0z
- `Table.AddColumn`: http://bit.ly/3vGJZ6b
- `Table.SelectColumns`: http://bit.ly/38Qk7Lt
- `Table.RenameColumns`: http://bit.ly/3rTVfd4
- M table function reference: http://bit.ly/2oj0k0I
- Combining binaries in Power BI Desktop: http://bit.ly/2oL2nM4

## 分析源数据

在开发任何语义模型和报表之前，尤其是在业务利益相关者开始使用内容获取洞察之前，评估和验证源数据质量是一个良好的实践。数据质量分析练习可以揭示各种问题的存在，从某些列中存在空值或空白值，到重复行，到缺乏唯一或标识（主键）列。此练习的结果可为是否可以清理源系统数据本身，或 BI 解决方案是否将通过 Power Query (M) 数据清理功能可用的各种方法来解决这些问题提供决策依据。

数据质量主题涉及语义模型的整体实用性，以及为某些目的轻松处理和使用数据（包括分析和报告）的能力。数据质量是数据治理的重要组成部分，确保业务数据准确、完整、一致且有效。良好的数据质量是任何数据分析和报告工作的基本要素。糟糕的数据质量可能导致企业做出错误的分析和决策——因此有“垃圾进，垃圾出”这一说法。幸运的是，Power Query Editor 提供了强大的数据分析工具，可帮助快速确定所处理数据的质量。本配方演示如何解锁 Power Query Editor 中的强大工具来分析列并了解所处理数据的质量。

## 准备工作

要为本配方做好准备，请通过执行以下操作从 AdventureWorksDW2022 数据库导入 FactFinance 表：

- 1. 打开 Power BI Desktop 并从 **Home** 选项卡的 ribbon 中选择 **Transform data** 以打开 Power Query Editor。
- 2. 创建一个名为 AdventureWorksDW2022 的导入模式数据源查询。此查询应类似于以下内容：
- 3. 通过右键单击 **Queries** 窗格中的查询并选择 **Move To Group | New Group** 将此查询隔离到名为 Data Sources 的查询组中。
- 4. 右键单击 AdventureWorksDW2022 查询并选择 **Reference**。
- 5. 选择 Data 列中的 FactFinance 表链接并将查询重命名为 Finances。
- 6. 将 Finances 查询移动到 Other Queries 组。

```
let
    Source = Sql.Database("localhost", "AdventureWorksDW2022")
in
    Source
```

有关执行这些步骤的更多详细信息，请参阅本章中的 *Importing Data* 配方。

## 如何分析源数据

要实现本配方，请执行以下步骤：

- 1. 选择 Finances 查询。
- 2. 在 View 选项卡中，勾选 Data Preview 区域 ribbon 中的 Column profile 复选框。
- 3. 选择一列以查看列统计信息，类似于图 2.37 中 Amount 列的统计信息：

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_119_0.png)

*Figure 2.37: Column profile information*

## 其工作原理

默认情况下，M 查询引擎从指定的数据源提取前 1,000 行数据。这是数据预览区域中使用的数据。仅使用前 1,000 行数据的默认值可以通过单击右下角状态栏中的 Column profiling based on top 1000 rows 并选择 Column profiling based on the entire data set 来更改。

对数据源中的每个列收集统计信息。当启用 Column profile 功能时，选择列会导致列统计信息和值分布显示在数据预览区域的下半部分，包括错误计数、不同值和唯一值的数量、最小值、最大值，以及取决于列数据类型而显示的额外信息，如图 2.37 所示。

## 还有更多...

Power Query Editor 中还提供了额外的<|message|> 数据质量对话框。从 **View** 选项卡中，勾选 **Column quality** 和 **Column distribution** 旁边的复选框。小视觉对象将出现在列标题下方。将鼠标悬停在此区域上会提供一个弹出对话框，提供建议操作，以及通过省略号菜单 (...) 进行常见数据清理活动。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_120_0.png)

*Figure 2.38: Column quality and Column distribution visuals*

## 另请参阅

- Using the data profiling tools: `http://bit.ly/3c465s5g`

## 诊断查询

Power Query M 引擎是一个极其强大且快速的数据转换和数据准备引擎，用于一系列产品中，包括：

- Excel for Windows
- Excel for Mac
- Power BI

