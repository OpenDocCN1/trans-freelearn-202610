
```text
1, ABS
, 0, REL
, ORDERBY(DimProduct[Product Category], ASC,
DimProduct[Product Subcategory], ASC)
, PARTITIONBY(DimProduct[Product Category])
)
)
```

以下图像显示了结果：

| Product Category | Product Subcategory | Sales | Window, RT Sales by Product Category and Subcategory Partitioned |
|---|---|---:|---:|
| Accessories | Bike Racks | $39,360 | $39,360 |
| Accessories | Bike Stands | $39,591 | $78,951 |
| Accessories | Bottles and Cages | $56,798.19 | $135,749.19 |
| Accessories | Cleaners | $7,218.6 | $142,967.79 |
| Accessories | Fenders | $46,619.58 | $189,587.37 |
| Accessories | Helmets | $225,335.6 | $414,922.97 |
| Accessories | Hydration Packs | $40,307.67 | $455,230.64 |
| Accessories | Lights |  | $455,230.64 |
| Accessories | Locks |  | $455,230.64 |
| Accessories | Panniers |  | $455,230.64 |
| Accessories | Pumps |  | $455,230.64 |
| Accessories | Tires and Tubes | $245,529.32 | $700,759.96 |
| Bikes | Mountain Bikes | $9,952,759.5644 | $9,952,759.5644 |
| Bikes | Road Bikes | $14,520,584.0363 | $24,473,343.6007 |
| Bikes | Touring Bikes | $3,844,801.05 | $28,318,144.6507 |
| Clothing | Caps | $19,688.1 | $19,688.1 |
| Clothing | Gloves | $35,020.7 | $54,708.8 |
| Clothing | Jerseys | $172,950.68 | $227,659.48 |
| Clothing | Shorts | $71,319.81 | $298,979.29 |
| Clothing | Socks | $5,106.32 | $304,085.61 |
| Clothing | Tights |  | $304,085.61 |
| Clothing | Vests | $35,687 | $339,772.61 |
| Total |  | $29,358,677.2207 | $29,358,677.2207 |

图 15.48：使用 WINDOW() 函数按每个产品类别计算按产品类别和产品子类别划分的销售额的运行总计

我们可以使用在分区数据上类似的模式来处理类似时间智能函数的计算。例如，我们可以使用 `PARTITIONBY(DimDate[Year])` 计算年初至今。以下度量满足该需求：

```text
Window, Sales YTD =
CALCULATE(
    [Sales]
    , WINDOW(
        1, ABS
        , 0, REL
        , ORDERBY( DimDate[Year], ASC, DimDate[MonthOrder], ASC,
DimDate[Month])
        , PARTITIONBY(DimDate[Year])
    )
)
```

在前面的代码中，我们必须先在 ORDERBY() 函数中使用 DimDate[MonthOrder]，然后再使用 DimDate[Month]。以下图像显示了结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_808_0.png)

图 15.49：使用 WINDOW() 函数计算销售额 YTD

到目前为止，我们已经了解了当 from_type 参数为 ABS（绝对）和 to_type 参数为 REL（相对）时，WINDOW() 函数在单个数据分区上计算运行总计的工作方式。现在让我们更进一步，计算销售额的三个月移动平均值。由于移动平均值必须在数据上移动窗口计算，因此 from_type 和 to_type 参数都必须是 REL（相对）。运行以下度量：

```text
Window, 3M Moving Avg =
AVERAGEX(
    WINDOW(
        -2, REL
      , 0, REL
      , ORDERBY(DimDate[Year], ASC, DimDate[MonthOrder],
ASC, DimDate[Month])
    )
    , [Sales]
)
```

在前面的代码中，-2 从当前行向后移动，即 0（零）；因此，我们始终计算 3 个月的平均值。以下图像显示了结果：

![](img/5d2bce74fd6fceebed14fd1488bf7088_809_0.png)

图 15.50：使用 WINDOW() 函数计算 3 个月移动平均值

在前面的图像中，虚线显示了如何计算移动 3 个月平均值。

在本小节中，我们查看了使用 WINDOW() 函数实现的几个场景。您可以从此处下载包含所有窗口函数的第 15 章 Window Functions.pbix 示例文件：<https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de8200/Samples/Chapter%2015/Chapter%2015,%20Window%20Functions.pbix>。

Microsoft 的官方文档可以在此处找到：

