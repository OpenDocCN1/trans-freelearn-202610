

- 合并列
- 添加自定义列
- 从示例中添加列
- 复制列
- 筛选行
- 使用分组依据
- 追加查询
- 合并查询
- 复制和引用查询
- 替换值
- 从文本中提取数字
- 处理日期、日期时间及日期时间/时区
- 透视表

虽然取消透视表也是常见的转换，但本章不涉及，因为我们在 *第 4 章，从各种来源获取数据* 的 *Excel* 部分中已经讨论过它。

让我们更详细地查看它们。

## 数据类型转换

数据类型转换是 Power Query 中最常见的步骤之一，但如果管理不当，它也是最重要且可能变得棘手的一步。Power BI 的一个很酷的功能是，如果启用，可以自动检测数据类型。虽然在许多情况下这是一个方便的功能，但它可能是以后出现一些问题的根源。需要特别注意的关键点是 Power BI 如何自动检测数据类型。Power BI 根据前几百行自动检测列数据类型；问题可能就出在这里，因为数据类型不是基于整个数据集检测的。相反，数据类型是基于加载用于预览的部分检测的。在大多数情况下，我们处理的是表值中的数据类型转换。我们要么使用 Power Query 编辑器 UI，要么手动编写表达式；在这里，我们使用以下函数：

```
Table.TransformColumnTypes(Table as table,
TypeTransformations as list, optional Culture as nullable text)
```

在 Table.TransformColumnTypes() 函数中，我们有以下内容：

- `Table` 通常是上一步的结果。
- `TypeTransformations` 接受列名及其对应数据类型的列表。
- `Culture` 是可选的，可为空的文本，例如 `en-NZ` 指定文化。

我们已经在第 3 章《Power Query 编辑器中的数据准备》中的《Power BI 中 Power Query (M) 公式语言简介》部分、《类型》小节中讨论了 Power Query 中可用的类型。下表显示了类型、其表示以及我们可用于指定数据类型的语法：

| 类型种类 | 类型表示 | 指定类型语法 1 | 指定类型语法 2 |
|---|---|---|---|
| binary | Binary | type binary | Binary.Type |
| date | Date | type date | Date.Type |
| datetime | Date/Time | type datetime | DateTime.Type |
| datetimezone | Date/Time/Zone | type datetimezone | DateTimeZone.Type |
| duration | Duration | type duration | Duration.Type |
| list | List | type list | List.Type |
| logical | Logical | type logical | Logical.Type |
| null | Null | type null | Null.Type |
| number | 整数 | - | Int64.Type |
| number | 小数 | type number | Int64.Type |
| number | 定点小数 | - | Currency.Type |
| number | 百分比 | - | Percentage.Type |
| record | Record | type record | Record.Type |
| text | Text | type text | Text.Type |
| time | Time | type time | Time.Type |
| type | Type | type type | Type.Type |
| function | Function | type function | Function.Type |
| table | Table | type table | Table.Type |
| any | Any | type any | Any.Type |
| none | None | type none | None.Type |

表 5.1：Power Query 类型、其表示及其语法

让我们通过一个场景更详细地了解这一点。

假设我们收到了别人制作的《第 5 章，常见数据准备步骤，Raw.pbix》文件，我们必须开发一份报表。请记住以下几点：

- 我们已经知道之前的开发人员已连接到源 Excel 文件。
- 我们已将数据加载到数据模型中，而未在 Power Query 中做任何更改。
- 我们在数据模型中创建了一些关系。
- 开发人员指出，地理表中的 PostalCode 列包含许多空白值，这些值与源 Excel 文件不匹配。
- 开发人员还注意到，使用日期表中的 Date 列和表视觉对象上的 Internet Sales 度量时，结果很奇怪。

我们想调查并修复这些问题。让我们打开《第 5 章，常见数据准备步骤，Raw.pbix》文件，并按以下步骤操作：

- 1. 打开 Power Query 编辑器并从查询窗格中选择地理表。
- 2. 当我们创建示例文件时，我们没有更改数据类型。最后一步是自动创建的，并且数据类型是自动检测的。
- 3. 向右滚动数据视图。在这里，我们可以看到 PostalCode 列的列质量栏变红，这意味着示例数据（前 1,000 行）中存在错误。
- 4. 在数据视图中向下滚动一点以找到错误的单元格。单击产生错误的单元格以查看错误消息。
- 5. 如以下屏幕截图所示，错误是由不正确的数据类型转换引起的：

![](img/5d2bce74fd6fceebed14fd1488bf7088_248_0.png)

图 5.2：Power BI 错误的数据类型检测导致错误

修复问题很简单。在这里，我们必须将 PostalCode 列的数据类型设置为文本，这是正确的数据类型。

- 6. 单击列类型指示器按钮。
- 7. 单击文本。
- 8. 单击更改列类型消息上的替换当前按钮：

![](img/5d2bce74fd6fceebed14fd1488bf7088_249_0.png)

图 5.3：更改列数据类型

如步骤 8 所示，我们在已应用步骤中没有获得新步骤，但问题已解决。在以下屏幕截图中，列分布不再显示任何问题迹象：

![](img/5d2bce74fd6fceebed14fd1488bf7088_249_1.png)

图 5.4：数据类型转换问题已解决

在前面的示例中，我们快速找到了并修复了问题。但在许多情况下，即使我们不使用自动数据类型检测功能，数据转换问题及其修复也并非如此简单。让我们应用更改，将文件另存为新文件，并命名为《第 5 章，常见数据准备步骤，Fixed PostCode.pbix》。

现在，单击 Power BI Desktop 上的数据视图，导航到地理表，并查看 PostalCode 列的数据以确保我们迄今为止所做的更改已修复该问题。以下图像显示了结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_250_0.png)

图 5.5：确认空白邮政编码问题已解决

如前面的图像所示，PostalCode 列不再包含空白值。

现在我们解决了第一个问题，让我们关注下一个问题，即在表视觉对象上使用日期表中的 Date 列和 Internet Sales 度量时结果不正确。

为了重现问题，我们在报表画布上放置一个表视觉对象，选择日期表中的 Date 列和 Internet Sales 度量，问题很快就会出现。如以下图像所示，结果看起来不正确：

![](img/5d2bce74fd6fceebed14fd1488bf7088_253_0.png)

图 5.8：Internet Sales 表中的 OrderDateTime 列没有空白值

因此，下一步要确保的是，日期表中的 Date 列和 Internet Sales 表中的 OrderDateTime 列的数据类型匹配。我们可以在 Power BI Desktop 中的多个位置检查列的数据类型。这里有一个：

- 1. 单击报表选项卡。
- 2. 从字段窗格中选择列。
- 3. 在功能区的列工具选项卡上查看数据类型。

以下图像显示了前面的步骤：

图 5.9：在 Power BI Desktop 中检查列的数据类型

查看 Internet Sales 中的 OrderDateTime 列和日期表中的 Date 列的数据类型可知，Date 列的数据类型为日期，而 OrderDateTime 列的数据类型为日期/时间。从数据建模角度来看，这些数据类型是兼容的。尽管如此，OrderDateTime 列中的数据具有时间元素。因此，OrderDateTime 列中唯一匹配的值是那些时间元素为 12:00:00 AM 的值。为了确保这一点，我们可以做一个快速测试。

我们可以在报表画布上放置一个表，同时并排显示日期表中的 Date 列和 Internet Sales 表中的 OrderDateTime 列。

以下屏幕截图显示了结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_254_0.png)

![](img/5d2bce74fd6fceebed14fd1488bf7088_254_1.png)

