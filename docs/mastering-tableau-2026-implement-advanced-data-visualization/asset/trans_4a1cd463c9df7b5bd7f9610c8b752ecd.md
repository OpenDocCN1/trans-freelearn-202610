

## 清理您的数据

您是否曾想过，您的数据是否足够可靠以供分析？几乎所有处理数据的人都会面临这个问题。这就是为什么本章侧重于 *数据准备*；将数据准备好以进行有意义分析的基本过程通常称为 *数据清理*。

在本章结束时，您将了解良好的数据质量是什么样子，如何识别数据集中的问题，以及如何使用 Tableau 2026 应用实用的清理技术。总体目标是帮助您在使用真正支持可靠结论的数据时建立信心。

这些信息很重要，因为即使是最先进的分析也依赖于准备充分且准确的数据。学习如何评估和清理您的数据将节省时间、减少错误，并提高您洞察的可信度。

在本章中，我们将讨论以下主题：

- 准备数据集
- 浏览数据
- 清理混乱的数据

现在，让我们深入探讨这个迷人的主题，目标是丰富我们对数据质量的理解。

### 准备数据集

Tableau 可以有效地用于各种数据准备阶段：

1. 数据收集/摄入：我是否可以访问正确的数据？
2. 数据剖析/浏览：我的数据是什么样子的？
3. 数据清理：我的数据是否足够可靠以供分析？
4. 数据转换：我的数据是否为我需要回答的问题构建了结构？
5. 数据集成：我是否正确且一致地合并数据？
6. 数据丰富：我是否有足够的上下文来获得有意义的洞察？
7. 数据验证：我可以信任输出结果吗？
8. 数据加载和发布：数据对利益相关者是否可访问且可用？

不幸的是，单靠一章不足以深入探讨 Tableau 在每个阶段的应用方式。确实，如此深入的探讨可能值得整本书！因此，本章的重点将放在第 2 步和第 3 步——数据剖析和数据清理上——因为这些任务历史上占据了数据准备工作量的高达 60%。我们的目标是学习如何使用 Tableau 来简化该过程。

### 浏览数据

Tableau 是一个用于简单浏览数据的非常有效的工具。有时在浏览过程中，您可能会发现基于现有字段清理数据或填充不完整数据的方法。有时，遗憾的是，根本没有足够的拼图碎片来拼凑出一个完整的数据集。在这种情况下，Tableau 可用于准确传达缺失的内容，而这反过来可能激励组织更充分地填充底层数据。

在本练习中，我们将探讨如何使用 Tableau 快速发现数据集中每个字段的空值百分比。接下来，我们将探讨如何从现有字段推断数据以填补缺失。

### 建立空值

各位一定都遇到过这种情况：您正在查看一个数据集，发现一些值缺失了。或者您可能会看到 null。但现在该怎么办？让我们探索一些选项。

以下是浏览数据的步骤：

1. 如果您尚未这样做，请导航至 https://github.com/PacktPublishing/Mastering-Tableau-2026/tree/main/Chapter02 以查找并下载与此章相关的工作簿。
2. 导航至名为 **Surveying & Exploring Data** 的工作表。
3. 将 **Region** 和 **Country** 拖到 **Rows** 书架。观察在某些情况下，**Region** 字段对某些国家/地区具有 null 值：

![](img/66cf6b89349fda3581a4f196cb3c98a4_80_0.png)

图 2.1：空区域

4. 右键单击并编辑名为 **Select Field** 的参数。请注意，**Data type** 字段设置为 **Integer**，我们可以观察到一个包含数据集中每个字段名称条目的列表：

![](img/66cf6b89349fda3581a4f196cb3c98a4_81_0.png)

图 2.2：编辑参数

5. 在 **Data** 窗格中，右键单击我们刚刚创建的参数，然后选择 **Show Parameter Control**。创建一个名为 **% Populated** 的计算字段，并编写以下计算：

```
SUM([Number of Records]) / TOTAL(SUM([Number of Records]))
```

6. 在 **Data** 窗格中，右键单击 **% Populated**，然后选择 **Default Properties | Number Format**...