[https://learn.microsoft.com/en-us/dax/window-function-dax?WT.mc_id=DP-MVP-5003466](https://learn.microsoft.com/en-us/dax/window-function-dax?WT.mc_id=DP-MVP-5003466)。

由于所有窗口函数都是新发布的，官方 Microsoft 文档网站仍有很大的改进空间。但如果你有兴趣深入研究，Jeffrey Wang 写了一篇全面解释 DAX 中窗口函数如何工作的博客；我强烈建议你阅读他的博客：

[https://pbidax.wordpress.com/2022/12/15/introducing-dax-window-functions-part-1/](https://pbidax.wordpress.com/2022/12/15/introducing-dax-window-functions-part-1/)。

虽然我们涵盖了大多数新函数，但 Microsoft 将继续扩展窗口函数和其他新函数的功功能。Microsoft 有一个专门的新函数网页，因此我建议你关注它：[https://learn.microsoft.com/en-us/dax/new-dax-functions?WT.mc_id=DP-MVP-5003466](https://learn.microsoft.com/en-us/dax/new-dax-functions?WT.mc_id=DP-MVP-5003466)。

## Summary

本章向我们介绍了新的数据建模选项、功能和 DAX 函数。我们更深入地研究了字段参数以及它们在创建更动态报表时如何变得方便。我们还通过实现一些简单示例学习了 Datamarts。我们从 SSMS 连接到底层 SQL 端点，并更深入地研究了 Datamarts 中的 RLS 实现。最后但绝对不是最不重要的，我们使用新的 DAX 函数实现了一些场景。

这是本书的最后一章；我希望你喜欢阅读它，并且学到了一些新的技术和想法来处理 Power BI 中的数据建模。有关本书中讨论主题的更多详细信息——以及未来更新——请关注我的网站：[www.biinsight.com](https://www.biinsight.com)。我在社交媒体上分享我感兴趣的技术主题；你可以在 Twitter 上关注我（[https://twitter.com/_SoheilBakhshi](https://twitter.com/_SoheilBakhshi)）和 LinkedIn（[https://www.linkedin.com/in/bakhshi/](https://www.linkedin.com/in/bakhshi/)）。

祝数据建模愉快！

## Join us on Discord!

加入 Packt Discord 服务器上的大数据与分析社区！

与 558 名其他成员一起聊天，享受免费的语音和文字聊天。

https://packt.link/ips2H

![](img/5d2bce74fd6fceebed14fd1488bf7088_811_0.png)

## Other Books You May Enjoy

如果你喜欢这本书，你可能对 Packt 的这些其他书籍感兴趣：

![](img/5d2bce74fd6fceebed14fd1488bf7088_812_0.png)

### Mastering Microsoft Power BI, Second Edition

Greg Deckler, Brett Powell

ISBN: 9781801811484

- 使用 Power Query M 语言和数据流构建高效的数据检索和转换流程
- 设计可扩展、用户友好的 DirectQuery、导入和复合数据模型
- 创建基本和高级 DAX 度量
- 添加 ArcGIS Maps 以创建有趣的数据故事
- 构建像素完美的分页报表
- 发现 Power BI 移动应用程序的功能
- 作为 Power BI 管理员管理和监控 Power BI 环境
- 通过 Power BI Premium 容量扩展 Power BI 解决方案以满足企业需求

![](img/5d2bce74fd6fceebed14fd1488bf7088_813_0.png)

### Microsoft Power BI Quick Start Guide, Third Edition

Devin Knight, Erin Ostrowsky, Mitchell Pearson, Bradley Schacht

ISBN: 9781804613498

- 使用导入、DirectQuery 和实时连接选项连接到数据源
- 使用 Power Query Editor 进行数据转换和数据清理流程，并编写 M 和 R 脚本以及数据流在云中执行相同操作
- 使用内置和自定义视觉对象设计有效的报表，以优化用户体验
- 在仪表板中实现行级和列级安全性
- 为你的组织管理 Power BI 云租户
- 使用内置 AI 功能增强 Power BI 数据转换技术
- 将 Power BI Desktop 文件部署到 Power BI Report Server

## Packt is searching for authors like you

如果你有兴趣成为 Packt 的作者，请访问 [authors.packtpub.com](https://authors.packtpub.com) 并申请。今天我们已与数千名开发人员和技术专业人士合作，帮助他们与全球技术社区分享他们的见解。你可以提交通用申请、申请我们正在招聘作者的特定热门主题，或提交你自己的想法。

## Share your thoughts

现在你已经完成了 *Expert Data Modeling with Power BI, Second Edition*，我们很想听听你的想法！如果你从 Amazon 购买了本书，请[点击此处直接前往 Amazon 评论页面](https://www.amazon.com)为本书分享你的反馈或在你购买的网站上留下评论。

你的评论对我们和技术社区非常重要，将帮助我们确保提供优质内容。
