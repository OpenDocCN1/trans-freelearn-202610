
2. 点击**文本/CSV**。
3. 导航到包含示例文件的文件夹，然后选择**Product Category.txt**文件。
4. 点击**打开**。
5. Power BI 正确检测到了**文件原始格式**和**分隔符**，因此点击**确定**。

下图展示了上述步骤：

![](img/5d2bce74fd6fceebed14fd1488bf7088_187_0.png)

图4.11：从Txt文件获取数据

我们已成功从**Power Query 编辑器**连接到**Product Category.txt**文件。

下一步是获取存储在TSV文件中的**Product Subcategory**数据。如前所述，目前Power BI中没有专门用于TSV文件的连接器。但这并不意味着我们无法从TSV文件获取数据。顾名思义，TSV文件用于以文本格式存储制表符分隔的数据。因此，我们必须使用文本/CSV连接器来获取TSV文件。为此，请按以下步骤操作：

1. 再次点击“新源”下拉按钮。
2. 点击**文本/CSV**。
3. 导航到包含示例文件的文件夹，并从文件类型下拉列表中选择“所有文件(*.*)”。
4. 出现**Product Subcategory.tsv**文件；选择该文件。
5. 点击**打开**。
6. Power BI 正确检测到了**文件原始格式**和**分隔符**，因此点击**确定**。

上述步骤在下图中突出显示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_188_0.png)

图4.12：从TSV文件获取数据

到目前为止，我们从TXT和TSV文件格式的基于文本的数据源中获取了数据，一切都进行得很顺利。但情况并非总是如此。实际上，在处理未经整理的文件型数据源时，我们可能会遇到许多问题。让我们来看一下在处理基于文本的数据时可能面临的最常见挑战之一。现在我们已有了**Product Category**和**Product Subcategory**数据，是时候继续获取以CSV格式存储的**Product**数据了。要从CSV文件获取数据，我们使用之前从*.txt获取数据时所用的同一个文本/CSV连接器，因此不必重复这些步骤。

*图4.13*中的**ProductKey**列的质量条显示该列存在一些错误。然而，**Product**表只有607行，如状态栏所示。请注意，示例数据中出现了错误。有时候，错误可能直到我们将数据导入数据模型时才显现。在我们的案例中，我们可以滚动数据预览，找到第204行中的错误单元格。在发生错误之前的最后一行，**ProductKey**值为226。

我们点击错误单元格以查看错误消息。

更仔细地查看数据会发现其他一些问题，如下所示：

- **ProductAlternateKey**列中的日期值不正确。
- **ProductSubcategoryKey**列的数据类型不正确。
- **ProductSubcategoryKey**列中的日期值不正确。这些值必须是数字类型。

虽然在某些情况下我们可以移除错误，但在我们的案例中，数据显示在错误的列中。下图说明了这些问题：

图4.13：从CSV获取数据时产生的错误

让我们更详细地查看数据。虽然我们只有607行数据，但在记事本++等文本编辑器中打开CSV文件是合理的。

> 注意
>
> 你可以从此处免费下载记事本++：https://notepad-plus-plus.org/downloads/。

在记事本++中，启用“显示所有字符”，然后搜索文件中的ProductKey值为226的位置。这样，发生了什么就更加明显了。文本中出现了回车符(CR)和换行符(LF)。

当我们进一步调查时，发现**EnglishProductDescription**列包含(CR)和(LF)字符。当用户在源系统中输入产品描述时按下键盘上的回车键，就可能发生此问题。然后，数据从源系统导出到CSV。这个问题也可能出现在任何其他文本类型的列中。如下图中所示的数据中的其他一些问题，在查看数据时并不明显。

![](img/5d2bce74fd6fceebed14fd1488bf7088_190_0.png)

这些问题在**Power Query 编辑器**中也没有产生任何错误，因此可能很难发现。然而，当我们稍后对数据进行核对时，它们会造成一些问题：

![](img/5d2bce74fd6fceebed14fd1488bf7088_191_0.png)

图4.14：在记事本++中打开的Product.csv文件揭示了一些问题

