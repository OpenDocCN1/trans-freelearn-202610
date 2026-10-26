

3. 在“可视化”选项卡中选中视觉对象的副本后，单击“叙述”视觉对象。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_404_0.png)

图 9.21：同一数据同时以簇状柱形图和叙述视觉对象呈现

叙述可视化会分析数据并创建数据在可视化中呈现的自然语言叙述。其他专注于将解释性文本或注释与语义模型中的数据集成的自定义视觉对象包括 Add Intelligent Narratives for BI 视觉对象、Enlighten Data Story 视觉对象和 Timeline by Queryon。使用叙述视觉对象是一种简单而有效的方式来展示和突出可能 otherwise 被可视化本身所掩盖或不清楚的关键洞察。

## 另请参阅

- Scatter charts, bubble charts, and dot plot charts in Power BI: https://bit.ly/3u38toZ
- Use bookmarks in Power BI Desktop to share insights and build stories: https://bit.ly/2QK3ACS
- Create smart narrative summaries: https://bit.ly/3e57xuM

## 嵌入统计分析

超出基本聚合的统计分析通常由数据科学家和数据科学应用程序在业务智能语义模型之外实现。然而，只要有可能，利用现有的语义模型、Power BI 技能以及用于其他 Power BI 报表和仪表板的特性——例如本章前面描述的“分析”窗格——会更加高效。

在本配方中，支持线性回归模型的数据点是从现有的 Power BI 语义模型中创建的。然后通过 DAX 度量对该模型进行分析和描述，其中包括斜率、Y 截距以及残差的 Z 分数。最后，构建一个丰富的报表页面来可视化回归模型的强度和准确性并检测异常值。

### 准备工作

1. 从以下 GitHub 仓库下载 Embedding Statistical Analysis Start.pbix：https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition。
2. 在 Power BI Desktop 中打开 Embedding Statistical Analysis Start.pbix。
3. 编辑 AdventureWorks 查询，并将 Source 行更改为指向安装了 AdventureWorksDW2022 数据库的 SQL 服务器。
4. 从“开始”选项卡的功能区中单击“关闭并应用”。
5. 保存您的工作。

### 如何嵌入统计分析

要实现本配方，请执行以下步骤：

1. 在“报表”视图中，单击“建模”选项卡的功能区中的“新建表”并使用以下代码：

```
MktSalesRegression =
    FILTER(
        SUMMARIZECOLUMNS(
            'Dates'[YearMonth],
            'Dates'[YearMonthSort],
            CALCULATETABLE('Dates',
                'Dates'[FullDateAlternateKey] >= DATE(2011,1,1) &&
                'Dates'[FullDateAlternateKey] <= DATE(2013,12,31)),
            "Marketing Amount",
                VAR __MonthNum = MAX('Dates'[MonthNumberOfYear])
                VAR __Value = SUM('Internet Sales'[SalesAmount])
                RETURN SWITCH(__MonthNum,
                    1,__Value*.8,
                    2,__Value*.8,
                    3,__Value*.8,
                    4,__Value*.8,
                    5,__Value*.8,
                    6,__Value*1,
                    7,__Value*.8,
                    8,__Value*.8,
                    9,__Value*.6,
                    10,__Value*.8,
                    11,__Value*.8,
                    12,__Value*.8),
            "Internet Sales", SUM('Internet Sales'[SalesAmount])
        ),
        NOT(ISBLANK([Internet Sales]) || ISBLANK([Marketing Amount]))
    )
```

2. 为相关系数、斜率、Y 截距和决定系数（R 平方）创建度量：

```
MktSalesCorrelNum = SUMX(MktSalesRegression,
    ( (MktSalesRegression[Marketing Amount] - AVERAGE(MktSalesRegression[Marketing Amount]) ) *
      ( MktSalesRegression[Internet Sales] - AVERAGE(MktSalesRegression[Internet Sales]) )
    ) )
MktSalesCorrelDenomX = SUMX(MktSalesRegression,
    ( MktSalesRegression[Marketing Amount] - AVERAGE(MktSalesRegression[Marketing Amount]) )^2 )
MktSalesCorrelDenomY = SUMX(MktSalesRegression,
    ( MktSalesRegression[Internet Sales] - AVERAGE(MktSalesRegression[Internet Sales]) )^2 )
Mkt-Sales Correl = DIVIDE([MktSalesCorrelNum], SQRT([MktSalesCorrelDenomX]*[MktSalesCorrelDenomY]) )
Mkt-Sales R Squared = [Mkt-Sales Correl]^2
MktSalesSlope = DIVIDE([MktSalesCorrelNum],[MktSalesCorrelDenomX])
MktSales Intercept = AVERAGE(MktSalesRegression[Internet Sales]) - ( [MktSalesSlope]*AVERAGE(MktSalesRegression[Marketing Amount]) )
```

3. 单击“建模”选项卡的功能区中的“新建表”并使用以下代码：

```
Residuals =
    VAR Intercept = [MktSales Intercept]
    VAR Slope = [MktSalesSlope]
RETURN
    ADDCOLUMNS(MktSalesRegression,"Y Intercept",Intercept,"Slope",Slope,
    "Predicted Internet Sales", ([Marketing Amount]*Slope) + Intercept,
    "Residual",[Internet Sales] - (([Marketing Amount]*Slope) + Intercept))
```

