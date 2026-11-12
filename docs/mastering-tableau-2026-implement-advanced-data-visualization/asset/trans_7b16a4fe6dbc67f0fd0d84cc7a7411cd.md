

两个数据集之间关系（基于逻辑层）的连线由于其看起来像 *有嚼劲* 的意大利面而被称为 **面条** —— 真让人垂涎！Tableau 会在你将第二个数据源拖放到数据源画布时立即检测出关系，但如果需要，你可以点击面条本身来添加更多键列或移除和调整它们：

![](img/66cf6b89349fda3581a4f196cb3c98a4_142_0.png)

图 4.3：关系键

如果在 Tableau 2020.2 或更高版本中打开较旧的仪表板，你会看到合并的数据将显示为 **已迁移**。这是有意为之。只需点击已迁移的数据源，Tableau 就会从逻辑层切换到物理层，这意味着你将看到基于连接的数据源画布，而不是关系画布。

查看工作表时，你也会注意到差异。在新的关系布局中，列首先按表名划分，然后按数据源拆分为维度和度量，而在连接中，列被划分为维度和度量，并按表名拆分。

关于关系，总结来说，新的数据源布局使合并数据集变得容易得多，而且你不需要事先决定是要连接还是混合。习惯了旧版 **数据源** 窗格的人可能需要一点时间来适应新的灵活性，但对于新的 Tableau 用户来说，从一开始就使用不同的数据集会容易得多。尽管如此，我们仍将在下一部分介绍连接，尤其是因为该功能仍然是 Tableau 物理层的一部分。

## 使用连接

