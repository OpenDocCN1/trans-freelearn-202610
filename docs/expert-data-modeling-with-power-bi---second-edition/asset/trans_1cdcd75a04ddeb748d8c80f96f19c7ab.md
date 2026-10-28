

Figure 12.5: 连接到 SharePoint Online 文件夹

- 4. 粘贴步骤 1 中复制的站点 URL。
- 5. 单击 OK。

Figure 12.6: 使用 SharePoint Online 站点 URL 连接到 SharePoint Online 文件夹

- 6. 选择 **Microsoft account** 部分。
- 7. 单击 **Sign in** 按钮并输入您的凭据。
- 8. 单击 **Connect**。

![](img/5d2bce74fd6fceebed14fd1488bf7088_681_0.png)

Figure 12.7: 输入 Microsoft 账户凭据以连接到 SharePoint Online 文件夹

- 9. 单击 **Transform Data** 按钮。

![](img/5d2bce74fd6fceebed14fd1488bf7088_681_1.png)

Figure 12.8: 在 Power Query Editor 中转换数据

- 10. 单击 **Folder Path** 列的筛选下拉菜单。
- 11. 找到托管 Excel 文件的 **Products List** 文件夹并选择它。

- 12. 将查询重命名为 Product。
- 13. 单击 OK。

![](img/5d2bce74fd6fceebed14fd1488bf7088_682_0.png)

Figure 12.9: 筛选结果以显示 Products List 文件夹

到目前为止，我们已经连接到 SharePoint Online 文件夹。下一步是合并 Excel 文件：

- 14. 从 Content 列中单击 Combine Files 按钮。

![](img/5d2bce74fd6fceebed14fd1488bf7088_682_1.png)

Figure 12.10: 合并 Excel 文件

- 15. 选择 ProductList 表。
- 16. 单击 OK。

![](img/5d2bce74fd6fceebed14fd1488bf7088_683_0.png)

Figure 12.11: 在 Combine Files 窗口中选择要从每个文件中提取的对象

上述过程创建了若干个分组到不同文件夹中的查询，如以下图像所示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_683_1.png)

Figure 12.12: 合并 Excel 文件的结果

合并后的数据结果显示了一个不必要的 **Source.Name** 列，可以将其删除：

- 17. 删除 **Source.Name** 列：

![](img/5d2bce74fd6fceebed14fd1488bf7088_684_0.png)

Figure 12.13: 删除 Source.Name 列后的结果

到目前为止，我们已成功合并了 Excel 文件。现在，我们只想保留最新的数据。在接下来的几个步骤中，我们将实现一种机制来识别最新数据并将其加载到数据模型中。但在继续之前，让我们花点时间仔细查看一下数据。以下图像中显示的结果按 **ProductNumber** 排序，并且移除了 **Product Category** 和 **Product Subcategory** 列以更好地了解数据的变化，但这些步骤完全不必要，仅用于演示目的。以下图像显示了结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_684_1.png)

Figure 12.14: 具有不同 Reporting Dates 的重复 ProductNumbers

如前一图像所示，多个产品出现在多个列表中。目标是仅根据 Reporting Date 保留最新的产品数据。因此我们应获取 ProductNumber 和 Reporting Date 的最大值。

为实现此目标，我们在 Power Query Editor 中使用 Group By 功能。使用 UI 中的 Group By 会在 Power Query 中使用 Table.Group() 函数。请按照以下步骤操作：

- 18. 右键单击 ProductNumber 列。
- 19. 从上下文菜单中单击 Group By 列。
- 20. 为新列名称输入 Reporting Date。
- 21. 从 Operation 下拉菜单中选择 Max。
- 22. 从 Column 下拉菜单中选择 Reporting Date。
- 23. 单击 OK。

![](img/5d2bce74fd6fceebed14fd1488bf7088_685_0.png)

Figure 12.15: 在 Power Query 中按列分组

以下图像显示了结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_686_0.png)

Figure 12.16: 按列分组的结果

现在我们有了所有产品编号及其最新的报告日期。难题的唯一剩余部分是将 Grouped Rows 步骤的结果与上一步的数据连接起来。为此，我们使用 Merge Queries 选项，后台会运行 Table.NestedJoin() 函数。请按照以下步骤操作：

