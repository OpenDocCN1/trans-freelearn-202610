
下一步是将所有 **DateTime** 列格式化为 **Time**。下图显示了运行上述 DAX 表达式的结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_114_0.png)

## 图 2.40：使用 DAX 生成时间表

## 注意

> 上图显示了按 **Time** 列升序排序的结果。

下一步是在 **Internet Sales** 表的 **Order Time** 列与 **Time** 表之间创建关系。

下图展示了我们如何向报表画布添加一些面积图，并按不同的时间桶可视化 **Internet Sales**：

![](img/5d2bce74fd6fceebed14fd1488bf7088_115_0.png)

图 2.41：按不同时间桶可视化 Internet Sales

最佳实践是尽可能将所有数据转换（例如生成日期表或时间表）移动到源系统。如果由于任何原因无法做到这一点，那么最好在 Power Query 编辑器中处理这些转换。

## 总结

在本章中，我们讨论了 DAX 的不同方面以及它如何影响我们的数据建模。我们考察了一些真实场景和挑战，以及如何使用 DAX 解决它们。

在下一章中，我们将更详细地了解 Power Query (M) 表达式语言。我们还将通过一些实际操作场景，在 Power Query 中逐步准备星型架构。

## 加入我们的 Discord！

加入 Packt Discord 服务器上的大数据与分析社区！

与 558 位其他成员一起交流，享受免费的语音和文本聊天。

https://packt.link/ips2H

![](img/5d2bce74fd6fceebed14fd1488bf7088_116_0.png)

## 在 Power BI 中介绍 Power Query M 公式语言

Power Query 是 Microsoft 提供的一种数据准备技术，可以连接到来自各种技术的许多不同数据源，使企业能够集成数据、转换数据、使其可用于分析，并从中获得有意义的见解。Power Query 目前可以连接到许多数据源。

它还提供了一个**自定义连接器软件开发工具包（SDK）**，第三方可以使用它来创建自己的数据连接器。Power Query 最初作为 Excel 加载项引入，很快成为 Microsoft 数据平台中数据准备和转换的重要组成部分。

Power Query 目前与一些 Microsoft 产品集成，例如 **Dataverse**（也称为 **Common Data Service (CDS)**）、**SQL Server Analysis Services 表格模型 (SSAS Tabular)**、**SQL Server Integration Services (SSIS)**、**Azure Data Factory (ADF)** 和 **Azure Analysis Services (AAS)**，以及 Power BI 和 Excel。因此，了解 Power Query 有助于数据专业人员支持多种技术中的数据准备。你可以通过以下链接在 Microsoft 官方网站上找到使用 Power Query 的 Microsoft 产品和服务的完整列表：

https://docs.microsoft.com/en-us/power-query/power-query-what-is-power-query?WT.mc_id=DP-MVP-5003466#where-can-you-use-power-query

Power Query M 是一种公式语言，能够连接到各种数据源，以混合和匹配这些数据源之间的数据，然后将其加载到单个数据集中。在本节中，我们将介绍 Power Query M。

## Power Query 区分大小写

虽然 Power Query 是一种*区分大小写*的语言，但**数据分析表达式（DAX）**不区分大小写，这可能会让一些开发人员感到困惑。Power Query 和 DAX 是截然不同的两个领域，它们在 Power BI 中结合在一起，负责处理数据工作的不同方面。Power Query 不仅在语法上区分大小写，在与数据交互时也区分大小写。例如，如果我们运行以下函数，就会收到错误消息：

```
datetime.localnow()
```

这是因为正确的语法如下：

```
DateTime.LocalNow()
```

忽略 Power Query 在数据交互中的大小写敏感性可能会成为一个难以识别且耗时的问题。一个真实世界的例子是：当我们从一个包含小写字符的数据源获取**全局唯一标识符（GUID）**值时，然后从另一个包含大写字符的数据源获取其他一些 GUID 值。当我们在 Power Query 中匹配这些值以合并两个查询时，不会得到任何匹配的值。但如果我们将小写 GUID 转换为大写，这些值就会匹配。

在 Power Query 中比较两个字符大小写不同的字符串值不会引发任何错误。数据混搭引擎只是比较两个值并返回 *false*，因为这两个字符串不相同。因此没有错误可引发；但结果是不正确的。

例如，以下两个 GUID 值在 Power Query 中不相等，而在 DAX 中相等：

```
D5E99E0E-0737-45B2-B62A-4170B3FEFC0E
d5e99e0e-0737-45b2-b62a-4170b3fefc0e
```

## 查询

在 Power Query 中，查询包含由 `let` 和 `in` 语句封装的**表达式**、**变量**和**值**。`let` 和 `in` 语句块的结构如下：

```
let
    Variablename = expression1,
    #"Variable name" = expression2
in
    #"Variable name"
```

如上述结构所示，变量名中可以包含空格。但是，我们需要使用井号（#）后跟引号来封装变量名，例如 #"Variable Name"。通过在查询中定义变量，我们在 Power Query 中创建了一个查询公式步骤。查询公式步骤可以引用任何先前的步骤。最后，查询输出是紧跟在 in 语句之后的变量。每个步骤都必须以逗号结尾，除了 in 语句之前的最后一个步骤。

## 表达式

在 Power Query 中，表达式是产生值的公式。例如，下图显示了一些表达式及其结果值：

![](img/5d2bce74fd6fceebed14fd1488bf7088_121_0.png)

图 3.1：表达式及其值

**记录值**：记录是一组构成数据行的字段。要创建记录，我们使用方括号 []。在方括号内，我们写出*字段名*和一个*等号*，后跟字段的*值*。我们使用逗号分隔不同的字段及其值，如下所示：

```
[
    First Name = "Soheil"
  , Last Name = "Bakhshi"
  , Occupation = "Consultant"
]
```

下图显示了该表达式及其值：

![](img/5d2bce74fd6fceebed14fd1488bf7088_124_0.png)

图 3.3：在 Power Query 中定义记录

注意
定义记录时，我们不需要将字段名放在引号中。如上图所示，记录是垂直显示的。

我们可以展开结构化列以获取其基元值，如下图所示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_127_0.png)

图 3.6：带有结构化列的表 下图显示了将结构化列展开为新行后的结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_127_1.png)

图 3.7：带有展开的结构化列的表

函数值：函数是接受输入参数并产生结果的值。要创建函数，我们将参数列表（如果有）放在括号中，后跟输出数据类型。我们使用 goes-to 符号（=>），后跟函数定义。例如，以下函数计算当前日期的月末日期：

```powerquery
() as date =>
Date.EndOfMonth(Date.From(DateTime.LocalNow()))
```

上述函数没有任何输入参数，但会产生输出。下图显示了一个无参数函数调用，该函数返回当前日期（今天）的月末日期：

![](img/5d2bce74fd6fceebed14fd1488bf7088_128_0.png)

图 3.8：调用自定义函数

## 类型

在 Power Query 中，值具有类型。**类型值**对其他值进行分类。类型有两个常规类别：**基元类型**和**自定义类型**。

## 基元类型

值可以具有基元类型，如下所示：

- `binary`
