

- 13. 现在让我们打印结果。搜索一个 **Write Line** 活动，并将其放置在 **For Each** 循环下方。

- 14. 使用以下代码配置 **Write Line** 活动，该代码将最终列表转换为一个可在 **Output** 面板中打印的字符串变量：

```
String.Join(" | ", ModifiedList)
```

到达此点时，您构建的工作流应类似于此：

![](img/84b06ad2832c53c5f677f8ce4c80f184_229_0.png)

图 7.12 – 列表数据操作工作流

同样，您在 **Variables** 面板中创建的变量的最终配置应类似于此：

| 名称 | 变量类型 | 范围 | 默认值 |
|---|---|---|---|
| StudentsList | List<String> | ListManipulation | New List(Of String) from ["John", "Daniel", "Keith"] |
| EmployeeList | List<String> | ListManipulation | New List(Of String) from ["Diana", "Lizi", "Peter", "Daniel"] |
| CombinedPeopleList | List<String> | ListManipulation | 输入 VB 表达式 |
| ModifiedList | List<String> | ListManipulation | New List(Of String) |
| 创建变量 | | | |

图 7.13 – Variables 面板中的列表数据操作变量

- 15. 执行程序并观察 **Output** 面板中的输出。您应收到以下输出：

```
`JOHN | KEITH | ANDREW | DIANA | LIZI | PETER | DANIEL | JANE`
```

本节所执行的步骤使您很好地了解如何使用 `List` 变量操作数据。与数组相比，`List` 变量提供的其他功能（例如在运行时添加或删除元素）使处理数据元素变得轻松。列表可以存储任何类型的值，包括数值。数值数据具有其独特的功能，例如求和、除法、乘法等等。本章的下一部分将重点介绍几种常用的数值计算。

## 了解数值计算

在执行从简单到复杂的各种计算时，数值数据类型非常有用。我们可以使用数值执行许多计算，例如求和、计算平均值、查找最小值和最大值等等。在 UiPath 中执行数值计算非常简单。让我们尝试一个简单直接的示例。

创建一个 `List` 变量，用于存储学生上学期五门科目的成绩。我们的任务是计算成绩的总分和平均分。按以下步骤操作：

- 1. 打开 UiPath Studio 并创建一个新的 **Blank Process** 活动。
- 2. 双击 `Main.xaml` 文件在 **Designer** 面板中打开该文件。
- 3. 搜索 **Sequence** 活动并将其添加到 **Designer** 面板。
- 4. 使用以下代码创建一个类型为 String 的新 List 变量。该变量包含学生在五门科目中获得的结果：

```
New List(Of String) From {"75", "34.4", "54", "34", "55"}
```

列表中的分数为 String 类型。我们无法使用 String 数据类型执行计算。因此，在执行计算时需要将它们转换为数值数据类型。

- 5. 创建一个名为 TotalScore 的 Int32 类型新变量。
- 6. 创建另一个名为 AverageScore 的 Double 类型变量。
- 7. 拖放一个 For Each 活动，以便我们可以遍历列表中的项目。
- 8. 使用 String 类型配置循环的 TypeArgument 属性。
- 9. 将 Assign 活动拖放到 For Each 循环的主体中。我们将使用 Assign 活动通过在循环中累加每个项目的值来计算分数总和。
- 10. 通过提供变量名 TotalScore 来配置 Assign 活动的 To 部分。
- 11. 使用以下代码配置 Assign 活动的 Value 部分。列表中包含带小数的值。小数值无法直接转换为 Int32。因此，对于我们的场景，我们将首先将字符串转换为 Double，并将其四舍五入到最接近的整数，然后再将其转换为 Int32：

```
TotalScore + Convert.ToInt32(Math.Round(Double.Parse(item)))
```

- 12. 在 For Each 活动下方拖放一个 Write Line 活动，并将其配置为在 Output 面板中将 TotalScore 变量的值作为字符串写入。
- 13. 在最后一个活动下方放置另一个 Assign 活动，并使用以下代码配置 Value 部分，该代码使用 TotalScore 变量中的值生成平均分。我们需要列表中的项目数来计算平均分——StudentMarks.Count 代码会给出学生完成的科目数量：

