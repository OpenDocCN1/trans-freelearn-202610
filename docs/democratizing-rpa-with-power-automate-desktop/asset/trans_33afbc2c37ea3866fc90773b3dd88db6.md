

- 添加项目到列表
- 从列表中移除项目
- 排序列表，洗牌列表
- 反转列表
- 从列表中移除重复项目

所有这些都相当自解释，并且会按其名称所暗示的方式执行。它们不会暴露任何额外的变量。

某些操作需要两个列表来运行。这些是合并列表、查找公共列表项和减去列表。然而，这些操作会创建一个新的列表，即相应操作的运算结果。

## 数据表

除了单维列表数据类型之外，我们还可以使用二维或表格型数据表来存储值。你可以将数据表视为一个简单的 Excel 电子表格。有一个特定的操作可以从 Excel 电子表格中读取数据并返回一个数据表。这些操作会创建和操作数据行，它们也位于 Variables 操作组中，但在 Data table 文件夹下。

要创建数据表变量，我们可以使用创建新的数据表选项。操作参数对话框中包含一个编辑按钮用于定义表格：

Figure 5.12 – Creating a data table

我们可以双击现有列来重命名它们，或按 + 按钮添加新列和行。保存将存储值并返回到操作参数对话框，显示新数据表包含的行数和列数。为了演示数据表的不同操作，我创建了一个包含六行两列的简单表格，并将其命名为 CarsTable，如以下屏幕截图所示：

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_135_0.png)

Figure 5.13 – The Cars table

现在，如果我们想向该表格添加新行，可以使用 **Insert row into data table** 操作。在相应的操作参数对话框中，我们需要选择数据表及其 **Into location**。在这里，我们可以选择 **End of the data table** 或 **before a row index**，选择后需要指定。添加的新值必须符合特定的语法。要添加一条制造商为 Chevrolet 且国家为 USA 的新汽车记录，我们需要输入 `%['Chevrolet', 'USA']%`。

我们将在下一节讨论 `%` 记法，但本质上，我们需要在 **New value(s)** 参数中传递一个列表或数据行，并用百分号封装。

**Update data table item** 的工作方式略有不同，因为它只是更新某个特定的单元格或项目。我们需要将列和行号作为参数值传递，以及要设置的新值。

要删除一行，请使用 **Delete row from data table** 并将行索引作为参数传递。

**Find or replace in data table** 操作是最复杂的，因为它具有很多功能。如其名称所示，它可以只是查找或查找并替换。可以利用正则表达式查找某些文本，并指定是否使用大小写敏感搜索。此操作的结果是另一个数据表，其中包含匹配项的列和行作为单独记录。

让我们看一下以下示例，看看它是如何工作的。

基于 Cars 表，我们想要插入包含一辆新汽车的新行。现在，我们想在所有位置将文本 USA 替换为 United States。我们可以创建一个如下所示的流程：

Figure 5.14 – Working with data tables

这里使用了 **Find or replace in data table** 操作来查找术语 USA 的所有出现。我将其配置如下：

**Main**

| Action | Description |
| --- | --- |
| Create new data table | Create a new data table and store it into CarsTable |
| Insert row into data table | Appends the row `['Chevrolet', 'USA']` at the end of CarsTable |
| Find or replace in data table | Finds all items in CarsTable containing the text `'USA'` and stores the matched cells into DataTableMatches |
| For each CurrentItem in DataTableMatches | |
| Update data table item | Updates the data table CarsTable at row index `CurrentItem['Row']` and column `CurrentItem['Column']` with the value `'United States'` |
| End | |

## Variables

Search variables

Input / output variables 0

There aren't any input or output variables here yet

Flow variables 3

- CarsTable
- CurrentItem
- DataTableMatches

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_138_0.png)

Figure 5.15 – The Find and replace in data table action

此操作会创建一个名为 **DataTableMatches** 的新数据表，在我们的例子中包含两条记录，即美国汽车的记录。流程运行后双击此变量会显示此表格的内容：

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_141_0.png)

Figure 5.18 – The data table before and after the update

### Find and replace with “Replace” mode

