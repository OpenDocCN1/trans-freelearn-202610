

+   5. 提交此步骤后，**Product** 表中的所有列都会立即重命名。

下图显示了结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_172_0.png)

图 3.47：一次性重命名所有列

+   6. 最后一步是将新步骤重命名为更有意义的名称。为此，请右键单击该步骤，在上下文菜单中单击**重命名**，然后输入新名称，如下图所示：

## 递归函数

递归函数是指在函数内部调用函数自身。**阶乘**是一种数学计算，用于将从任意选定数字递减到 1 的所有正整数相乘。在数学中，感叹号表示阶乘计算（例如 $n!$）。下面的公式展示了**阶乘**的数学计算：

$n! = n * (n - 1)!$

正如前面的计算所示，**阶乘**计算是一种递归计算。在**阶乘**计算中，我们可以选择一个正整数（大于 0 的整数），但 0 是一个例外；如果选择 0，则结果为 1。以下示例有助于更清楚地理解**阶乘**计算：

```
10! = 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1 = 3,628,800
5! = 5 * 4 * 3 * 2 * 1 = 120
1! = 1
0! = 1
```

我们需要使用 @ 运算符来引用函数自身。例如，以下函数计算数值输入的阶乘：

```
let
    Factorial =
        (ValidNumber as number) as number =>
            if ValidNumber < 0
                then error "Negative numbers are not allowed to calculate Factorial. Please select a positive number."
                else
                    if ValidNumber = 0
                        then 1
                        else ValidNumber * @Factorial(ValidNumber - 1)
in
    Factorial
```

如果输入值为 0，我们返回 1；否则，我们以递归方式计算**阶乘**。

你注意到代码中的 `error` 部分了吗？我们可以使用 `error` 关键字引发自定义错误消息。因此，在前面的代码中，我们会检查输入值是否有效；如果无效，就会引发自定义错误消息。下图显示了使用无效数字调用**阶乘**函数时的结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_175_0.png)

图 3.49：使用无效值调用阶乘函数时引发错误

有时，即使输入无效，我们也不希望引发任何错误消息，而是希望显示一个常量值。以下代码展示了 `Factorial` 函数的另一个版本：它检查输入值是否为正数；如果是，则计算阶乘，否则返回 -1：

```
let
    Factorial =
        (ValidNumber as number) as number =>
            if Number.Sign(ValidNumber) = 1
            then
                if ValidNumber = 0
                then 1
                else ValidNumber * @Factorial(ValidNumber - 1)
            else -1
in
    Factorial
```

下图显示了调用阶乘函数计算 10! 的结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_176_0.png)

图 3.50：调用阶乘函数计算 10! 的结果

## 总结

本章介绍了 Power Query M 公式语言的不同方面，并探讨了如何使用 Power Query 编辑器。我们还研究了实际场景及其挑战，这些因素会直接影响我们的工作效率，并学习了如何更高效地管理数据准备步骤。

下一章将讨论从各种数据源获取数据、连接模式，以及它们如何影响数据建模。

## 加入我们的 Discord！

加入 Packt Discord 服务器上的大数据与分析社区！

与其他 558 名成员一起交流，享受免费的语音和文字聊天。

https://packt.link/ips2H

## 从各种源获取数据

在前面的章节中，我们讨论了数据建模的一些方面，包括 Power BI 中的各种层、数据如何在不同层之间流动、数据分析表达式（DAX）中的虚拟表，以及它们与数据建模之间的关系。我们还讨论了如何利用查询参数，以及如何在 Power Query 编辑器中使用 Power Query 公式语言创建自定义函数。

本章讨论如何从各种源获取数据，并介绍从某些源获取数据时已知的挑战和常见陷阱。我们还将讨论数据源认证，以及不同数据源的各种连接模式。

在本章中，你将了解以下主题：

- 从常见数据源获取数据
- 了解数据源认证
- 使用连接模式
- 使用存储模式
- 了解数据集存储模式

### 从常见数据源获取数据

借助 Power BI，我们可以连接到许多不同的数据源。本节将介绍一些可以在 Power BI 中使用的常见数据源。我们还将探讨连接到这些数据源时常见的陷阱。不过在开始之前，让我们先花一点时间讨论许多 Power BI 开发人员和用户对获取数据这一概念存在的一个常见误解，那就是获取

### 右键单击 Parameter1，然后单击“转换数据”

处理存储在表中的数据通常比处理工作表更加直接。

+   13. 单击 Sales_Data 表。
+   14. 单击“确定”。

前面的步骤在下图中有所突出显示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_183_0.png)

图 4.6：浏览要合并的 Excel 文件

前面的步骤会在 Power Query 编辑器中创建四个新查询，如下图所示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_184_0.png)

图 4.7：为合并文件而自动创建的查询和文件夹

让我们来看看 Power Query 编辑器自动生成的四个新查询：

- **Parameter1**：一个 **Binary** 查询参数，用于 **Transform File** 自定义函数和 **Transform Sample File** 查询。
- **Sample File**：这是我们的原始查询，只增加了一个导航到第一个 Excel 工作簿的步骤。
- **Transform File**：一个接受 **Parameter1** 作为输入参数的自定义函数。然后，根据我们在*步骤 13*中的选择，它会浏览 Excel 文件，从工作表或表中读取数据（在本例中为表）。
- **Transform Sample File**：一个使用 **Parameter1** 打开 Excel 文件的示例查询。该示例查询被禁用，不会加载到数据模型中。

在某些情况下，我们需要保留某些文件的元数据，例如**创建日期**（你可以在*图 4.8*中看到）。**Attribute** 列是一个由记录组成的结构化列，用于保存更多元数据。这里留给你自行进一步研究。不过，在此场景中，我们希望保留**创建日期**列，以供将来参考。现在需要按如下方式修改自动创建的步骤：

+   1. 单击 **Removed Other Columns1** 步骤右侧的齿轮图标。
+   2. 勾选 **Date created** 列。

## CSV/文本/TSV

虽然 Power BI 为 CSV 和 TXT 文件提供了专用连接器，但没有专门的连接器支持制表符分隔值（TSV）文件。本节将通过一个示例场景介绍这三种文件类型。

在本章前面的部分中，我们已经设法导入存储在文件夹中的销售数据。现在，业务部门收到了一些以各种格式存储的数据转储文件。我们拥有 CSV 格式的 Product 数据、TSV 格式的 Product Subcategory 数据，以及 .txt 格式的 Product Category 数据。我们需要将这些文件中的数据导入 Power BI。我们使用 Chapter 4, Get Data From Various Sources.pbix 示例文件：

https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/ea3530461d68ff6b08af69d1acdbc4c26204a63c/Samples/Chapter%2004/Chapter%204,%20Get%20Data%20From%20Various%20Sources.pbix

下图显示了要导入 Power BI 的文件：

![](img/5d2bce74fd6fceebed14fd1488bf7088_186_0.png)

图 4.10：要导入 Power BI 的 CSV、TXT 和 TSV 文件

我们首先从 Product Category 文件获取数据，步骤如下：

+   1. 在 Power Query 编辑器中，单击“新建源”下拉按钮。

