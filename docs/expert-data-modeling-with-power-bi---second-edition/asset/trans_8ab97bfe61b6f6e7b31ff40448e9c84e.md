

![](img/5d2bce74fd6fceebed14fd1488bf7088_68_0.png)

### 图 2.10：Product 与 Internet Sales

下一步是创建一个度量值来计算平均产品标准成本：

```
Avg. Product Standard Costs =
AVERAGE('Internet Sales'[ProductStandardCost])
```

让我们通过以下步骤测试所创建的度量值：

-   在报表画布上放置一个矩阵视觉对象。
-   将 Product 表中的 Product Category、Product Subcategory 和 Product 列放入矩阵视觉对象的“行”区域。
-   将 Avg. Product Standard Costs 度量值放入矩阵的“值”区域。

以下截图说明了这些步骤：

### 图 2.11：按 Product Category、Product Subcategory 和 Product 可视化 Avg. Product Standard Costs

很简单吧！但让我们更深入地查看底层数据，以确保计算是正确的。在 Power BI Desktop 的左侧窗格中单击“数据视图”，然后点击 Internet Sales 表。

如下图所示，每笔交易都有一个 ProductStandardCost 值。换句话说，我们考虑了从供应商供货到商品售出的所有产品成本：

### 图 2.12：每笔交易都有 ProductStandardCost 值

![](img/5d2bce74fd6fceebed14fd1488bf7088_69_0.png)

![](img/5d2bce74fd6fceebed14fd1488bf7088_69_1.png)

产品成本通常是变动的，而且会随时间变化。因此，在我们的场景中，确认情况是否如此至关重要。如果是，那么前面的计算就是正确的。

现在，让我们让场景变得更具挑战性一些。为了确认我们对需求的理解是否正确，我们请业务方确认：他们是否会计算一件商品从开始到售出为止的所有成本。如果是这样，我们就需要将产品成本保留在 **Internet Sales** 表中。

业务方的回答是，他们需要显示商品上架前的当前产品标准成本。换句话说，每个产品关联的标准成本只有一个固定值。因此，在这个特定场景中，我们不需要在 **Internet Sales** 表的 **ProductStandardCost** 列中保留成本历史。相反，我们必须将 **ProductStandardCost** 列移动到 **Product** 表中。

因此，前面的计算是不正确的。通过将 **ProductStandardCost** 列移动到 **Product** 表，每个产品只关联一个产品标准成本。这就是数据建模如何帮助我们降低 DAX 表达式的复杂度。但为了演示虚拟表，我们再创建一个度量值并分析结果。在创建度量值之前，让我们再次回顾一下需求。

我们需要按 **Product Category**、**Product Subcategory** 和 **Product** 获取产品标准成本的平均值。每个产品只有一个当前产品标准成本。因此，我们可以创建一个虚拟表，将 **ProductKey** 列和 **ProductStandardCost** 列并排放置。请注意，这两个列来自不同的表。然而，由于这两个表之间已经存在物理关系，我们可以轻松地在单个虚拟表中让这两个列并排。

下面的度量值满足了上述需求：

```
Avg. Product Standard Costs Correct =
AVERAGEX(
    SUMMARIZE (
        'Internet Sales'
        , 'Product'[ProductKey]
        , 'Internet Sales'[ProductStandardCost]
    )
    , 'Internet Sales'[ProductStandardCost]
)
```

现在，我们可以将新度量值添加到之前放在报表画布上的矩阵视觉对象中，以同时查看两个度量值。这样，我们就可以快速看到两种计算之间的差异。总计尤其显示了两者之间的巨大差异。下图显示了这些差异：

| Product Category | Avg. Product Standard Costs | Avg. Product Standard Costs Correct |
|---|---:|---:|
| Accessories | 7.26 | 12.02 |
| Bike Racks | 44.88 | 44.88 |
| Bike Stands | 59.47 | 59.47 |
| Bottles and Cages | 2.66 | 2.99 |
| Cleaners | 2.97 | 2.97 |
| Fenders | 8.22 | 8.22 |
| Helmets | 13.09 | 13.09 |
| Hydration Packs | 20.57 | 20.57 |
| Tires and Tubes | 5.30 | 7.29 |
| Bikes | 1,105.71 | 918.41 |
| Mountain Bikes | 1,094.39 | 1,004.64 |
| Road Bikes | 1,113.45 | 881.46 |
| Touring Bikes | 1,102.87 | 885.93 |
| Clothing | 22.34 | 25.56 |
| Caps | 6.92 | 6.92 |
| Gloves | 9.16 | 9.16 |
| Jerseys | 39.97 | 40.03 |
| Shorts | 26.18 | 26.18 |
| Socks | 3.36 | 3.36 |
| Vests | 23.75 | 23.75 |
| Total | 286.07 | 679.19 |