本书假设你具备连接的基础知识，具体包括内连接、左外连接、右外连接和全外连接。如果你不熟悉连接的基础知识，可以考虑学习 [W3Schools 的 SQL 教程](https://www.w3schools.com/sql/default.asp)。基础知识并不困难，所以你很快就能上手。

以下截图显示了三个相关的数据集：

![](img/66cf6b89349fda3581a4f196cb3c98a4_143_0.png)

图 4.4：相关数据集

为了更好地理解 Tableau 在连接多个表时对数据所做的操作，让我们来探索连接查询。

## 连接查询

以下截图是图 4.4 中已连接数据集的表示：

![](img/66cf6b89349fda3581a4f196cb3c98a4_144_0.png)

图 4.5：连接裁剪

上一张截图表示 **Orders** 表和 **Returns** 表之间的内连接，它们通过公共键 **Order ID** 进行内连接。**Orders** 和 **People** 在公共键 **Region** 上进行连接。

为了更好地理解刚才描述的内容，我们将继续进行连接裁剪练习。我们将查看 Tableau 在使用这两个表构建简单视图时生成的 SQL 查询，这将向我们展示 Tableau 在我们简单拖放时在幕后所做的操作。

请按照以下步骤操作：

- 1. 从 https://github.com/PacktPublishing/Mastering-Tableau-2026/tree/main/Chapter04 定位并下载本章关联的工作簿。
- 2. 选择 **连接裁剪** 工作表并单击 **Sample - EU Superstore Join** 数据源。
- 3. 在菜单中，选择 Help，然后是 Settings and Performance，接着是 Start Performance Recording：

![](img/66cf6b89349fda3581a4f196cb3c98a4_145_0.png)

图 4.6：开始性能录制

- 4. 将 People 拖放到行架，并将 Sales 放置到文本架：

![](img/66cf6b89349fda3581a4f196cb3c98a4_146_0.png)

图 4.7：拖放

- 5. 通过 Help，然后是 Settings and Performance，最后是 Stop Performance Recording 来停止录制。
- 6. 在生成的 Performance Summary 仪表板中，将时间滑块拖动到 0.0000 并选择前两个绿色条：Executing Query。现在，你会看到 Tableau 生成的两个 SQL 查询：

![](img/66cf6b89349fda3581a4f196cb3c98a4_147_0.png)

图 4.8：性能录制

到目前为止，我们已将 Tableau 连接到我们的数据集，然后创建了一个非常简单的工作表，显示了每个人的销售数量。但是，为了让 Tableau 显示人员姓名和销售金额，它需要从两个不同的表中获取数据：**Orders** 和 **People**。*图 4.8* 中的性能录制显示了构建仪表板每个步骤所需的时间以及向数据库发送了哪个查询。由于我们先将一个维度拖放到工作表，然后再拖放一个度量，因此我们会看到两个查询。第二个查询组合了这两个拖放的字段；因此，我们将重点介绍后者。让我们详细看一下：

```
"SELECT "People"."Region" AS "Region",
SUM("Orders"."Sales") AS "sum:Sales:ok"
FROM "TableauTemp"."Orders$" "Orders"
INNER JOIN "TableauTemp"."People$" "People" ON ("Orders"."Region" = "People"."Region")
INNER JOIN "TableauTemp"."Returns$" "Returns" ON ("Orders"."Order ID" = "Returns"."Order ID")
GROUP BY 1"
```

请注意，表之间生成了一个内连接。这是 Tableau **连接功能** 幕后的一个视图。

在讨论完技术细节后，让我们来看看仪表板本身中的连接。

## 连接计算

在 Tableau 中，可以基于计算连接两个文件。你会使用此功能来解决两个数据源之间的不匹配。计算连接可以在每个连接的下拉菜单中访问。

请参阅以下截图：

![](img/66cf6b89349fda3581a4f196cb3c98a4_148_0.png)

图 4.9：创建连接计算...

作为示例，假设一个数据集包含一个名为 **First Name** 的列和另一个名为 **Last Name** 的列。你希望将其连接到第二个数据集，该数据集有一个名为 **Name** 的列，包含姓和名。连接这两个数据集的一种方法是在第一个数据集中创建一个类似于以下内容的 **连接计算**：

```
[First Name] + ' ' + [Last Name]
```

现在，在第二个数据集中选择 **Name** 列，你的键应该就匹配了！

如果你想了解更多关于计算连接的信息，请查看 [Tableau 帮助页面](https://help.tableau.com/current/pro/desktop/en-us/joining_tables.htm#use-calculations-to-resolve-mismatches-between-fields-in-a-join)。

## 空间连接

Tableau 还支持 **空间连接**。这意味着你可以连接 MapInfo 表以及 Shape、Esri、KML、TopoJSON 和 GeoJSON 文件中的空间字段。你也可以使用 CSV 或 Excel 文件，并通过 Tableau MAKEPOINT 函数将数据转换为空间数据（参见 _第 11 章，利用高级分析_）。在实践中，想象两个数据集：一个关于流域位置，由空间字段指示，另一个包含水禽目击位置的数据集，也在空间列中。Tableau 允许你连接这两个数据集，否则这很难实现，因为大多数程序不支持空间数据。

要在空间列上进行连接，你需要从 **Join** 下拉菜单中选择 **Intersects** 字段，该选项仅在处理空间数据时可见。

有关在 Tableau 中连接空间文件的更多信息，请阅读 Tableau 的以下资源：

- https://help.tableau.com/current/pro/desktop/en-us/maps_spatial_join.htm
- https://www.tableau.com/about/blog/2018/8/perform-advanced-spatial-analysis-spatial-join-now-available-tableau-92166
- https://help.tableau.com/current/pro/desktop/en-us/maps_spatial_join.htm
- https://help.tableau.com/current/pro/desktop/en-us/examples_spatial_files.htm

作为横向连接的替代方案，你可能需要垂直连接，也称为 **合并**。合并的典型用例是可视化存储在多个文件或表中的随时间变化的数据。你可能有一份每日报告，其列结构相同，但每天作为新文件收集，存储在每月或每年的文件夹中。

合并这些文件比逐一查看它们能提供更多洞察！你将能够分析趋势以及每日或每年的变化，或执行年初至今分析。因此，我们将在下一部分深入介绍 Tableau 中的合并操作。

