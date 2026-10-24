

# 介绍 LET 函数

LET 函数至少需要一个 **Name**，即 **Variable**。这些 **Names** 仅在单个单元格的上下文中有效。这意味着你可以随意重复使用 **Variables**，尽管最好将 **Names** 分配给你反复引用的输入单元格。正如我们所讨论的，你可以使用“新建名称”对话框将公式存储在 **Name** 中。但是，此类公式在驻留在工作表单元格中的公式中使用往往很困难。相反，LET 函数中的 **Variables** 可以包含文本、数字或计算，然后可以通过 **calculation** 参数中的 **Name** 引用。正如你将看到的，这可以消除公式中的重复计算。

LET 函数工作表中单元格 G3 包含 =LET(length,C3,width,C4,height,C5,length*width*height) 公式并返回 1,000。如图 11.13 所示，这就像使用 **Names**，但你不需要提前创建 **Variables** – 你只需在编写公式时将它们添加进去：

![](img/0c7a357df7e57e06b6d9f631c219040c_359_0.png)

图 11.13 – LET 函数

LET 函数具有以下参数：

- Name1：此必需参数 **Names a Variable**，即你将在公式中使用的变量 – 在本例中为 len。你最多可以指定 126 个 **Names**。

> **Nuance**
> Excel 的 LEN 函数计算工作表单元格或文本字符串中的字符数，但在 LET 中，在 name 参数中使用 len（或 LEN）将创建一个覆盖 LEN 工作表函数的 Variable。LET 还会忽略已存在的 **Names**。

- name_value1：此必需参数是要存储在 Name1 **Variable** 中的值或计算 – 在本例中为单元格 C3。name_value 参数可以是单元格引用、数字、用双引号括起来的文本或计算。你最多可以指定 126 个 name_value 参数，每个参数都必须与一个 **Name** 配对。

- calculation_or_Name2：在指定 name1 和 name_value1 后，你可以添加更多 name/name_value 对，也可以输入计算。因此，在此上下文中，我们将创建一个名为 width 的 **Variable**。

- name_value2：每个 **Name** 都必须有一个对应的 name_value，因此在此示例中，我们将引用单元格 C4。

- calculation_or_Name3：我们的第三个 **Variable** 名为 height。

- name_value3：**C5** 被分配为 height **Variable** 的值。

- calculation_or_Name4：现在所有输入都已命名，我们可以指定计算，即 length*width*height。

你可能记得，我们在“名称和公式”工作表的单元格 F4 中使用了 =Length*Width*Height 公式。如果你没有更改原始输入，该单元格将返回 3,456。相反，LET 函数返回 1,000，因为 **Variables** 引用的是 LET 函数工作表上的单元格 **C3:C5**，而不是“名称和公式”工作表上的命名单元格。

稍后，我们将学习如何使用 LET 来消除公式中的重复计算，但首先，我想比较 IFERROR 和 ISERROR 工作表函数。

# 处理公式错误

IFERROR 和 ISERROR 都用于测试计算中的错误。如果计算结果不是错误，IFERROR 将显示计算结果。相反，如果计算结果是错误（例如 #DIV/0!、#N/A 等），IFERROR 将返回你指定的备用值，该值可以是数字、文本或计算。

如果计算返回错误，ISERROR 返回 TRUE；如果不返回错误，则返回 FALSE。如图 11.14 所示，“公式错误”工作表中单元格 **D3** 包含 =(C3–D3)/D3 公式。该公式返回 11%，因为单元格 **C3** 和 **D3** 都包含非零值。在单元格 **D4** 中，=(C4–D4)/D4 公式返回 #DIV0!，因为 Excel 不允许除以零。单元格 **E3** 包含 =D4 公式，由于 # 错误在 Excel 中产生的级联效应，该公式也返回 #DIV/0!。IFERROR 函数是管理此类错误的现代方法：

![](img/0c7a357df7e57e06b6d9f631c219040c_360_0.png)

