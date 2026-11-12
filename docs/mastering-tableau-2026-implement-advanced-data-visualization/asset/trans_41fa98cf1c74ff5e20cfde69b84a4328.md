

- 10. 按照 R 安装和集成部分所示，将 R 连接到 Tableau。完成后，返回 Tableau 并单击 **测试连接** 按钮，查看是否成功：

![](img/66cf6b89349fda3581a4f196cb3c98a4_630_0.png)

图 14.11: 测试 R 连接

- 11. 单击 **确定** 关闭窗口

- 12. 在 Tableau 中创建以下以 R 为中心的计算。注意，R 函数（例如 mean）区分大小写：

```
R - mean: SCRIPT_INT("mean(.arg1)", SUM(Sales))
R - median: SCRIPT_REAL ("median(.arg1)", SUM(Sales))
R - sd: SCRIPT_INT("sd(.arg1)", SUM(Sales))
```

- 13. 将每个 R 计算字段放在 **度量值** 货架上，并排列它们，使同类计算相互相邻，但 Tableau 计算与 R 计算交替排列；例如，**Tab Avg** 然后 **R — 均值** 然后 **Tab 中位数** 然后 **R — 中位数** 等等。由于 SCRIPT_ 函数被归类为表计算（稍后详述），请确保每个 R 计算字段和 Tableau 计算字段的 **按以下方式计算** 均设置为 **表(向下)**：

![](img/66cf6b89349fda3581a4f196cb3c98a4_631_0.png)

图 14.12: Tableau 与 R 输出对比

- 14. 观察 Tableau 和 R 函数显示相同的结果

请注意，中位数计算中已将 INT 替换为 REAL，这表明，正如名称所暗示的那样，SCRIPT_REAL 使用浮点值，而 SCRIPT_INT 使用整数。在中位数的情况下，SCRIPT_INT 函数会导致舍入差异——请自行尝试并亲自看看！

这是我们首次进行 R 集成练习——简单，对吧？本练习的主要目的是向您展示 R 计算有效，因此我们比较了三个相同的计算，每个计算均由 Tableau 和 R 分别计算。换句话说，这是复制 Tableau 功能，以证明 R 集成按预期工作。下一个练习将是使用 Tableau 内置功能无法完成的操作（截至撰写时）。我们将计算包含两个以上变量的回归分析。

## 使用 R 进行回归计算

简而言之，回归分析是一种用于估计变量关系的技术。有多种回归分析类型，其中最常用的是线性回归。如以下截图所示，线性回归估计最适合数据的直线，且是 Tableau 的内置功能。

您只需在 **行** 和 **列** 上放置两个度量值，以及一个用于划分视图中点的维度。然后，进入 **分析**，将 **趋势线** 拖到屏幕上并选择 **线性**：

![](img/66cf6b89349fda3581a4f196cb3c98a4_632_0.png)

图 14.13: 散点图

请注意，此截图来自 Tableau。您可以单击与本章关联的工作簿中的 **散点图** 工作表来访问它。这是一个打开趋势线的简单散点图。在 Tableau 中，趋势线默认为线性，但还包括对数、指数和多项式选项，这些都是回归分析的示例。通过在使用趋势线的可视化中访问 **工作表 | 导出 | 数据**，您可以为视图上的标记生成带有预测值和残差的 **Access** 数据库。但这是一个繁琐的过程，无法为实施更强大的线性回归用途提供健壮的动态解决方案。使用 R 提供了更大的灵活性。

线性回归可使用单个或多个变量。单变量方程适合学习，但多变量方程通常是现实世界应用所必需的。以下练习包含多变量方程。本练习的目标是确定 **Profit** 的线性回归模型与 COUNT(Quantity)、SUM(Sales) 和 AVG(Discount) 的拟合程度如何：

- 1. 导航到回归工作表

- 2. 选择 Superstore 数据源

- 3. 通过将 **Profit** 放在 **列** 货架上，将 **State** 放在 **行** 货架上，并通过将 **State** 放在 **筛选器** 货架上按 **Sum of Profit** 筛选 **Top 10**，来构建基本布局：

![](img/66cf6b89349fda3581a4f196cb3c98a4_633_0.png)