4. 创建以下度量：

```
Residuals Amount = SUM(Residuals[Residual])
Residuals Average = CALCULATE(AVERAGE(Residuals[Residual]),ALL(Residuals))
Residuals Sample Std Dev = CALCULATE(STDEV.S(Residuals[Residual]),ALL(Residuals))
Residuals Z Score = DIVIDE([Residuals Amount] - [Residuals Average],[Residuals Sample Std Dev])
Regression Line Message = "Regression Line: Y= " & FORMAT([MktSalesSlope],"#,###.##") & "X" & "+" & FORMAT([MktSales Intercept],"#,###")

Last Month Internet Sales =
    VAR __LastMonth = MAX('Residuals'[YearMonthSort])
    RETURN
        LOOKUPVALUE('Residuals'[Internet Sales],[YearMonthSort],__LastMonth)
Last Month Predicted Internet Sales =
    VAR __LastMonth = MAX('Residuals'[YearMonthSort])
    RETURN
        LOOKUPVALUE('Residuals'[Predicted Internet Sales],[YearMonthSort],__LastMonth)
```

5. 从“报表”视图创建卡片视觉对象以显示“上月互联网销售额”、“上月预测互联网销售额”、“Mkt-Sales Correl”、“Mkt-Sales R Squared”和“Regression Line Message”度量。
6. 创建一个散点图可视化，使用 MktSalesRegression 表中的 Marketing Amount 列（求和）作为 X 轴，使用 MktSalesRegression 表中的 Internet Sales 列（求和）作为 Y 轴，并使用 MktSalesRegression 表中的 YearMonth 列作为值。
7. 从“分析”窗格添加趋势线。
8. 创建第二个散点图可视化，使用 Residuals 表中的 Predicted Internet Sales 列（求和）作为 X 轴，使用 Residuals 表中的 Residuals Z Score 列（求和）作为 Y 轴，并使用 Residuals 表中的 YearMonth 列作为值。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_407_0.png)

图 9.22：回归报表页面

通过这种报表设计，用户可以立即通过按 YearMonth 的 Marketing Amount 和 Internet Sales 散点图以及相关系数和 R 平方卡片的高值感知关系的强度，如图 9.22 所示。按 YearMonth 的 Predicted Internet Sales 和 Residuals Z Score 求和的散点图有助于识别变化相对较大的月份。

### 工作原理

在本示例中，我们显然人为设计了月度营销支出指标使其与互联网销售额高度相关。如果我们使用总账（FactFinance）事实表中的实际销售和营销支出数字，我们将发现完全没有相关性。

简单的单变量回归模型通常不足以准确估计 Y 值，但本配方中使用的许多概念和技术适用于更复杂的多元线性回归模型。

主要的相关性计算处理 MktSalesRegression 表中的数据。该表创建了我们希望相关联的两个指标之间对应的数据行。SUMMARIZECOLUMNS 函数按月粒度对表进行分组，而 FILTER 函数删除没有互联网销售额和营销值的行（月份）。数据在任何一个时间序列中都不应存在缺口，这对计算至关重要。CALCULATETABLE 函数将过滤后的日期表传递给 SUMMARIZECOLUMNS 以排除某些月份。Marketing Amount 和 Internet Sales 列是我们希望找到或不找到相关性的两个度量。注意，要尝试不同度量之间的相关性，只需更改 Marketing Amount 和 Internet Sales 表达式——所有其他度量和 DAX 计算都可以保持不变。

可以在源系统中开发新的 SQL 视图以满足回归表的要求，另一个替代方案是语义模型中的 M 查询可以利用现有的总账、互联网销售和日期查询。然而，像本示例这样的小型 DAX 表（有 36 行）是支持自定义或高级分析和功能的一个不错的选择。

相关系数被拆分为三个单独的中间度量（MktSalesCorrelNum、MktSalesCorrelDenomX 和 MktSalesCorrelDenomY），这些度量在 Mkt-Sales Correl 度量中被引用。相关性和其组成部分在模型中定义后，斜率（MktSalesSlope）度量可以利用相同的分子度量 MktSalesCorrelNum 以及 MktSalesCorrelDenomX 度量。最后，我们可以计算回归的 Y 截距 MktSales Intercept。

我们现在有了我们直线方程 y = mx + b 的所有组成部分，其中 m 是斜率，b 是 Y 截距。这个公式使我们能够在 Residuals 表中计算 Predicted Internet Sales。Residuals 表中的 Predicted Internet Sales 列通过引用 Marketing Amount 列（x）、斜率（MktSalesSlope）和 y 截距（MktSales Intercept）来实现直线方程（y = mx + b）。最后，Residual 列计算为从 Internet Sales 列中的观测（实际）值中减去预测销售额值。

为每个残差数据点（一个月）计算 Z 分数，以确定预测值和观测值之间的差异（或误差）相对于其他数据点是否较大。为了支持可视化，一个度量返回包含回归模型直线方程的文本字符串。此外，还创建了两个度量以显示分析中最后一个月的实际和预测互联网销售额。

本配方中的两个计算表 MktSalesRegression 和 Residuals 与模型中的其他表没有任何关系。刷新两个 DAX 表的源表（查询）也会刷新计算表。