请注意，我们本可以使用“Replace”模式的 Find and replace 操作，而不是仅查找。通过这样做，我们可以在参数操作对话框中确定用于替换的文本片段。这将是一种更方便的方式来替换数据表中的文本，我们就不需要遍历此操作产生的数据表了。但有时，我们只需要查找给定的文本片段，而不直接用其他文本替换它或执行更复杂的操作。前面示例中展示的技术就可以用来完成这些任务。

这个小例子说明了数据表的工作方式。在本书中，我们还将使用其他会产生数据表的操作。它们如下：

- Read from Excel worksheet (the Excel action group)
- Execute SQL statement (the Database action group)
- Extract data from web page (the Browser automation action group in the Web data extraction subfolder)

这些操作将在相应的章节中介绍。

## 自定义对象

到目前为止，我们查看的数据类型都表现出相似的结构，例如数据表，其中相关记录始终具有相同的结构。然而，我们也可以使用一种称为 **object** 的数据类型，它由一组属性和相关值描述。让我们以本章前面出现的 `cars` 示例为例。例如，一辆汽车具有以下属性：品牌、颜色、制造年份、车辆类型等。有一段时间以来，所谓的 **JavaScript Object Notation (JSON)** 格式在此类对象的表示中占据主导地位。这可能看起来如下：

```
{
  'Brand': 'BMW',
  'Color': 'blue',
  'year of manufacture': '2020',
  'type of vehicle': 'SUV'
}
```

可以为自定义对象创建输入/输出变量。你所要做的就是选择 **Custom object** 作为数据类型。然后，你可以在单独的对话框中编辑对象的数据：

**Tip:** If you want to have a readable JSON file that you can easily edit, you can use the free JSON Formatter at https://jsonformatter.org/.

Figure 5.19 – Creating and editing a custom object

当我们需要创建与某些外部 Web 服务通信的流程时，此数据类型特别有用，这些服务期望或返回 JSON 对象。我们将在本书后面发现这一点。

要将自定义对象转换为有效的 JSON 字符串，我们可以使用 Convert custom object to JSON 操作，它期望自定义对象作为输入并生成包含 JSON 字符串的新变量。反向操作是 Convert JSON to custom object。两个操作都在 Variables 操作组中。我们将在本书后面查看这些操作的使用示例。

## 文本数据类型及对应操作

我们现在已经了解了 PAD 自带复杂的复杂数据类型。文本数据类型不属于这一组数据类型，但有大量操作仅处理此数据类型。因此，从我的角度来看，在这里简要提及是有意义的。

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_143_0.png)

这些操作位于 Text 操作组中，执行你期望文本编辑器完成的所有任务，并且在流程设计中非常有用。其中包括 Split text、Get subtext、Crop text 等等。我想特别强调 Recognize entities in text 操作，它能够使用 AI 从一段文本中提取某些实体。我鼓励你为此目的尝试 Microsoft 的相应示例。为此，切换到 Console 窗口并进入 Examples 部分。在 Text Manipulation 标题下，你会找到一个名为 Extract phone numbers and emails from text 的条目。编辑此流程并查看调用，以体验首次与 PAD 交互时 AI 的强大功能。

## % 记法

在我们之前看到的所有示例中，我们都需要使用 % 记法，每当我们想使用变量时都需要它。变量或其中包含一个或多个变量的表达式必须始终用 % 字符括起来，因为正是这部分会被 PAD 评估。在两个 % 字符之间，可以使用变量或硬编码值一起使用简单的算术运算、逻辑比较或表达式。以下是一些示例：

- `%CarName%`: Returns the content of a variable called `CarName`.
- `%'CarName'%`: Please notice the quotes. This returns the hard-coded `CarName` value.
- `%Counter + 1%`: Returns the value of the `Counter` variable increased by 1.
- `%(Counter + 1) > 15%`: Makes use of parenthesis and returns a Boolean value of `True` if the `Counter` variable has a value of 15; otherwise, it returns False.
- `%NettoPrice + VAT%`: Returns the sum of the values of the `NettoPrice` and `VAT` variables.

### Using the % sign as a simple character

