
查看工作表，你也会注意到一些差异。你会发现，在新的关系布局中，列首先按表名划分，然后按数据源拆分为 `Dimensions`（维度）和 `Measures`（度量），而在联接中，列则被划分为 `Dimensions` 和 `Measures`，并按表名拆分。

总而言之，新的数据源布局让合并数据集变得容易得多，你无需预先决定是要联接还是混合。习惯了旧版数据源窗格的用户可能需要一点时间来适应这种新的灵活性，但对于 Tableau 新用户来说，从一开始处理不同的数据集将容易得多。尽管如此，我们接下来仍将介绍联接，特别是因为该功能仍作为物理层的一部分保留在 Tableau 中。

# 联接

本书假设读者具备联接的基础知识，特别是内联接、左外联接、右外联接和全外联接。如果你不熟悉联接的基础知识，建议学习 W3Schools 的 SQL 教程：[https://www.w3schools.com/sql/default.asp](https://www.w3schools.com/sql/default.asp)。基础知识并不难，所以你不会花太长时间就能上手。

下面的屏幕截图显示了三个相关的数据集：

![](Images/B18435_04_04.png)

图 4.4：相关数据集

## 联接查询

下面的屏幕截图展示了图 4.4 中已联接数据集的表示：

![](Images/B18435_04_05.png)

图 4.5：联接剪裁

上述屏幕截图表示 `Orders` 表和 `Returns` 表之间的内联接，通过公共键 `Order ID` 进行内联接。`Orders` 和 `People` 通过公共键 `Region` 进行联接。

为了更好地理解刚才的描述，我们将继续进行一个联接剪裁练习。我们将查看在使用这两个表构建简单视图时 Tableau 生成的 SQL 查询，这将向我们展示当我们仅仅进行拖放操作时，Tableau 在后台做了什么。

请遵循以下步骤：

1.  从 [https://public.tableau.com/profile/marleen.meier/](https://public.tableau.com/profile/marleen.meier/) 找到并下载与本章关联的工作簿。
2.  选择 `Join Culling` 工作表，然后点击 `Sample - EU Superstore Join` 数据源。
3.  从菜单中选择 `Help`（帮助），然后选择 `Settings and Performance`（设置和性能），最后选择 `Start Performance Recording`（开始性能记录）：

![](Images/B18435_04_06.png)

图 4.6：开始性能记录

4.  将 `People` 拖到 `Rows`（行）架上，将 `Sales` 放在 `Text`（文本）架上：

![](Images/B18435_04_07.png)

图 4.7：拖放操作

5.  通过 `Help`、`Settings and Performance`，最后选择 `Stop Performance Recording` 停止记录。
6.  在生成的 `Performance Summary`（性能摘要）仪表板中，将时间滑块拖到 `0.0000`，并选择两个绿色条形：`Executing Query`（执行查询）。现在你可以看到 Tableau 生成的两条 SQL 查询：

![](Images/B18435_04_08.png)

图 4.8：性能记录

到目前为止，我们已将 Tableau 连接到数据集，然后创建了一个非常简单的工作表，显示人员的销售数量。但是，为了显示人员姓名和销售金额，Tableau 需要从两个不同的表中获取数据：`Orders` 和 `People`。图 4.8 中的性能记录显示了构建仪表板每个步骤所花费的时间以及发送到数据库的查询。因为我们先拖入一个 `dimension`（维度），然后拖入一个 `measure`（度量）到工作表中，所以看到了两条查询。第二条查询结合了两个拖入的字段；因此，我们将重点关注后者。让我们详细看一下：

```sql
"SELECT ""People"".""People"" AS ""People"",
  SUM(""Orders"".""Sales"") AS ""sum:Sales:ok""
FROM ""TableauTemp"".""Orders$"" ""Orders""
  INNER JOIN ""TableauTemp"".""People$"" ""People"" ON (""Orders"".""Region"" = ""People"".""Region"")
  INNER JOIN ""TableauTemp"".""Returns$"" ""Returns"" ON (""Orders"".""Order ID"" = ""Returns"".""Order ID"")
GROUP BY 1"
```

请注意，表之间生成了内联接。这是 Tableau 联接功能的后台视图。

现在技术细节已经讨论过了，让我们看看仪表板本身中的联接。

## 联接计算

在 Tableau 中，可以基于计算联接两个文件。你可以使用此功能来解决两个数据源之间的不匹配问题。计算联接可以在每个联接的下拉菜单中访问。

请看下面的屏幕截图：

![](Images/B18435_04_09.png)

图 4.9：创建联接计算……

举个例子，假设一个数据集包含一列 `First Name`（名）和另一列 `Last Name`（姓）。你想将其联接到第二个数据集，该数据集有一列名为 `Name`，包含名和姓。在第一个数据集中创建如下 `Join Calculation`（联接计算）是联接这两个数据集的一种选择：

```
[First Name] + ' ' + [Last Name]
```

现在，在第二个数据集中选择 `Name` 列，你的键应该就匹配了！

如果你想了解更多关于计算联接的信息，请查看 Tableau 帮助页面：[https://help.tableau.com/current/pro/desktop/en-us/joining_tables.htm#use-calculations-to-resolve-mismatches-between-fields-in-a-join](https://help.tableau.com/current/pro/desktop/en-us/joining_tables.htm#use-calculations-to-resolve-mismatches-between-fields-in-a-join)。

## 空间联接

在 Tableau 2018.2 中，增加了 `spatial joins`（空间联接）。这意味着你可以联接来自 MapInfo 表以及 Shape、Esri、KML、TopoJSON 和 GeoJSON 文件的空间字段。你也可以使用 CSV 或 Excel 文件，并通过 Tableau 的 `MAKEPOINT` 函数将数据转换为空间数据（请参见第 12 章，利用高级分析）。在实践中，想象一下两个数据集，一个关于流域的位置，由空间字段指示；第二个数据集包含水禽目击位置，也在空间列中。Tableau 允许你联接这两个数据集，否则这很难实现，因为大多数程序不支持空间数据。

要在空间列上联接，必须从 `Join`（联接）下拉菜单中选择 `Intersects`（相交）字段，该字段仅在处理空间数据时可见。

有关在 Tableau 中联接空间文件的更多信息，请阅读 Tableau 提供的以下资源：

*   [https://help.tableau.com/current/pro/desktop/en-us/maps_spatial_join.htm](https://help.tableau.com/current/pro/desktop/en-us/maps_spatial_join.htm)
*   [https://www.tableau.com/about/blog/2018/8/perform-advanced-spatial-analysis-spatial-join-now-available-tableau-92166](https://www.tableau.com/about/blog/2018/8/perform-advanced-spatial-analysis-spatial-join-now-available-tableau-92166)
*   [https://help.tableau.com/current/pro/desktop/en-us/maps_spatial_join.htm](https://help.tableau.com/current/pro/desktop/en-us/maps_spatial_join.htm)
*   [https://help.tableau.com/current/pro/desktop/en-us/examples_spatial_files.htm](https://help.tableau.com/current/pro/desktop/en-us/examples_spatial_files.htm)

作为水平联接的替代，你可能需要垂直联接，也称为 `union`（联合）。典型的 `union` 用例是可视化存储在多个文件或表中随时间变化的数据。你可能有一份日报，其列结构相同，但每天作为一个新文件收集，存储在月度或年度文件夹中。

合并这些文件比逐个查看它们能提供更多洞察！你将能够分析趋势和每日或每年的变化，或执行年初至今分析。因此，接下来我们将继续深入解释 Tableau 中的 `unions`。

# 联合
