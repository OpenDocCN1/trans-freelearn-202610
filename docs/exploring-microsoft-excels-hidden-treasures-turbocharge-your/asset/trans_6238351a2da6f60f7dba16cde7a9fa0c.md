

## 更新源数据连接

当你移动或重命名数据源（例如你已连接的数据源）时，Power Query 连接将保持附加到该数据源——在此示例中，即 Chapter 12 - Workbook Index.xlsx。让我们为工作簿指定一个新名称，然后更新 Power Query 连接：

+   1. 选择 File | Save As，指定一个新名称，例如 Power Query Index.xlsx，然后单击 Save。
+   2. 激活 Summary 工作表并按 Shift + F11 创建新工作表。

> Tip
> 你也可以通过选择 Home | Insert | Worksheet 来插入新工作表。

+   3. 选择 File | Save。
+   4. 右键单击单元格 **A1** 并选择 **Refresh**。请注意，即使你刷新了列表，新工作表也不会出现。这是因为 **Power Query** 仍在引用 *Chapter 12 – Workbook Index.xlsx* 工作簿。
+   5. 在 **Index** 工作表的 **Table** 中选择任意单元格，然后选择 **Query | Edit**。
+   6. 在 **Query Settings** 任务窗格的 **Applied Steps** 列表中，单击 **Source** 步骤旁边的 **Gear Icon**。
+   7. 单击 **Browse** 按钮以选择文件的更新版本，然后单击 **OK**，如 *Figure 12.8* 所示：

![](img/0c7a357df7e57e06b6d9f631c219040c_395_0.png)

Figure 12.7 – Excel Workbook connection dialog box

+   8. 选择 **Home | Close & Load** 将数据返回到工作表。
+   9. 你添加的新工作表现在将出现在列表中。

这个创建工作簿工作表索引的简单示例涵盖了你在 **Power Query** 中连接和刷新数据源时会遇到的几个常见问题。现在，让我们学习如何使用 **Power Query** 来自动化会计报告的清理工作。

## 自动化报告清理

你可能会发现自己手头有一些需要在 Excel 中分析的报告，但这些报告从分析角度来看非常不友好。你甚至可能每月都在手动清理此类报告。或者，你可能正在编写 Excel 宏来自动化此类重复性任务。我要告诉你，自从有了 **Power Query**，我现在编写的宏少得多了。

正如我们在工作簿索引示例中所见，**Power Query** 提供了一种无需编码的方法，旨在实现“设置并遗忘”。我们将把一份 1 月份会计报告转换为可分析的格式，然后用 2 月份的报告覆盖它。这将说明一旦你建立了 **Power Query**，唯一的重复步骤就是运行新报告并覆盖数据源的先前版本。

首先，让我们看看从其他软件程序导出到 Excel 的报告中出现的一些常见问题。

## 分析障碍

本章 GitHub 仓库中提供的 _Chapter 12 - January Aging.xlsx_ 工作簿说明了 Excel 用户可能遇到的常见问题。**应收账款账龄报告**是一份会计报告，显示客户应付款项以及发票未付的时间长度。如 **Figure 12.9** 所示，该报告从分析角度存在几个问题。

![](img/0c7a357df7e57e06b6d9f631c219040c_396_0.png)

Figure 12.8 – Typical issues that arise in reports exported from accounting software and other tools

一些问题包括：

+   - 列 B:C、D:E、G:H、J:K、M:N、P:Q 和 R:S 中的合并单元格
+   - 每个租户后面出现一个总计行，这使得筛选更加困难，并且在求和时不小心重复部分或全部金额的风险也更大。
+   - 租户和账户 **名称** 仅出现在第一行，但交易出现在其他行
+   - 虽然 _Figure 12.9_ 中未显示，但与特定租户无关的其他报告总计出现在报告下方