- 24. 选择 Grouped Rows 步骤。
- 25. 从 Home 选项卡中单击 Merge Queries 按钮。
- 26. 从下拉菜单中选择 Product (Current) 表。注意，我们选择的是当前查询（Product）。
- 27. 在顶部表中，按住键盘上的 Ctrl 按钮并依次选择 ProductNumber 和 Reporting Date 列。
- 28. 对底部表执行相同操作。注意选择列的顺序很重要。
- 29. 确保 Join Kind 为 Left Outer（全部来自第一个，匹配来自第二个）。
- 30. 单击 OK。

![](img/5d2bce74fd6fceebed14fd1488bf7088_687_0.png)

Figure 12.17: 左外连接（合并）Product 查询自身

在前面的操作中，Grouped Rows 是我们将其结果与其自身连接的最后一个转换步骤。我们已在第 5 章《Common Data Preparation Steps》的 Merging queries 部分解释了 Table.NestedJoin() 函数的工作原理。经过步骤 24 到 30 后，Power Query Editor 生成的代码如下：

```
Table.NestedJoin(#"Grouped Rows", {"ProductNumber", "Reporting Date"}, #"Grouped Rows", {"ProductNumber", "Reporting Date"}, "Grouped Rows", JoinKind.LeftOuter)
```

但我们希望将 Grouped Rows 转换步骤的结果与 Removed Columns 步骤的结果连接起来。因此我们将前面的表达式修改如下：

```
Table.NestedJoin(#"Grouped Rows", {"ProductNumber", "Reporting Date"}, #"Removed Columns", {"ProductNumber", "Reporting Date"}, "Grouped Rows", JoinKind.LeftOuter)
```

连接 Grouped Rows 与 Removed Columns 步骤后，我们需要展开 Grouped Rows 列。请按照以下步骤操作：

- 31. 单击 Grouped Rows 列上的 Expand 按钮。
- 32. 取消选择 ProductNumber 和 Reporting Date 列以保留其他列被选中。
- 33. 取消勾选 Use original column name as prefix 选项。
- 34. 单击 OK。

![](img/5d2bce74fd6fceebed14fd1488bf7088_688_0.png)

Figure 12.18: 展开 Grouped Rows 列

全部完成！以下图像显示了最终结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_688_1.png)

Figure 12.19: 实现 SCD 1 的最终结果

我们可以单击 Close & Apply 按钮将数据加载到数据模型中，发布到 Power BI 服务并安排自动数据刷新。这样，当新的 Excel 文件（新的 Product 列表）上传到 SharePoint Online 时，Power BI 会经过上述转换步骤，以确保我们始终将最新的 Product 数据加载到数据模型中，这与 SCD 1 的行为非常相似。下一节将解释 SCD 类型 2。

## SCD type 2 (SCD 2)

这种类型的 SCD 在业务需要保留员工的历史和当前数据时，我们在数据仓库中保留数据变化的历史。在 SCD 2 场景中，我们必须根据业务需求维护历史数据，这些需求决定了我们如何实现这种类型的 SCD。在许多情况下，每当事务系统中发生更改时，我们都会向数据仓库中插入一行新数据。插入新行会导致数据仓库中出现数据重复，这意味着我们不能使用源表的 primary key 作为维度的 primary key。因此，我们需要引入一组新的列，如下所示：

- 一个新的键列，保证 **Customers** 维度中行的唯一性。这个新键列只是一个表示数据仓库维度中存储的每一行数据的索引。新键是所谓的 **surrogate key**。虽然 surrogate key 保证了该维度中行的唯一性，但我们仍需要维护源系统的 primary key。在数据仓库中，源系统的 primary key 被称为 **business keys** 或 **alternate keys**。换句话说，**surrogate key** 是作为其 primary key 添加到维度的新列。此新列在数据源中不存在，因此与源系统中的 primary key 不同。
- 一个 **Start Date** 和一个 **End Date** 列，表示一行数据处于当前状态的时间范围。
- 另一个显示每行数据状态的列。

SCD 2 是最常见的 SCD 类型。

让我们重新回顾本章 *Dealing with SCDs* 部分开头我们查看过的原始场景，其中 **Brian Welcker** 提交了