图 11.14 – 公式错误处理

# IFERROR 函数

如图 *Figure 11.14* 所示，“*Formula Errors*”工作表中单元格 **D7** 包含 =IFERROR((C7-D7)/D7, "n/a") 公式并返回 11%。相反，单元格 **D8** 包含 =IFERROR((C7-D7)/D7, "n/a") 公式并返回 n/a，因为单元格 **D8** 包含零。

IFERROR 函数有两个参数：

- value：这是一个可能在 Excel 中返回任何类型 # 符号错误的计算，例如 #DIV/0!、#NAME?、#CALC! 等。在单元格 **E7** 中，IFERROR 返回 11%，因为 (C7-D7)/D7 计算结果不是错误。

- value_if_error：当 *value* 参数返回错误时返回的备用值或计算。单元格 **D8** 显示 n/a，因为 (C8-D8)/D8 计算结果是错误。

![](img/0c7a357df7e57e06b6d9f631c219040c_361_0.png)

如果你使用 Excel 已经有 15 年或更长时间，你可能还记得 IFERROR 函数不存在的时代。那时，错误处理通常涉及同时使用 IF 和 ISERROR 函数。

# ISERROR 函数

“公式错误”工作表中单元格 **E10** 包含 =ISERROR((C10-D10)/D10) 公式并返回 **FALSE**，因为 (C10-D10)/D10 计算结果不是错误。相反，单元格 **E11** 包含 =ISERROR((C11-D11)/D11) 公式并返回 **TRUE**，因为 (C11-D11)/D11 计算结果是错误。

![](img/0c7a357df7e57e06b6d9f631c219040c_361_1.png)

单元格 **E12** 包含 =IF(ISERROR((C12-D12)/D12), "n/a", (C12-D12)/D12) 公式并返回 11%。单元格 **E13** 包含 =IF(ISERROR((C13-D13)/D13), "n/a", (C13-D13)/D13) 公式并返回 n/a。

我提供这个简单示例，说明像 (C13-D13)/D13 这样的重复计算（在单元格 E13 中出现两次）如何潜入我们的公式中。IFERROR 被添加到 Excel 中，以在处理公式错误时消除重复计算，但我们发现自己多次重复公式片段的方式有无数种。在公式的多个位置进行相同的编辑总是很棘手，如果你漏掉了 _一个_ 位置，那么公式中可能会出现错误，这也可能影响其他单元格。这让我们回到了 LET 函数。

# 消除重复计算

“公式错误”工作表中单元格 E15 包含 =LET(calc, (C15-D15)/D15, IF(ISERROR(calc), "n/a", calc)) 公式并返回 11%，因为 (C15-D15)/D15 计算结果不是错误。让我们分解一下公式：

- Name1：此参数建立一个我们稍后可以在公式中使用的 calc Variable。

- name_value1：此参数包含我们计算的重复部分，(C15-D15)/D15，我们将能够通过 calc Variable 引用它，而不是重复计算两次。

- Name2_or_calculation：由于我们只需要一个 Variable，第三个参数包含 IF(ISERROR(calc), "n/a", calc) 计算。现在，(C15-D15)/D15 在公式中只出现一次，但被使用了两次。这意味着如果我们需要编辑公式，只需在一个位置进行编辑。

如果你正在 B 列跟随操作，你可以将在单元格 B15 中输入的公式复制到单元格 B16，无需进行任何编辑。

Tip

LET 函数在 Excel 2019 及更早版本中将返回 #NAME?。在第 9 章“Excel Quirks and Nuances”中，我解释了如何使用 Compatibility Checker 来确定工作表是否使用了任何与旧版 Excel 不向后兼容的工作表函数或功能。

现在，让我们使用 LET 创建第三个计算，以便我可以进一步阐述 Variables 的工作原理。

# Variables 限制

让我们回到 LET 函数工作表，如图 11.15 所示，以便我可以向你展示如何在 LET 函数中建立 Variables 而不在计算中使用它们：