虽然我们已经找到了罪魁祸首，但同样的问题也可能发生在数据中其他不太明显的地方。处理这个问题最简单的方法是在源中修复它，但在我们的案例中，我们只能访问导出为CSV格式的数据，而无法访问源系统。因此，我们必须在Power Query中修复此问题。我们可能会认为可以用空白替换**(CR)/(LF)**字符。但现实是，**(CR)/(LF)**字符代表新的一行数据。因此，我们不能简单地用空白替换它们，然后期望问题得到解决。

幸运的是，在我们的案例中，这个问题修复起来并不复杂。查看错误的行，我们可以看到**(CR)/(LF)**字符之前总是有一个空格字符。**(LF)**字符也是如此。为了修复此问题，我们首先需要获取文本，并在将其转换为表格之前修复问题。虽然我们可以这样修复问题，但如果将来在其他CSV文件中再次发生这种情况该怎么办？在*第3章“Power Query 编辑器中的数据准备”*中，我们学习了如何创建自定义函数。要解决当前场景中面临的问题，我们需要一个自定义函数，它执行以下操作：

1. 接受文件路径作为文本值。
2. 将文件作为文本打开。
3. 将所有出现的` #(cr,lf)`（读作空格、回车、换行）替换为空字符串("")。
4. 在上一步的文本结果中，将所有出现的` #(lf)`（读作空格、换行）替换为空字符串("")。
5. 将文本转换为二进制。
6. 将上一步的结果作为CSV打开。
7. 将第一行提升为标题。
8. 更改列类型。

通过以下表达式，我们可以创建一个满足所需条件的自定义函数：

```
// fnTextCleaner
(FilePath as text) as table =>
let
    Source = File.Contents(FilePath),
    GetText = Text.FromBinary(Source),
    ReplaceCarriageReturn = Text.Replace(GetText, " #(cr,lf)", ""),
    ReplaceLineBreaks = Text.Replace(ReplaceCarriageReturn, " #(lf)", ""),
    TextToBinary = Text.ToBinary(ReplaceLineBreaks),
    #"Imported CSV" = Csv.Document(TextToBinary, [Delimiter=",", Columns=25, Encoding=1252, QuoteStyle=QuoteStyle.None]),
    #"Promoted Headers" = Table.PromoteHeaders(#"Imported CSV", [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {{"ProductKey", Int64.Type}, {"ProductAlternateKey", type text}, {"ProductSubcategoryKey", type number}, {"WeightUnitMeasureCode", type text}, {"SizeUnitMeasureCode", type text}, {"EnglishProductName", type text}, {"StandardCost", type text}, {"FinishedGoodsFlag", type logical}, {"Color", type text}, {"SafetyStockLevel", Int64.Type}, {"ReorderPoint", Int64.Type}, {"ListPrice", type text}, {"Size", type text}, {"SizeRange", type text}, {"Weight", type text}, {"DaysToManufacture", Int64.Type}, {"ProductLine", type
```

图4.15：调用fnTextCleaner函数修复Product.csv文件中的文本问题

如上图所示，我们现在得到**606**行，这是正确的。

## Excel

Excel是Power BI中最常用的数据源之一。Power BI有一个专门的Excel连接器，但取决于Excel文件的存储位置，我们可能需要使用**Web**连接器，这并不像看起来那么简单。无论使用哪种方法连接到Excel文件，处理Excel数据源并不总是那么简单。本节解释了连接到Excel文件的两种最常见场景：存储在本地驱动器上的Excel文件和存储在SharePoint Online中的Excel文件。

### 存储在本地驱动器中的Excel文件

实际上，在很多情况下，我们会收到一个充满公式的Excel文件，这些公式引用了其他工作表甚至其他工作簿中的表、列或单元格。在这些情况下，我们可能会遇到一些问题，并得到由公式生成的错误。

在其他场景中，我们会收到充满多个维度数据透视表的Excel文件。Power BI遵循由表组成的关系数据建模，而表中包含列，因此处理多维数据透视表并不总是那么直接。

Excel源的另一个常见问题是合并单元格，这会导致Power BI中出现缺失数据。您可能还会想到其他复杂的场景，使得处理Excel数据更具挑战性。本节将介绍最常见的场景之一：包含数据透视表的Excel文件。`Yearly Product Category Sales.xlsx`文件是我们在该场景中使用的示例Excel文件。下图显示了该示例文件的内容：