```
TotalScore / StudentMarks.Count
```

- 14. 通过指定 AverageScore 变量来配置 Assign 活动的 To 部分。AverageScore 变量将保存前一步计算得出的平均分。
- 15. 放置一个 **Write Line** 活动在 For Each 活动下方，并通过将 `AverageScore` 中的值转换为 `string` 来对其进行配置，以便在 **Output** 面板中打印该值。到达此点时，您构建的工作流应类似于此：

![](img/84b06ad2832c53c5f677f8ce4c80f184_232_0.png)

图 7.14 – 数值数据操作工作流示例

- 16. 运行工作流并观察 **Output** 面板中的结果。您应该看到总数显示为整数，而平均值由小数值组成。

.NET Framework 中提供了许多可用于执行数值计算的函数。此处提供的示例是基础示例。其旨在让您了解执行某些数值操作（例如四舍五入、计算总数和求得平均值）的高级思路。您在本章中已获得的知识将为您探索 Math 库中的其他函数以及可用于数值计算的其他各种函数打下基础。请探索其他方法并熟悉这些函数，因为在为您的组织构建自动化解决方案时，它们将非常有用。

虽然数值数据类型具有其独特的功能，但 `Dictionary` 类型变量提供了一种使用键值对概念管理数据的独特方式。本章的下一部分将解释如何使用 `Dictionary` 变量操作数据。

## 了解字典操作

`Dictionary` 类型与 `List` 和 `Array` 类型有一个关键区别。列表和数组存储相同数据类型的多个数据元素，而字典可以存储每个数据元素及其唯一键。这种组合称为 **key-value** 对。用户在创建变量时选择键和值的数据类型。

`Dictionary` 类型主要用于存储配置详细信息或其他类似信息。您可能想知道为什么不应该使用 `List` 类型来存储和访问配置数据。在实际场景中，每个配置数据元素都有一个描述性名称，用于描述配置代表的内容。例如，如果配置文件包含有关 **Uniform Resource Locators (URLs)**、超时和文件路径的数据，则文件可能如下所示：

| 键 | 值 |
| :--- | :--- |
| BusinessURL | https://www.google.com |
| InputExcelFolderPath | C:\Invoice Files\ |
| ProcessTimeout | 10000 |

图 7.15 – 配置文件的示例结构

在类似场景中，字典变量使用描述性键，使用户更容易了解工作流中使用了哪些值。在列表或数组变量中无法实现相同概念，因为列表和数组中的值仅通过索引访问。

UiPath Studio 提供了一个易于使用的环境来处理字典，以执行初始化、添加、删除、提取和重新赋值等操作。让我们通过一个简单的示例来了解如何使用字典。

### 字典初始化

字典也是一种集合变量，类似于列表和数组。因此，初始化过程与这些数据类型相似。初始化发生在通过 Variables 面板创建变量或在工作流中使用 Assign 活动时。

让我们创建一个简单的 Dictionary 变量，用于保存个人信息，例如姓名、年龄、国家和出生日期。按以下步骤操作：

- 1. 打开 UiPath Studio 并创建一个新的空进程。
- 2. 在 Designer 面板中打开 `Main.xaml` 文件。
- 3. 拖放一个新的 Sequence 活动以开始构建工作流。
- 4. 将 Assign 活动从 Activities 面板拖放到 Sequence 容器中。
- 5. 打开 Variables 面板，单击“创建变量”选项，提供名称 "PersonalData" 来创建一个新的 Dictionary 变量。该变量的默认数据类型为 String。
- 6. 单击变量的“变量类型”字段以访问下拉菜单并选择“浏览类型”。此操作将打开“浏览并选择 .NET 类型”窗口。
- 7. 在窗口中搜索 Dictionary 类型，并选择位于 mscorlib.System.Collections.Generic 下的 Dictionary<TKey, TValue> 项目，如下图所示：

