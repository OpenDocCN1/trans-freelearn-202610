

## 数据分析表达式与数据建模

上一章讨论了 Power BI 具有不同的层：数据准备、数据建模和数据可视化。本章讨论数据分析表达式（DAX）及其与数据建模的关系。虽然数据建模和 DAX 相互关联，以至于你无法想象其中一个没有另一个，但我们本书的目标并不是只关注 DAX。数据建模涵盖了更广泛的概念，而 DAX 是开发人员必须在数据模型中实现业务逻辑时使用的表达式语言。我们假设你对 DAX 具有中级知识水平；因此，本书不涵盖基础 DAX 概念，例如 DAX 语法。相反，我们专注于更高级的概念并结合实际场景。

本章涵盖以下主题：

- 理解虚拟表
- 理解虚拟表中的关系
- 时间智能

### 理解虚拟表

DAX 中虚拟表的概念有些令人困惑且被误解，但它是 DAX 中最强大和最重要的概念之一。当我们谈论虚拟表时，我们指的是使用某些 DAX 函数或构造函数构建的内存中表。虚拟表中的数据要么源自数据模型中的数据，要么是我们为特定目的构造的数据。

![](img/5d2bce74fd6fceebed14fd1488bf7088_59_0.png)

图 2.3：计算表和虚拟表

我们首先使用前面的代码创建了一个带有 **Value** 列的虚拟表。然后，我们将 **Value** 列重命名为 **ID**，最后，我们用结果填充了一个计算表。

在前面的场景中，我们使用虚拟表创建了一个计算表。在下面的场景中，我们将演示虚拟表用于度量的用法。

让我们进一步进行一个更复杂的场景。

从以下 URL 下载 `Chapter 2, Virtual Tables 02, Adventure Works, Internet Sales.pbix` 样本：

https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/main/Samples/Chapter%2002/Chapter%202%2C%20Virtual%20Tables%2002%2C%20Adventure%20Works%2C%20Internet%20Sales.pbix

在 **Internet Sales** 表中创建一个度量，以计算产品列表价格高于 $1,000 时的订购数量。将该度量命名为 **Orders with List Price Bigger than or Equal to $1,000**：

**Note**: GOOD PRACTICE

计算为客户购买的产品列表价格大于 $1,000 且数量超过 4 件的订单数量。将该度量命名为 **Order Qty for Customers Buying More than 4 Products with List Price Bigger Than $1,000.**

为了解决此场景，我们需要创建一个客户虚拟表，这些客户对价格超过 $1,000 的产品下了超过 4 个订单。以下代码将处理此操作：

```
Order Qty for Customers Buying More than 4 Products with List Price Bigger Than $1,000 =
SUMX(
    FILTER(
        VALUES(Customer[CustomerKey]) //Virtual table
        , [Orders with List Price Bigger than or Equal to $1,000] > 4
    )
    , [Orders with List Price Bigger than or Equal to $1,000]
)
```

分析前面的计算有助于我们更好地理解虚拟表的强大功能：

- 此处的虚拟表只有一个列，即 **CustomerKey**。请记住，此列仅在当前计算中可访问。
- 然后，我们使用 `FILTER()` 函数过滤 `VALUES()` 的结果，仅显示对列表价格超过 $1,000 的产品下了超过 4 个订单的客户键。
- 最后，我们对 `FILTER()` 的结果求和这些数量。

我们现在可以在报表页面上放置一个表格视觉对象，然后从 **Customer** 表中选择 **First Name** 和 **Last Name** 以及我们刚刚创建的新度量。以下图显示了订购了超过 4 件价格超过 $1,000 的商品的客户：

| First Name | Last Name | Order Qty for Customers Buying More than 4 Products with List Price Bigger Than $1,000 |
|---|---|---|
| Adriana | Gonzalez | 5 |
| Brad | She | 5 |
| Brandi | Gill | 5 |
| Francisco | Sara | 5 |
| Janet | Munoz | 5 |
| Kaitlyn | Henderson | 5 |
| Margaret | He | 5 |
| Maurice | Shan | 6 |
| Nichole | Nara | 5 |
| Randall | Dominguez | 5 |
| Rosa | Hu | 5 |
| Total |  | 56 |

图 2.6：按客户的 Order Qty for Customers Buying More than 4 Products with List Price Bigger Than $1,000 表

如前所述，我们可以使用所有返回表值的 DAX 函数来创建虚拟表。但是，下表中列出的函数是最常用的：

| ADDCOLUMNS() | CALENDARAUTO() | FILTERS() | SELECTCOLUMNS() | VALUES() |
|---|---|---|---|---|
| ADDMISSINGITEMS() | CALCULATETABLE() | GENERATESERIES() | SUMMARIZE() | Table Constructor {} |
| ALL() | CROSSJOIN() | INTERSECT() | SUMMARIZECOLUMNS() |  |
| ALLEXCEPT() | DATATABLE() | NATURALINNERJOIN() | TOPN() |  |
| ALLSELECTED() | DISTINCT() | NATURALLEFTOUTERJOIN() | TREATAS() |  |
| CALENDAR() | EXCEPT() | RELATEDTABLE() | UNION() |  |

图 2.7：常用函数

虚拟表仅在内存中可用。此外，它们在 Power BI Desktop 的模型视图中不可见。因此，它们有点难以理解。然而，仍然有一些方法可以测试虚拟表并直观地看到结果，我们将在下一节中介绍。

### 可视化显示虚拟表的结果

在本节中，我们解释如何查看虚拟表的结果。观察虚拟表的输出总是有益的，而不仅仅是。

![](img/5d2bce74fd6fceebed14fd1488bf7088_65_0.png)

图 2.8：在 Power BI Desktop 计算表中可视化显示虚拟表

### 使用 DAX Studio

DAX Studio 是由 SQLBI 的出色团队创建的最受欢迎的第三方工具之一，可免费下载。你可以从工具的官方网站获取它（https://daxstudio.org/）。你可以使用 DAX Studio 轻松查看虚拟表的结果。首先，你需要在从 DAX Studio 连接之前打开你的 Power BI 文件（*.pbix）。打开 DAX Studio 并按照以下步骤操作：

1. 点击 PBI/SSDT Model。
2. 选择所需的 Power BI Desktop 实例。
3. 输入 EVALUATE 语句，然后将计算的虚拟表部分复制并粘贴到新行上：

```
FILTER('Product' //Virtual table start
    , 'Product'[List Price]>=1000
) //Virtual table end
```

- 假设虚拟表是从数据模型中现有的物理表派生的。虚拟表与原始物理表之间存在关系，这就是所谓的 **lineage**。
- 在某些情况下，我们在一次计算中创建了多个虚拟表。然后，我们在程序上在这些虚拟表之间创建关系。在某些其他情况下，我们可能需要用新的关系替换现有关系。这种关系称为 **virtual relationship**。

无论哪种方式，该关系都不是数据模型中的物理关系。它是在计算本身中创建的，并在计算运行期间驻留在内存中。因此，理解虚拟关系至关重要。所以，让我们通过一个实际场景来了解。

企业要求你按 **Product Category**、**Product Subcategory** 和 **Product** 计算平均产品标准成本。

为了解决此场景，请打开样本文件：`Chapter 2, Virtual Tables 03, Avg Product Standard Cost.pbix`。查看数据模型。它非常简单。它仅由两个表组成，**Product** 和 **Internet Sales**。正如你所见，我们可以在度量中使用来自 **Internet Sales** 表的 **ProductStandardCost** 列。以下图显示了数据模型：