![](img/66cf6b89349fda3581a4f196cb3c98a4_82_0.png)

图 2.3：调整默认属性

7. 在弹出的对话框中，选择 **Percentage**。

8. 创建一个名为 **Null & Populated** 的计算字段，并添加以下代码。请注意，完整的 CASE 语句相当冗长，但也具有重复性。在需要冗长但重复计算的情况下，考虑使用 Excel 来更快速且准确地编写代码。通过使用 Excel 的 CONCATENATE 函数，您可能能够节省时间并避免拼写错误。

在以下代码块中，代码行仅代表总体的一部分，但应足以让您生成完整内容：

```
CASE [Select Field]
WHEN 1 THEN IF ISNULL ([Country]) THEN 'Null Values' ELSE
'Populated Values' END
WHEN 2 THEN IF ISNULL ([Region]) THEN 'Null Values' ELSE
'Populated Values' END
WHEN 3 THEN IF ISNULL ([Economy (GDP per Capita)]) THEN 'Null Values' ELSE
'Populated Values' END
WHEN 4 THEN IF ISNULL ([Family]) THEN 'Null Values' ELSE
'Populated Values' END
WHEN 5 THEN IF ISNULL ([Freedom]) THEN 'Null Values' ELSE
'Populated Values' END
WHEN 6 THEN IF ISNULL ([Happiness Rank]) THEN 'Null Values' ELSE
'Populated Values' END
WHEN 7 THEN IF ISNULL ([Happiness Score]) THEN 'Null Values' ELSE
'Populated Values' END
WHEN 8 THEN IF ISNULL ([Health (Life Expectancy)]) THEN 'Null Values' ELSE
'Populated Values' END
WHEN 9 THEN IF ISNULL ([Standard Error]) THEN 'Null Values' ELSE
'Populated Values' END
END
```

9. 从 Rows 书架中删除 Region 和 Country。

10. 将 Null & Populated 放在 Rows 和 Colour 书架上，将 % Populate 放在 Columns 和 Label 书架上：

![](img/66cf6b89349fda3581a4f196cb3c98a4_83_0.png)

图 2.4：已填充的值

11. 如果需要，将 Null Values 的颜色更改为红色，将 Populated Values 的颜色更改为绿色。您可以通过单击 Marks 卡片中的 Colour，然后单击 Edit Colours 来实现此操作。

12. 单击工作表中 Select Field 参数右上角的箭头，然后选择 Single Value List。

13. 在 Select Field 参数中选择各种选项，并注意某些字段的空值百分比很高。例如，在以下图表中，32.98% 的记录没有 Region 的值：

![](img/66cf6b89349fda3581a4f196cb3c98a4_83_1.png)

图 2.5：比较空值和已填充的值

基于此练习，让我们探讨如何使用相同的数据集来清理和推断现有数据。

### 推断数据

本练习将在前一个练习的基础上进行扩展，通过清理现有数据并从已知信息填充一些缺失数据。我们将假设我们知道哪个国家属于哪个区域。我们将利用该知识来修复 **Region** 字段中的错误，并使用 Tableau 填补缺失。

从前一个练习结束的地方开始，创建一个名为 **Region Extrapolated** 的计算字段，并使用以下代码块：

```
CASE [Country]
WHEN 'Afghanistan' THEN 'Southern Asia'
WHEN 'Albania' THEN 'Central and Eastern Europe'
WHEN 'Algeria' THEN 'Middle East and Northern Africa'
WHEN 'Angola' THEN 'Sub-Saharan Africa'
WHEN 'Argentina' THEN 'Latin America and Caribbean'
WHEN 'Armenia' THEN 'Central and Eastern Europe'
WHEN 'Australia' THEN 'Australia and New Zealand'
WHEN 'Austria' THEN 'Western Europe'
//complete the case statement with the remaining fields in the data set
END
```

为了加快创建冗长计算字段的繁琐过程，您可以将数据下载到 Excel 文件，并通过连接各个部分来创建计算字段，如下图所示：

![](img/66cf6b89349fda3581a4f196cb3c98a4_84_0.png)

图 2.6：在 Excel 中编译计算