图 14.14: 筛选前 10

- 4. 创建名为 Profit_Expected 的计算字段，使用以下代码：

```
SCRIPT_REAL("
x <- lm(.arg1 ~ .arg2 + .arg3 + .arg4)
x$fitted",
SUM(Profit), COUNT(Quantity), SUM(Sales), AVG(Discount)
)
```

- 5. 创建名为 % Diff 的计算字段，用于计算 SUM(Profit) 与 Profit_Expected 之间的百分比差异：

```
SUM(Profit)/Profit_Expected - 1
```

- 6. 创建名为 Profit_Expected (残差) 的计算字段，以美元为单位返回 SUM(Profit) 与 Profit_Expected 之间的差异：

```
SCRIPT_REAL("
x <- lm(.arg1 ~ .arg2 + .arg3 + .arg4)
x$residuals",
SUM(Profit),COUNT(Quantity), SUM(Sales), AVG(Discount))
```

- 7. 将 Profit_Expected 放在列货架上，与 Profit 相邻。然后，单击其中任何一个并启用双轴。

- 8. 右键单击一个轴并选择同步轴

- 9. 在标记卡的 SUM(Profit) 部分中，将 % Diff 移动到颜色货架上

- 10. 选择 Red-Black 发散调色板以及分级颜色中的 2 个步骤。这样，所有正值将显示为黑色，所有负值显示为红色。请确保视图类型选择为条形图。

![](img/66cf6b89349fda3581a4f196cb3c98a4_634_0.png)

图 14.15: 颜色编码

- 11. 从菜单中选择 分析 | 堆叠标记 | 关闭

- 12. 在标记卡的 Profit_Expected 部分中，将 Profit_Expected (残差)、% Diff 和 Profit_Expected 移动到标签货架上。请确保视图类型选择为甘特条。

- 13. 再次检查所有表计算（由三角形符号指示：Profit_Expected (残差)、% Diff 和 Profit_Expected）是否都使用 State 进行计算：

![](img/66cf6b89349fda3581a4f196cb3c98a4_635_0.png)

图 14.16: 按以下方式计算

- 14. 单击标签和文本部分中的三个点，并按喜好编辑标签：

![](img/66cf6b89349fda3581a4f196cb3c98a4_636_0.png)

图 14.17: 预期利润

根据需要调整条形图和甘特条的大小，并观察结果：

![](img/66cf6b89349fda3581a4f196cb3c98a4_636_1.png)

图 14.18: 回归

图 14.18 中的可视化显示了实际利润、预期利润及两者之间差异的比较。此计算在零售市场中尤其有用，也适用于任何公司的财务规划。

现在我们已经完成了练习，请花点时间考虑本练习中使用的一些代码：

| SCRIPT_REAL | 此 Tableau 函数调用 R 引擎并返回浮点值。 |
| :--- | :--- |
| "x <- lm(.arg1 ~ .arg2 + .arg3+.arg4); x$residuals" | 这是一个 R 表达式，包含一个变量、一个函数和一个参数，然后返回预测值。 |
| x <- | 这是要由后续 R 函数填充的变量。 |
| lm(.arg1 ~ .arg2 + .arg3 + .arg4) | 此 R 函数用于拟合线性模型。它可以用于根据参数中提供的变量返回回归。括号中的信息称为参数，用于拟合模型。具体来说，响应位于波浪号 (~) 的左侧，模型位于右侧。因此，这是一个多变量线性回归，其中 .arg1 = SUM(Profit)，.arg2 = COUNT(Quantity)，.arg3 = SUM(Sales)，.arg4 = AVG(Discount)。从英语角度来看，该参数可读作 SUM(Profit)，并被建模为 COUNT(Quantity)、SUM(Sales) 和 AVG(Discount) 的组合项。 |
| ", SUM(Profit), COUNT(Quantity), SUM(Sales) , AVG(Discount)) | 这些是用于填充 .arg# 变量的参数。请注意，双引号 (") 指定传递给 R 的代码结束，逗号 (,) 指定 Tableau 函数的下半部分，即表达式。 |

表 14.1: 代码快速回顾

在成功实现回归后，我们将现在研究一种常用于区分数据集中子集的不同统计方法，称为聚类。

