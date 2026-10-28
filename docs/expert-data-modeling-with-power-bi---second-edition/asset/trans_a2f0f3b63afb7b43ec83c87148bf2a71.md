

![](img/5d2bce74fd6fceebed14fd1488bf7088_790_0.png)

图 15.38：DAX Debug Output 工具中第二个 EVALUATEANDLOG() 的结果

下图显示了 DAX Debug Output 工具中第三个 `EVALUATEANDLOG()` 的结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_790_1.png)

图 15.39：DAX Debug Output 工具中第三个 EVALUATEANDLOG() 的结果

包含 `EVALUATEANDLOG()` 函数的代码在 Power BI Desktop 中运行时，就像不包含 `EVALUATEANDLOG()` 函数一样，如下图所示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_791_0.png)

图 15.40：在 Power BI Desktop 中运行包含 `EVALUATEANDLOG()` 的度量值的结果

在调试完成后，从代码中移除 `EVALUATEANDLOG()` 函数至关重要。

正如您所见，在无数的调试和 DAX 性能调优场景中，`EVALUATEANDLOG()` 函数都非常有用。您可以从这里下载 `Chapter 15, EVALUATEANDLOG.pbix` 示例文件：[https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de820/Samples/Chapter%2015/Chapter%2015,%20EVALUATEANDLOG.pbix](https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de820/Samples/Chapter%2015/Chapter%2015,%20EVALUATEANDLOG.pbix).

## 窗口函数

DAX 中的**窗口函数**有助于优雅且高效地解决滚动平均值等复杂计算。使用窗口函数，我们可以将特定表的行划分为逻辑数据窗口，这些窗口由某些列的值分隔。这些数据窗口称为**分区**。要定义分区，我们在 PARTITIONBY() 函数中指定所需的列。在某些计算中，例如数据排名，我们需要按特定顺序对数据进行排序，并使用 ORDERBY() 函数。

PARTITIONBY() 和 ORDERBY() 函数都只能作为可选参数在其他窗口函数中使用。窗口函数作用于当前分区或当前行。当前行由**绝对位置**或**相对位置**确定。

绝对位置是指我们在窗口函数中明确指定位置，例如第一行；而相对位置则是必须根据当前行的位置计算得出的位置，例如当前行之前的第五行。某些窗口函数（如 INDEX() 函数）要求使用绝对位置，某些函数接受相对位置（如 OFFSET()），还有一些函数同时接受绝对位置和相对位置（如 WINDOW() 函数）。

以下窗口函数目前在 Power BI 中可用，但我们可以期待未来 DAX 会添加更多窗口函数：

- PARTITIONBY()
- ORDERBY()
- INDEX()
- OFFSET()
- WINDOW()

虽然这些函数已经发布并可供我们使用，但我们仍在了解此功能的优缺点。Power BI 团队还会不断为现有函数添加新特性和功能，并随着时间的推移引入更多窗口函数。

DAX 中所有窗口函数的一个有趣特性是可选参数可以省略。因此，如果我们不在上述某个窗口函数中指定 `PartitionBy` 参数，该函数仍然可以工作。在这种情况下，只有一个数据分区。相反，当使用 `PARTITIONBY()` 函数定义 `PartitionBy` 参数时，计算会针对每个分区重置。例如，如果我们想要按年份分区计算销售额的滚动平均值，则计算公式会在每年年初重置计算。

## PARTITIONBY()

`PARTITIONBY()` 函数中指定列的值用于在窗口函数中对表行进行分区。其语法如下：

```
PARTITIONBY ( <partitionBy_columnName>[,
partitionBy_columnName [, ...] ] )
```

例如，我们可以想象 `PARTITIONBY(DimDate[Year], DimDate[Month], DimDate[MonthOrder])` 函数会创建分区，如下图所示：

![](img/5d2bce74fd6fceebed14fd1488bf7088_794_0.png)