任何 _一项_ 这些问题都足以令人沮丧，更不用说同时存在多个问题了。我们将使用 **Power Query** 将 _Figure 12.9_ 中所示的丑陋报告转换为 _Figure 12.10_ 中所示的可分析格式：

![](img/0c7a357df7e57e06b6d9f631c219040c_397_0.png)

Figure 12.9 – Data transformed by Power Query

让我们看看 Power Query 如何消除解锁感觉被困在电脑玻璃下的数据所涉及的挫败感和障碍。

## 转换报告

像通过下一个示例进行操作这样的实践经验，是克服 Power Query 带来的学习曲线的好方法：

+   1. 打开 Chapter 12 - January Aging.xlsx 工作簿。
+   2. 选择 File | Save As 并输入名称 Aging.xlsx。请注意，这里没有月份名称，以避免未来月份混淆。稍后在此序列中，我们将用 Chapter 12 - February Aging.xlsx 工作簿覆盖此 Aging.xlsx 工作簿。
+   3. 选择 File | Close 关闭 Aging.xlsx 工作簿。
+   4. 按 Ctrl + N (% + N) 创建空白工作簿（或选择 File | New 并选择 Blank Workbook）。
+   5. 选择 Data | Get Data | From File | From Excel Workbook，选择 Chapter 12 - January Aging.xlsx 工作簿，然后单击 Import。
+   6. 从 Navigator 窗口左侧列表中选择 Sheet1。请注意数据预览如何出现。单击 Transform Data 打开 Power Query 编辑器。
+   7. 在 Query Settings 任务窗格的 Applied Steps 列表中，右键单击 Promoted Headers，选择 Delete Until End，然后单击 Delete。请小心操作，以免不小心删除 Source 或 Navigation 步骤，因为这样做可能会从 Data Preview 窗口中删除数据，或者更糟的是，完全中断与数据的连接。无论哪种情况，如果你是 Power Query 新手，阻力最小的路径是选择 File | Discard & Close，然后从步骤 5 重新开始。

## Power Query

> > **Tip**
> > **Power Query** 有时会假设你希望将工作表的第一行提升为列标题，并自动更改某些列的 **Data Type** 属性。这样做通常会将基于文本的数字转换为整数或小数值。我倾向于删除 **Power Query** 添加到 Applied Steps 列表中的任何“有帮助”的步骤，以便我从 Source 和 Navigation 步骤开始，然后添加我需要的任何其他步骤来转换报告。有时，当这些“有帮助”的步骤与你稍后添加的步骤发生冲突时，它们会变得无帮助。

+   8. 在 **Power Query** 中删除列就像单击列并按 _Delete_ (_Fn + Delete_) 一样简单。尝试此技术，删除 Column1，它不包含最终报告中需要的任何数据。

> > **Tip**
> > **Power Query** 没有撤销命令，但你可以通过从 Applied Steps 列表中删除步骤来回滚转换。如果你连续执行某个步骤，例如删除空列，Applied Steps 列表中只会有一个跨越你删除的所有列的 Removed Columns 步骤。如果你删除此步骤，所有已删除的列都将返回，但你可以再次删除它们。

+   9. 在 **Applied Steps** 列表中右键单击 **Removed Columns**，选择 **Rename**，将标题更改为 `Removed First Column`，然后按 _Enter_。

> > **Tip**
> > 随着 **Applied Steps** 列表的增长，很容易在数据转换过程中失去对正在发生的事情的跟踪。在操作过程中重命名步骤可以记录转换过程，并使排查产生意外结果的转换变得容易得多。

+   10. 报告的前三行是多余的，但你无法删除它们。相反，请选择 **Home** | **Remove Rows** | **Remove Top Rows**。
+   11. 在 _Figure 12.11_ 所示的 **Remove Top Rows** 对话框的 **Number of rows** 字段中输入 3，然后单击 _OK_：

![](img/0c7a357df7e57e06b6d9f631c219040c_398_0.png)

Figure 12.10 – The Remove Top Rows dialog box

