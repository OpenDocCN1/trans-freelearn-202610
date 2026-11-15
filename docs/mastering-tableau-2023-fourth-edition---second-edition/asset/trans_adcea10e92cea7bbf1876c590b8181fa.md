

# Hyper 要点总结

总而言之，`Hyper` 是一个高度专业化的数据库引擎，让我们作为用户能从查询中获得最佳性能。如果你还记得，在 `第 1 章` `回顾基础知识` 中，我们已经看到工作表或仪表板上的每一次更改，包括拖放药丸、筛选器和计算字段等，都会被转化为查询。这些查询非常像 `SQL`；然而，在 Tableau 中，我们称查询引擎为 `VizQL`。

`VizQL` 是 Tableau Desktop 中的另一颗隐藏宝石，负责以图表格式可视化数据，并完全在内存中执行。优势在于数据库端不需要额外的空间。当用户将字段放置在货架上时，会生成 `VizQL`。然后 `VizQL` 被翻译成 `SQL`、`MDX` 或 `Tableau 查询语言` (`TQL`)，并通过驱动程序传递给后端数据源。

## 专注于数据准备

Tableau 可以有效地用于各个数据准备阶段。遗憾的是，单个章节不足以深入探讨 Tableau 如何在每个阶段中使用。确实，这种深入探讨可能值得写成一整本书！因此，我们的重点将放在数据准备上，因为从历史上看，该阶段占据了高达 60% 的数据挖掘工作量。我们的目标是学习如何使用 Tableau 来简化这项工作。

### 勘察数据

Tableau 可以成为简单勘察数据的非常有效的工具。有时在勘察过程中，你可能会发现清理数据或根据现有字段填补不完整数据的方法。有时，遗憾的是，拼图碎片根本不足以拼凑出完整的数据集。在这种情况下，Tableau 可用于准确传达缺口所在，这反过来可能会激励组织更完整地填充底层数据。

在本练习中，我们将探索如何使用 Tableau 快速发现数据集中每个字段的空值百分比。接下来，我们将探索如何从现有字段推断数据以填补空白。

#### 确定空值

以下是勘察数据的步骤：

1.  如果你尚未这样做，请导航至 `https://public.tableau.com/profile/marleen.meier` 以查找并下载与本章关联的工作簿。
2.  导航至名为 `勘察` `&` `探索` `数据` 的工作表，并选择 `幸福报告` 数据源。
3.  将 `区域` 和 `国家` 拖至 `行` 货架。观察在某些情况下，`区域` 字段对于某些国家显示 `Null` 值：
    ![](Images/B18435_02_01.png)
    图 2.1：空值区域
4.  右键单击并 `编辑` 名为 `选择字段` 的参数。注意 `数据类型` 设置为 `整数`，我们可以观察到一个包含数据集中每个字段名称条目的列表：
    ![](Images/B18435_02_02.png)
    图 2.2：编辑参数
5.  在 `数据` 窗格中，右键单击刚创建的参数并选择 `显示参数控件`。
6.  创建一个名为 `% 已填充` 的计算字段并编写以下计算：
    ```
    SUM([Number of Records]) / TOTAL(SUM([Number of Records]))
    ```
7.  在 `数据` 窗格中，右键单击 `% 已填充` 并选择 `默认属性` | `数字格式…`：
    ![](Images/B18435_02_03.png)
    图 2.3：调整默认属性
8.  在生成的对话框中，选择 `百分比`。
9.  创建一个名为 `空值与已填充` 的计算字段并添加以下代码。注意完整的 CASE 语句相当冗长但也重复。
    在需要冗长但重复计算的情况下，考虑使用 Excel 更快、更准确地编写代码。通过使用 Excel 的 `CONCATENATE` 函数，你可能能够节省时间并避免拼写错误。
    在下面的代码块中，代码行仅占总量的一部分，但应足以让你生成完整的代码块：
    ```
    CASE [Select Field]
    WHEN 1 THEN IF ISNULL ([Country]) THEN 'Null Values' ELSE 'Populated Values' END
    WHEN 2 THEN IF ISNULL ([Region]) THEN 'Null Values' ELSE 'Populated Values' END
    WHEN 3 THEN IF ISNULL ([Economy (GDP per Capita)]) THEN 'Null Values' ELSE 'Populated Values' END
    WHEN 4 THEN IF ISNULL ([Family]) THEN 'Null Values' ELSE 'Populated Values' END
    WHEN 5 THEN IF ISNULL ([Freedom]) THEN 'Null Values' ELSE 'Populated Values' END
    WHEN 6 THEN IF ISNULL ([Happiness Rank]) THEN 'Null Values' ELSE 'Populated Values' END
    WHEN 7 THEN IF ISNULL ([Happiness Score]) THEN 'Null Values' ELSE 'Populated Values' END
    WHEN 8 THEN IF ISNULL ([Health (Life Expectancy)]) THEN 'Null Values' ELSE 'Populated Values' END
    WHEN 9 THEN IF ISNULL ([Standard Error]) THEN 'Null Values' ELSE 'Populated Values' END
    END
    ```
10. 从 `行` 货架中移除 `区域` 和 `国家`。
11. 将 `空值与已填充` 放置在 `行` 和 `颜色` 货架上，将 `% 已填充` 放置在 `列` 和 `标签` 货架上：
    ![](Images/B18435_02_04.png)
    图 2.4：已填充值
12. 如果需要，将 `空值` 的颜色改为红色，`已填充值` 的颜色改为绿色。你可以通过单击 `标记` 卡中的 `颜色` 并选择 `编辑颜色` 来实现。
13. 单击工作表上 `选择字段` 参数右上角的箭头，并选择 `单值列表`。
14. 在 `选择字段` 参数中选择不同的选项，并注意某些字段具有较高的空值百分比。例如，在下图中，`32.98%` 的记录没有 `区域` 的值：
    ![](Images/B18435_02_05.png)
    图 2.5：比较空值和已填充值

基于此练习，让我们探索如何使用相同的数据集清理并从现有数据推断数据。

#### 推断数据

本练习将在上一个练习的基础上进行扩展，通过清理现有数据并利用已知信息填补部分缺失数据。我们将假设我们知道哪个国家属于哪个区域。我们将利用该知识来修复 `区域` 字段中的错误，并使用 Tableau 填补空白：

1.  从上一个练习结束的地方开始，创建一个名为 `区域推断` 的计算字段，包含以下代码块：
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
    //用数据集中剩余的字段完成 case 语句
    END
    ```
    作为 `CASE` 语句的替代方案，你可以使用 `IF` 语句，如下所示：
    ```
    If [Country] = 'Afghanistan' then 'Southern Asia' 
    ELSEIF [Country] = 'Albania' then …
    END
    ```
    为了加快创建冗长计算字段的繁琐过程，你可以将数据下载到 Excel 文件中，通过连接各个部分来创建计算字段，如下所示：
    ![](Images/B18435_02_06.png)
    图 2.6：在 Excel 中编译计算

