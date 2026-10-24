

## 3. 单击 **Solver Add-In** 复选框，如 **图 6.23** 所示：

![](img/0c7a357df7e57e06b6d9f631c219040c_206_0.png)

图 6.23 – 启用 Solver 加载项复选框

- 4. 单击 **OK**。
- 5. 如有需要，关闭 **Options** 对话框。

此时，**Solver** 命令将出现在 Excel 功能区的 **Data** 选项卡上。单击此按钮将显示 **Solver** 对话框，如 **图 6.24** 所示：

![](img/0c7a357df7e57e06b6d9f631c219040c_207_0.png)

图 6.24 – Solver 参数对话框

假设您想以与 **Goal Seek** 相同的方式求解付款金额。如果您将利率改回 5.25% 或任何其他静态金额，可以使用与 **Goal Seek** 相同的示例。以下是使用 **Solver** 确定基于特定利率、期限或付款可借款金额的步骤：

- 1. 激活本章示例工作簿中的 **Solver** 工作表。
- 2. 选择 **Data | Solver**。
- 3. 在 **Solver Parameters** 对话框的 **Set Objective** 字段中输入 C4，如 **图 6.21** 所示。
- 4. 选择 **Value Of**。
- 5. 在 **Value Of** 旁边的字段中输入付款金额，例如 350。
- 6. 将 **By Changing Variable Cells** 字段设置为 C5。
- 7. 单击 **Solve** 按钮。

8. Excel 将显示 **Solver Results** 对话框，如图 6.25 所示。可选地，如果您想更改此付款金额或返回可能包含在具有更多变量的场景中的任何其他变量，可以单击 **Return to Solver Parameters Dialog** 复选框。

9. 单击 **OK** 以根据上一步中的选择返回工作表或 **Solver Parameters** 对话框：

![](img/0c7a357df7e57e06b6d9f631c219040c_208_0.png)

图 6.25 – Solver 结果对话框

**细微差别**

使用 **Solver** 而非 **Goal Seek** 的一个好处是，**Solver** 功能会记住您所做的选择，而 **Goal Seek** 对话框每次都会重置。如果您在启动 **Goal Seek** 命令之前单击公式单元格，可以预填 **Goal Seek** 对话框中的 **Set Cell** 字段。简而言之，如果您要执行重复的目标求解操作，**Solver** 更高效。

如图 6.25 所示，您还可以将 **Solver** 结果存储在 Excel 的 **Scenario Manager** 中，我在前面的 _Understanding the Scenario Manager feature_ 部分中讨论过该功能。

## 摘要

通常，Excel 的 **What-If Analysis** 功能作为自动化 Excel 中重复任务的手段而被忽视。在本章中，您了解了 `PMT`、`CUMIPMT` 和 `CUMPRINC` 函数，这些函数可用于计算贷款中一个或多个期间的贷款付款和总利息。我们使用这些函数作为基础，演示了 Excel 中的 **What-If Analysis** 功能。例如，**Scenario Manager** 功能使您能够将不同的输入集交换到工作表中，并通过场景报告比较结果。或者，**Goal Seek** 允许您求解单个缺失的输入。这消除了在尝试确定缺失金额时可能涉及的手动猜测。

然后，我们通过 Data | What-If Analysis 菜单来到了 **Data Table** 功能。**Data Tables** 可以基于交换一个或多个输入来一次求解多个方程式。这使您的公式具有更好的完整性，因为您不必尝试在公式中链接输入；您只需用单个公式锚定 **Data Table**，Excel 就会交换输入。

如果您想将一系列数字投射到未来，可以创建 **Forecast Sheet** 来自动化您的投射。此类表格无法在 **Excel for macOS** 中创建，但可以在该版本中访问。本章以介绍隐藏的 **Solver** 功能结束，该功能可用于求解两个或多个缺失输入。我没有空间深入探讨 **Solver**，但您看到了 **Solver** 如何简化您通常在 **Goal Seek** 中执行的任务，因为 **Solver** 会记住最近的设置，而 **Goal Seek** 对话框每次访问时都会重置。

在第 7 章 *Automating Tasks with the Table Feature* 中，您将了解通过仅将底层数据转换为 Table 来自动化 Excel 功能的众多方法。此外，我将阐明同样命名的 **Data Table**、**Table** 和 **PivotTable** 功能之间的区别。

## 使用 Table 功能自动化任务

我认为 **Table** 功能是 Excel 中的超能力，只需一个键盘快捷键或几次鼠标单击即可解锁巨大的自动化功能。例如，引用 **Table** 中数据（而非正常单元格范围）的公式和功能会自动“看到”您添加到 **Table** 的任何新数据。这消除了重写公式以扩展单元格范围并返回到 **PivotTables** 等功能以扩展源数据的需要。**Slicers** 在筛选时消除了导航下拉菜单的需要，而启用 **Total Row** 选项仅统计可见行而不是整个列表。**Tables** 允许 **Charts** 和 **Sparklines** 自动更新——列表还在继续。在本章中，我将涵盖以下主要主题：

- Excel 的不成文规则
- 什么是 Table？
- **Table** 增强功能
- **Table** 自动化机会
- 导航和选择细微差别
- **Table** 故障排除

简而言之，本章旨在向您展示如何一次性消除重复任务并提高数据完整性。本章还补充了 _Chapter 12, Power Query_，因为您通过 Power Query 从其他来源提取的数据通常会返回到 **Table**。

## 技术要求

本章中的所有键盘快捷键均适用于 **Excel for Windows**，但只有部分快捷键适用于 **Excel for macOS**。除非另有说明，本章中的所有功能在 **Excel for Windows** 和 **macOS** 中的工作方式相同。包含本章中使用的所有示例的工作簿可从 GitHub 下载，网址为 `https://github.com/PacktPublishing/Exploring-Microsoft-Excels-Hidden-Treasures/tree/main/Chapter07`。

## Excel 的不成文规则

多年来，我观察到一条不成文规则，即许多用户无意中违反了该规则：_任何_ 列表的第一行都应该是一行唯一的标题——即 Excel 术语中的 **Headers**。这适用于您在 Excel 中创建的每个列表，无论您是否使用 **Table** 功能。**Headers** 非常重要，以至于 **Table** 功能在需要时会添加占位符 **Headers**。**Tables** 和 **PivotTables** 都要求每个 **Header** 唯一，并会在重复 **Headers** 后追加数字以强制执行合规性。

> **Tip**
> 以上述规则的补充是，列表不应有任何空行或空列。大多数 Excel 功能作用于 **Current Region**，即活动单元格周围的非空单元格连续块。空行和空列会截断 **Current Region**，并要求您在排序或筛选前手动选择整个列表，而不是能够选择列表中的单元格然后排序或筛选。

创建新电子表格时，请尽量将 **Headers** 保持为单行，然后在需要多行显示时使用 **Home** | **Wrap Text**。您也可以在单元格中想要换行的每个位置按 `Alt + Enter` (`Option + Enter`)。如果您想保留多行 **Headers**，请考虑在最后一个 **Header Row** 上方插入一个空行，然后隐藏该行。

> **Tip**
> 此外，您可以在 **Excel 2019** 及更高版本中使用 `TEXTJOIN` 函数将多个单元格的文本合并为一个。将结果作为值复制并粘贴后，您可以删除原始行。