图 15.41：PARTITIONBY() 函数在 Power BI 中的工作方式

如前所述，`PARTITIONBY()` 函数本身并不有用，必须作为其他窗口函数的 `PartitionBy` 参数使用。窗口函数中的 `PartitionBy` 参数是可选的，因此如果省略，数据只有一个分区。如果 `PartitionBy` 参数

在此处阅读有关 DAX 查询的内容：https://learn.microsoft.com/en-us/dax/dax-queries?WT.mc_id=DP-MVP-5003466.

## INDEX()

INDEX() 函数返回已排序数据分区中绝对位置处的行。其语法如下：

```
INDEX(<position>[, <relation>][, <orderBy>][, <blanks>][, <partitionBy>])
```

以下是对上述语法中所用参数的简要说明：

- `position` 参数是唯一必需的参数，可以是基于 1 的绝对位置，也可以是返回标量值的 DAX 表达式。`position` 参数可以为正数或负数。正数（如 1）表示第一行，负数（如 -2）表示倒数第二行。
- `relation` 参数是可选的。指定后，`orderBy` 和 `PartitionBy` 参数的列必须来自 `relation` 表。如果省略，`orderBy` 和 `PartitionBy` 参数中的所有列必须来自同一个表。我们还必须显式定义 `orderBy` 参数。此外，默认值为 `orderby` 和 `PartitionBy` 参数中使用的所有列的 `ALLSELECTED()`。
- 我们使用 `ORDERBY()` 函数来定义可选的 `orderBy` 参数。如果指定了 `relation` 参数，则 `orderBy` 参数中使用的列必须来自 `relation` 表。如果省略 `orderBy` 参数，则必须显式指定 `relation` 参数，并且默认排序顺序是 `relation` 参数中未在 `PartitionBy` 参数中指定的列。
- `blanks` 是一个可选参数，用于定义排序时如何处理空白值。当前唯一接受的值是 `KEEP`，这也是默认值。
- 我们使用 `PARTITIONBY()` 函数来定义可选的 `PartitionBy` 参数。如果指定了 `relation` 参数，则 `PartitionBy` 参数中使用的列必须来自 `relation` 表。如果省略，`relation` 参数充当单个分区。

让我们看看它的实际工作原理。我们想要获取最畅销的产品。以下度量值可以实现这一点：

```
Index, Best Selling Product =
VAR __ProductSale =
    ADDCOLUMNS (
        SUMMARIZE (FactInternetSales
        , DimProduct[Product])
        , "ProductSales", [Sales]
    )
VAR __BestSeller =
    INDEX(1
        , __ProductSale
        , ORDERBY([ProductSales], DESC)
    )
RETURN
    CONCATENATEX(
        __BestSeller
        , 'DimProduct'[Product] & " : " &
        FORMAT([ProductSales], "$#,##.#")
    )
```

在上述代码中，我们首先定义了一个包含产品的销售额的虚拟表（__ProductSale）。然后我们在 INDEX() 函数的 relation 参数中使用它，该参数在 ORDERBY() 函数中按降序（DESC）排列，并且我们要求获取结果的第一行。最后，我们连接了结果列的值。您可能已经注意到，上述计算只有一个数据分区。

下图显示了在 DAX Studio 中运行上述代码的不同部分时，它是如何工作的：

![](img/5d2bce74fd6fceebed14fd1488bf7088_798_0.png)

图 15.42：INDEX() 函数在 DAX Studio 中的工作方式

下图显示了在 Power BI 报表中使用 `Index, Best Selling Product` 度量值时的情形：

![](img/5d2bce74fd6fceebed14fd1488bf7088_798_1.png)

图 15.43：在 Power BI 中使用 Index, Best Selling Product 度量值

在使用 `INDEX()` 函数的这个示例中，我们没有使用 `partition` 参数；因此，`INDEX()` 函数只认为有一个分区。让我们实现一个需要数据分区的示例。

