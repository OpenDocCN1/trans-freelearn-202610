

## 过滤数据

数据配方中的过滤节点使您能够快速过滤数据以适应使用场景。也就是说，您可以删除希望从输出数据集(s)中排除的数据行。

要过滤您的数据，请按照以下步骤操作：

1. 添加一个新节点（如 *Figure 4.3* 所示）到您的配方中并选择 **Filter**。
2. 添加过滤条件，如 *Figure 4.6* 所示。如果您选择在维度列上定义过滤条件，您可以从列表中选择过滤值。
3. 点击 **Apply**。
4. 保存配方。

以下屏幕截图显示了添加过滤条件时用户界面（UI）的外观：

![](img/06379e43783b9ddfee45e9da5192efe1_97_0.png)

Figure 4.6 – Adding filter conditions

4. 您可以通过在 **Group Columns** 字段中添加最多两个维度列、为每个列选择值并点击 **Apply** 来将维度透视为列。
5. 配置完聚合后，点击 **Apply**。
6. 保存配方。

这是添加聚合节点时 UI 的外观：

![](img/06379e43783b9ddfee45e9da5192efe1_99_0.png)

Figure 4.7 – Applying an aggregate node to a recipe

> **Note Regarding Aggregation And Data Granularity**
> 当您使用聚合节点时，较低（源）数据的粒度会在该数据配方的流中丢失。然而，最新的版本使您能够在聚合后通过连接将粒度带回来。

接下来，您将学习如何在 TCRM 中使用强大的转换工具。

## 转换数据

转换节点提供了大量的灵活性和功能 – UI 看起来像这样：

![](img/06379e43783b9ddfee45e9da5192efe1_100_0.png)

Figure 4.8 – Adding a transformation node

在本节中，将涵盖以下转换：

- Apply formulae
- Trim
- Substring
- Split
- Uppercase and lowercase
- Replace
- Dimension to measure
- Dimension to date
- Format dates
- Bucket
- Flatten
- Edit attributes
- Drop columns
- **Detect sentiment**
- **Predict missing values**
- **Discovery predict**

那么，让我们从第一个开始！

### Apply formulae

这会在配方中创建一个列，该列根据公式计算显示值。此计算将从同一行中的其他字段或跨行提取数据。您需要在 **EA-SQL format** 中输入公式 – 用于 *numeric*、*string* 和 *date* 数据的标准和自定义函数集合。

公式对话框看起来像这样：

![](img/06379e43783b9ddfee45e9da5192efe1_101_0.png)

Figure 4.9 – Custom formula dialog box without (left-hand side) and with (right-hand side) the multiple row formula option enabled

要向您的配方添加公式，请按照以下步骤操作：

1. 在 **Preview** 选项卡中选择一列。
2. 要为计算列添加公式，请在 **Transform** 工具栏中选择 **Formula** 按钮并选择 **Custom Formula**。然后公式编辑器会出现。
3. 在公式编辑器中，输入公式并使用正确的语法添加必要的参数。
4. 对于具有窗口函数的公式，请选择 **Multiple row formula** 选项。选择用于划分行的列 **Partition By**，用于对数据进行排序的列 **Order By**，以及 **Sort Direction** 中的排序方向。
5. 通过输入 API 名称或从 **Columns** 选项卡中选择列，按其 API 名称标识公式中的每个列。
6. 在 **Output Type** 中，在文本、数字、日期或日期时间之间进行选择。
7. 根据输出类型，您可以分配这些属性：**Precision**（数字列中的最大位数）、**Scale**（数字列中小数点右侧的位数）、**Length**（文本列中的最大字符数）以及 **Date Format**。
8. 在 **Default Value** 中，设置在表达式未返回结果时显示的值。
9. 您可以根据需要更改计算列的标签。
10. 要将公式作为 **Transform node** 中的步骤添加，请点击 **Apply**。计算列随后将出现在配方预览中。
11. **Save** 配方。

> **Tip**
> 常见计算在 **Transform** 工具栏中拥有自己的函数按钮 – 这些按钮会为您用正确的列和语法填充公式。工具栏会根据所选列的数量和类型显示适当的函数。要使用函数，只需点击 **Transform** 工具栏中的 **Function** 按钮即可。

有五种类型的函数可用：

- **Date and time functions**：这些用于调整或从配方中的日期计算值。
- **Numeric operators and functions**：这些用于从配方中的度量计算值。
- **String functions**：这些用于基于其他字符串创建值。
- **Window functions**：这些用于跨行执行计算。必须选择 **Multiple Row Formula** 选项才能使用窗口函数。
- **Additional functions**：这些与 `text`、`number`、`date_only` 和 `datetime` 数据类型一起使用，以向公式添加逻辑。

### Trim

此转换会从字符串中删除前导和尾随空格。您可以选择在以下三个位置之一显示结果：

- 原始列（覆盖）。
- 新列 – 保留原始列（新列需要给定标签）。
- 新列 – 删除原始列（新列需要给定标签）。

### Substring

这从值中的字符位置开始并提取指定长度的字符数到新字段。公式编辑器会出现，并且以下文本会被预填充：

```
substr([target column], position, length)
```

substring 函数所需的输入可以在对话框中找到，如下所示：

- **Output Type**
- **Length**
- **Default Value**
- **Show Results In**
- **Column Label**

### Split

此转换在指定的分隔符处将列值分为两个新值。所需的输入如下：

- **Delimiter**
- **Show Results In**
- **Column 1 Label**
- **Column 2 Label**

> **Note**
> Split transformation always creates two new columns.

### Uppercase and lowercase

此选项将所选字段中的值转换为大写或小写。

### Replace

此转换在字段中查找特定值并将其替换为新值。请注意，会创建一个新字段，其中包含已替换的值和未更改的值。您也可以使用 replace 转换将空值替换为值。

所需的输入如下：

- **Find** 或 **Search** for null（在搜索窗口中）
- **Replace**（在搜索窗口中）
- **Show Results In**
- **Column Label**

### Dimension to measure

此转换将 `Dimension` 列类型转换为 `Measure`，这允许对其值进行数学计算。您可以选择将结果显示在原始列中、在新列中并保留原始列，或在新列中并删除原始列。

### Dimension to date

此转换将 `Dimension` 列类型转换为 `Date`，这允许您按日期组件（如年份和月份）进行分组和过滤。您可以选择将结果显示在原始列中、在新列中并保留原始列，或在新列中并删除原始列。

### Format dates

此转换将所选 `Dimension` 列中的日期转换为指定格式。可选地，您可以将列类型转换为 `Date`。一致的格式允许您正确地按日期过滤和分组记录，包括按日期组件（如日）过滤。一致的日期格式还确保您可以将列类型从维度转换为日期。

