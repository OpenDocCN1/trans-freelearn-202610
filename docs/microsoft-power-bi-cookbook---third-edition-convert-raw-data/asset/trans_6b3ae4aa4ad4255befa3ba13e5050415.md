

```
),
AddCustomerMonth =
    Table.AddColumn(
        AddCustomerYear, "Customer Month", each Date.Month([BirthDate]),
        Int64.Type
    ),
AddCustomerDay =
    Table.AddColumn(
        AddCustomerMonth, "Customer Day", each Date.Day([BirthDate]),
        Int64.Type
    )
```

5. 使用 if...then 表达式添加 Age 列。

```
CustomerAge =
    Table.AddColumn(
        AddCustomerDay,"Customer Age", each
        if [Customer Month] < CurrentMonth
            then CurrentYear - [Customer Year]
        else if [Customer Month] > CurrentMonth
            then CurrentYear - [Customer Year] - 1
        else if [Customer Day] < CurrentDay
            then CurrentYear - [Customer Year]
        else CurrentYear - [Customer Year] - 1
    )
```

6. 通过步骤 5 中计算的列添加 Customer Age Segment 列。

```
CustomerSegment =
    Table.AddColumn(
        CustomerAge, "Customer Age Segment", each
        if [Customer Age] < 30 then "Less than 30"
        else if [Customer Age] < 40 then "30 to 39"
        else if [Customer Age] < 50 then "40 to 49"
        else if [Customer Age] < 60 then "50 to 59"
        else "60 or Older"
    )
```

7. 修改查询末尾的 in 子句以返回 CustomerSegment。

完整的查询可在本教程的 PBIX 文件中找到。

## 更多内容

Power Query 编辑器提供了用于添加列的图形用户界面。这些界面提供了添加列的机制，作为手动编写代码的替代方案。本质上，添加列的 M 代码是这些界面生成的输出。

其中一种界面是**从示例创建列**功能，用户只需输入所需列输出的示例。引擎会确定需要向查询中添加哪些 M 函数和步骤序列，以返回与所提供的示例一致的结果。要探索此功能，请按照以下步骤操作：

- 1. 创建一个引用 AdventureWorksDW2022 查询的新查询。
- 2. 选择 DimCustomer 表。
- 3. 选择 Title、FirstName、MiddleName、LastName 和 BirthDate 列，并删除所有其他列。
- 4. 在功能区中选择“添加列”选项卡，然后单击“从示例创建列”按钮。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_109_0.png)

*图 2.31：从示例创建列功能*

- 5. 在第一行输入客户的 FirstName、MiddleName 和 LastName 值，然后按 Enter 键。请注意，其余行将根据这第一行自动计算。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_109_1.png)

*图 2.32：从示例创建列界面*

- 6. 单击“确定”按钮以接受转换。

另一种添加列的界面是“条件列”功能。此功能提供了一种创建条件列的方法，作为编写 if...then 表达式的替代方案。要了解此功能的运作方式，请按照以下步骤操作：

- 1. 选择“添加列”选项卡，然后从功能区中选择“条件列”。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_110_0.png)

图 2.33：条件列功能

- 2. 在“添加条件列”对话框中填写字段，使用“添加子句”按钮添加额外的 else if 语句；单击“确定”按钮退出对话框并创建新列。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_110_1.png)

图 2.34：添加条件列对话框

可以引用表中的任何列，并且可以使用省略号 (...) 在评估顺序中上下移动多个已创建步骤。打开高级编辑器以检查创建的代码。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_111_0.png)

*图 2.35：添加的条件列 M 代码*

## 另请参阅

- - `Table.AddColumn`：http://bit.ly/3vGJZ6b
- - `Table.SelectColumns`：http://bit.ly/38Qk7Lt
- - DateTime 函数：http://bit.ly/3tPtKlJ
- - 从示例添加列：http://bit.ly/3eWTLv6

## 组合和合并查询

Power BI 查询功能的全部力量在于通过其合并和追加转换，将代表不同数据源的不同查询集成在一起。可以高效且安全地开发整合来自多个网络位置的文件或集成来自多个数据源的数据的检索过程。此外，使用 M 语言可以实现 SQL 和 ETL 开发人员所熟悉的相同连接类型和数据转换模式。本教程提供了将多个源合并到单个查询中并利用 M 语言的表连接函数来支持常见转换场景的示例。

## 准备工作

要遵循本教程，您可以使用 Power Query 编辑器“**主页**”选项卡上的“**合并查询**”和“**追加查询**”图标来生成本教程中使用的连接表达式。但是，由于连接查询是检索过程的基础，建议学习如何使用 `Table.Join`、`Table.NestedJoin` 和 `Table.Combine` 函数。

要为本教程做好准备，请执行以下操作，从 AdventureWorksDW2022 数据库中导入 `DimCustomer` 和 `FactInternetSales` 表：

- 1. 打开 Power BI Desktop，然后从“**主页**”选项卡的功能区中选择“**转换数据**”以打开 Power Query 编辑器。
- 2. 创建一个名为 `AdventureWorksDW2022` 的“**导入**”模式数据源查询。此查询应类似于以下内容：

```m
let
    Source = Sql.Database("localhost", "AdventureWorksDW2022")
in
    Source
```