### 图 2.13：比较为 Avg. Product Standard Costs 创建的两个度量值

让我们分析上面的度量值，看看它是如何工作的。

AVERAGEX() 函数是一个迭代器函数，它会遍历其表参数的所有行，以计算其表达式参数。这里，我们使用 SUMMARIZE() 函数创建了虚拟表。

如下图所示，我们在 **Internet Sales** 表之上创建了一个虚拟表。我们从 **Product** 表中获取按 **ProductKey** 分组的 **ProductStandardCost** 值：

![](img/5d2bce74fd6fceebed14fd1488bf7088_72_0.png)

### 图 2.14：在两个表之上创建的虚拟表

这怎么可能呢？其原理如下。

如前所述，`AVERAGEX()` 函数是一个迭代器函数。它会遍历由 `SUMMARIZE()` 函数创建的虚拟表中的每一行。`SUMMARIZE()` 函数通过两个表之间现有的关系，从 **Internet Sales** 表中获取 **ProductStandardCost** 列的值，并按 **Product** 表中相关的 **ProductKey** 实例进行分组。当我们在 DAX Studio 中运行上述计算的 `SUMMARIZE()` 部分时，可以看到 `SUMMARIZE()` 只检索了 158 行，而 **Internet Sales** 表有超过 60,000 行。原因在于 **Internet Sales** 表与 **Product** 表之间现有的关系。

下图显示了在 DAX Studio 中运行上述计算中 `SUMMARIZE()` 部分所得的结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_73_0.png)

### 图 2.15：在 DAX Studio 中运行上述计算中 SUMMARIZE() 部分所得的结果

到目前为止，一切正常。

另一个需要注意的要点是，我们在度量值内部创建了一个虚拟表。当我们将 Product Category、Product Subcategory 和 Product 列放入矩阵视觉对象时，它们会为 Avg. Product Standard Costs Correct 度量值显示正确的值。我们并没有在 Internet Sales 与 SUMMARIZE() 函数创建的虚拟表之间物理创建任何关系。为什么我们仍能得到正确的结果呢？

答案在于派生虚拟表与原始物理表之间的沿袭。虚拟表从 Product 表继承了与 Internet Sales 表的关系。换句话说，虚拟表通过 ProductKey 与 Product 表具有一对一的虚拟关系。因此，当我们将 Product Category、Product Subcategory 和 Product 列放入矩阵视觉对象时，筛选器会传播到 Product 表和 Internet Sales 表。下图说明了虚拟表如何与 Product 表和 Internet Sales 表关联：

最后一个需求，也是到目前为止最令人困惑的一个，就是：如果两个表之间存在物理关系，则参与连接的列必须具有相同的沿袭。如果表之间没有物理关系，则参与连接的列不得与数据模型中的任何物理列存在沿袭关系。因此，我们需要断开连接列的沿袭。否则，`NATURALINNERJOIN()` 函数将无法工作。我们需要使用表达式而不是实际的列名来断开沿袭。因此，我们为 `'Internet Sales'[OrderDate]` 加上 0 天，并将 `'Exchange Rates'[CurrencyKey]` 乘以 1，以断开这两列的沿袭。

最终，我们得到了所需的结果集，因此可以将 `[SalesAmount]` 乘以 `[AverageRate]`，得到以美元计的销售额。

下图并排显示了 **Internet Sales**（未考虑汇率）与 **Internet Sales USD** 两个度量值：

| Product Category | Internet Sales | Internet Sales USD |
|---|---:|---:|
| Accessories | 700,759.96 | $640,920.11 |
| All-Purpose Bike Stand | 39,591.00 | $35,628.69 |
| Bike Wash - Dissolver | 7,218.60 | $6,352.51 |
| Fender Set - Mountain | 46,619.58 | $41,974.10 |
| Hitch Rack - 4-Bike | 39,360.00 | $35,934.55 |
| HL Mountain Tire | 48,860.00 | $42,839.80 |
| HL Road Tire | 27,970.80 | $24,531.24 |
| Hydration Pack - 70 oz. | 40,307.67 | $35,307.84 |
| LL Mountain Tire | 21,541.38 | $19,092.44 |
| LL Road Tire | 22,435.56 | $21,492.01 |
| ML Mountain Tire | 34,818.39 | $31,580.66 |
| ML Road Tire | 23,140.74 | $20,554.35 |
| Mountain Bottle Cage | 20,229.75 | $18,814.51 |
| Mountain Tire Tube | 15,444.05 | $13,759.74 |
| Patch Kit/8 Patches | 7,307.39 | $6,595.31 |
| Road Bottle Cage | 15,390.88 | $13,953.77 |
| Total | 29,358,677.22 | $26,054,827.45 |

