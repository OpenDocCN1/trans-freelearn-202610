

``` 
Table.FuzzyNestedJoin(
    FirstTable as table
  , KeyColumnofFirstTable as any
  , SecondTable as table
  , KeyColumnofSecondTable as any
  , NewColumnName as text
  , optional JoinKind as nullable JoinKind.Type
  , optional JoinOptions as nullable record
)
```

在前面的两个函数中，联接类型是可选的；如果未指定，则默认为**左外部**。我们可以使用数字枚举来指定联接类型，也可以明确指定联接类型。下表展示了各种联接类型及其对应的枚举值：

| 联接类型 | 枚举值 |
|---|---|
| JoinKind.Inner | 0 |
| JoinKind.LeftOuter | 1 |
| JoinKind.RightOuter | 2 |
| JoinKind.FullOuter | 3 |
| JoinKind.LeftAnti | 4 |
| JoinKind.RightAnti | 5 |

表 5.2：合并查询中的 Power Query 联接类型

`Table.NestedJoin()` 函数与 `Table.FuzzyNestedJoin()` 函数之间的区别在于，`Table.NestedJoin()` 函数使用键列值的相等性，而 `Table.FuzzyNestedJoin()` 函数使用键列中的文本相似度。

### 注意

合并两个查询的重要说明

合并查询支持合并表中的复合键；因此，在选择第一个表和第二个表的键列时，我们可以选择多个列。

共有六种不同的联接类型。因此，我们必须了解各种联接类型之间的区别，以及哪些联接类型适合当前需求。

在本节中，我们将重点介绍各种联接类型及其区别：

- **Inner**：根据参与联接操作的两个表中键列的匹配值联接两个查询。
- **LeftOuter**：根据第一个表中键列的所有值联接两个查询，并匹配第二个表中键列的值。
- **RightOuter**：根据第二个表中键列的所有值联接两个查询，并匹配第一个表中键列的值。
- **FullOuter**：根据参与联接操作的两个表中键列的所有值联接两个查询。
- **LeftAnti**：根据第一个表中键列的所有值联接两个查询，这些值在第二个表的键列中没有任何匹配值。
- **RightAnti**：根据第二个表中键列的所有值联接两个查询，这些值在第一个表的键列中没有任何匹配值。

让我们使用不同联接的图形表示来理解这些联接：

## 联接类型

![](img/5d2bce74fd6fceebed14fd1488bf7088_290_0.png)

图 5.44：不同的联接类型

### 注意

有关示例场景，请参阅第 1 章《Power BI 中的数据建模简介》中的“雪花模型”部分，该场景介绍了如何将产品子类别表和产品类别表反规范化到产品表中。

## 复制和引用查询

复制查询和引用查询在某些方面非常相似。当我们需要保留在原始查询中已经执行的所有转换步骤，同时又希望修改这些步骤或添加更多转换步骤时，就会复制查询。在这种情况下，我们必须改变原始查询的性质，将其转换为从业务角度来看具有不同含义的查询。但是，当我们引用查询时，我们引用的是该查询的最终结果。因此，新查询（引用查询）不会继承转换步骤。引用查询是拆分转换活动的常用方法。这是大多数**提取、转换和加载（ETL）**专家以及数据仓库专业人员首选的数据准备方式。从这个意义上说，我们可以执行以下操作：

- 我们创建连接到源系统的基础查询，这些查询对应 ETL 流程中的**提取**部分。
- 我们引用基础查询，并执行 ETL 流程中的**转换**步骤。
- 最后，我们引用转换查询来准备星型架构。这是 ETL 流程中的**加载**部分。

在上述方法中，前两个步骤中的所有查询都是转换步骤，因此我们会取消加载这些查询；我们只为**加载**部分（最后一个步骤）中的查询启用数据加载。复制和引用查询非常简单：在**查询**窗格中右键单击所需的查询，然后单击**复制**或**引用**，如下图所示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_291_0.png)

图 5.45：引用或复制查询

2. 在**转换**选项卡中单击**替换值**。
3. 根据所选列的数据类型，我们可以在**高级选项**下看到更多可用选项，例如**使用特殊字符替换**。
4. 在**要查找的值**文本框中输入 `;`。
5. 在**替换为**文本框中输入 `,`。
6. 单击“确定”：

![](img/5d2bce74fd6fceebed14fd1488bf7088_293_0.png)

图 5.46：“替换值”弹出窗口

然而，在大多数实际场景中，我们会遇到更具挑战性的情况。让我们继续了解一个具体场景。

业务部门希望在表格可视化中显示产品描述。其中有一些描述较长。业务部门希望在描述长度超过 30 个字符时，截断产品表中“描述”列的多余文本，并在描述末尾显示“...”。这三个点表示“描述”列中的文本值已被截断。我们使用第 5 章《常见数据

6. 在生成的表达式中，将 `TO BE REPLACED` 字符串替换为以下表达式：

   ```
   each if Text.Length([Description]) > 30 then
   [Description] else ""
   ```

7. 将 `REPLACING WITH` 替换为以下表达式：

   ```
   each Text.Start([Description], 30) & "..."
   ```

8. 单击“提交”（✔）按钮。

完整表达式应如下所示：

```
= Table.ReplaceValue(#"Renamed Columns", each if Text.Length([Description]) > 30 then [Description] else "",each Text.Start([Description], 30) & "...",Replacer.ReplaceValue,{"Description"})
```

前面的代码块将返回以下输出：

![](img/5d2bce74fd6fceebed14fd1488bf7088_295_0.png)

图 5.48：截断“描述”列中长度超过 30 个字符的值

应用更改，并将文件另存为新文件：Chapter 5, Common Data Preparation Steps, Replaced Values.pbix。

替换值是一种广泛使用的转换步骤，具有许多应用场景，例如根据另一列、前一个步骤中的值，甚至另一个查询中的值来替换值。因此，如何高效地替换值取决于开发人员的创造力以及具体问题的性质。

## 从文本中提取数字

另一种常见的数据准备步骤是从文本值中提取数字。例如，我们可能需要从地址中提取门牌号或邮政编码。其他示例包括从销售订单号中提取数字部分，或清理包含拼写错误的全名，例如某些姓名中混入了数字。我们将继续使用上一节中的示例文件 Chapter 5, Common Data Preparation Steps, Replaced Values.pbix。在本场景中，我们希望向 Customer 表添加两个新列，如下所示：

- 从 **AddressLine1** 中提取**门牌号**作为新列。
- 将地址的其余部分，即**街道名称**，提取为新列。

如下图所示，AddressLine1 列中的门牌号出现在地址的不同位置；因此，按照从数字过渡到非数字的方式进行拆分并不可行：

![](img/5d2bce74fd6fceebed14fd1488bf7088_296_0.png)

图 5.49：门牌号出现在 AddressLine1 的不同位置

为了实现目标，我们需要从文本中提取数字。为此，我们使用 `Text.Select(Text as nullable text, SelectChars as any)` 函数。在 Power Query 编辑器中按照以下步骤操作：

1. 选择 Customer 查询。
2. 在功能区的“添加列”选项卡中单击“自定义列”按钮。
3. 在“新列名”中输入 `Flat Number`。
4. 输入以下表达式：

   ```
   Text.Select([AddressLine1], {"0".."9"})
   ```

5. 单击“确定”。

以下屏幕截图展示了上述步骤：

![](img/5d2bce74fd6fceebed14fd1488bf7088_297_0.png)

图 5.50：从地址中提取门牌号作为新列