- 3. 将此查询隔离在名为“**数据源**”的查询组中，并禁用查询的加载。
- 4. 右键单击 `AdventureWorksDW2022` 查询并选择“**引用**”，在数据预览区域中选择 `DimEmployee` 表，并将此查询重命名为 `Employees Staging`。
- 5. 重复*步骤 4*，但选择 `FactInternetSales` 表，并将此查询命名为 `Sales 2011`。筛选 `OrderDate` 列，使其介于 2011 年 1 月 1 日和 2011 年 12 月 31 日之间。这可以在用户界面中通过单击 `OrderDate` 列标题中的向下箭头图标，选择“**日期/时间筛选器**”，然后选择“**介于...**”来完成。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_112_0.png)

```
SelectCols =
    Table.SelectColumns(
        ManagerName,
        { "EmployeeKey", "Manager Name", "Title" }
    )
in
    SelectCols
```

- 11. 创建一个新查询，使用 `Table.NestedJoin` 函数或功能区中的“合并查询”功能来连接 Managers 查询和 Employees Staging 查询。将此查询命名为 Employees，并将其放在“其他查询”组中。

```
let
    Source = Table.NestedJoin(#"Employees Staging", {"ParentEmployeeKey"}, Managers, {"EmployeeKey"}, "Managers", JoinKind.LeftOuter)
in
    Source
```

- 12. 使用 `Table.ExpandTableColumn` 函数添加 Manager Name 和 Manager Title 列。

```
#"Expanded Managers" = Table.ExpandTableColumn(Source, "Managers", {"Manager Name", "Title"}, {"Manager Name", "Manager Title"})
```

- 13. 添加 Employee Name 列。

```
EmployeeName =
    Table.AddColumn(
        #"Expanded Managers",
        "Employee Name", each [FirstName] & " " & [LastName]
    )
```

- 14. 将 Title 列重命名为 Employee Title。

```
#"Renamed Columns" = Table.RenameColumns(EmployeeName,{{"Title", "Employee Title"}})
```

- 15. 仅选择 EmployeeKey、Employee Name、Employee Title、Manager Name 和 Manager Title 列，并删除所有其他列。

```
#"Removed Other Columns" = Table.SelectColumns(#"Renamed Columns",{"EmployeeKey", "Employee Title", "Manager Name", "Manager Title", "Employee Name"})
```

## 工作原理

对于 `InternetvSalesv2011-2013` 查询，只需使用 `Table.Combine` 函数的一个表达式。在此示例中，无需其他表达式，因为暂存查询具有相同的列名和相同数量的列。`Table.Combine` 函数执行追加操作，并且不删除重复行，类似于 SQL UNION 语句。

在 `Table.Combine` 函数中，输入表中任一表独有的列都将添加到结果集中，其他表的行对应的值为 null。根据具体情况，开发人员可以应用 `Table.Distinct` 函数来避免任何重复行进入数据模型。

`Employees` 查询在 `Table.NestedJoin` 函数中引用 `Employees Staging` 查询作为左表，并通过左外部连接与 `Managers` 查询连接。在此场景中，需要进行左连接以保留所有员工行，因为 `DimEmployees` 表包含一个没有父员工键的员工，即首席执行官。

连接在 `DimEmployees` 查询中的 `ParentEmployeeKey` 列与 `Managers` 查询中的 `EmployeeKey` 列之间进行。此步骤之后，来自匹配 `Managers` 表的所有行将作为 `Table` 对象存储在 `Managers` 列中。使用 `Table.ExpandTableColumn` 函数展开 `Managers` 列时，`Manager Name` 列可以保留相同的名称，但 `Title` 列被重命名为 `Manager Title`，以避免与 `Employees Staging` 查询中的 `Title` 列冲突。

在实现表连接时，您可以选择使用 `Table.Join` 和 `Table.NestedJoin` 函数。这两个函数都支持所有六种连接类型——内连接、左外部、右外部、完全外部、左反和右反。`Table.NestedJoin` 函数将连接结果（右表或第二个表）输入到一个包含表值的新列中，并将使用本地资源执行连接操作，除非使用 `Table.ExpandTableColumn` 函数用右表的列替换此新列。如果未指定 `JoinKind` 参数，则默认连接类型为左外部。出于性能原因，不应在未使用 `Table.ExpandTableColumn` 函数移除表列的情况下使用 `Table.NestedJoin`。

相反，`Table.Join` 函数会自动使用右表输入中的列展开左表（扁平连接），如果未指定 `JoinKind` 参数，则默认为内连接。`Table.Join` 函数无需任何附加函数即可折叠到源，但对于内连接以外的 `JoinKind`，连接的表之间不能有匹配的列名。对于内连接，必须在连接键参数中指定两个表中匹配的列名。需要使用 `Table.SelectColumns` 函数来排除随连接添加的右表中的任何列。

无论是通过 `Table.NestedJoin` 还是 `Table.Join` 实现，如果源表具有引用完整性（例如外键约束），并且这符合要求，开发人员都应考虑使用内连接。对于针对较大表的连接，开发人员应确认是否发生查询折叠，并且可以在性能方面评估替代检索设计生成的不同查询计划。

