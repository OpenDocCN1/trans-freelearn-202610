

+   4. 在 Power BI Desktop 的右下角，单击 **Make changes to this model** 链接。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_437_0.png)

*Figure 10.1: Adding a local model*

+   5. 单击 **Add a local model** 按钮，如 *Figure 10.1* 所示。
+   6. 在下一个屏幕上，保留所有表均被选中，然后单击 **Submit** 按钮。请注意，此过程完成后，Power BI Desktop 的右下角现在显示 **Storage Mode: DirectQuery**。
+   7. 再次选择 **Get data**，然后选择 **Excel workbook**。
+   8. 导航到 **Adworks Sales Plan.xlsx** 文件并选择 **Open**。
+   9. 在 **Navigator** 对话框中，选择 **Table1**，然后单击 **Load** 按钮。
+   10. 显示 **Potential security risk** 对话框；单击 **OK** 按钮。
+   11. 在 **Data** 窗格中，将 **Table1** 重命名为 **Sales Plan**。

请注意，Power BI Desktop 的右下角现在显示 **Storage Mode: Mixed.**

## How it works

首次连接到 Power BI 语义模型时（*steps 2 and 3*），语义模型以实时模式连接。实时模式连接到 Power BI 语义模型允许使用现有语义模型创建报表，并且最初是 Power BI 语义模型可用的唯一连接方法。

微软随后发布了 **DirectQuery for Power BI datasets and Analysis Services models**。*Steps 4, 5, and 6* 使用此功能将连接从实时连接转换为 DirectQuery 连接。*Steps 7 through 10* 向数据模型添加导入模式表，使语义模型成为复合或“混合模式”模型。

语义模型中各种表之间的差异在 Model 视图中可见，如图 10.2 所示：

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_438_0.png)

Figure 10.2: Different storage modes in the composite model

在 Model 视图中，DirectQuery 表以蓝色条显示，而导入表则没有此类条。在图 10.2 中，Sales Plan 表被选中，在属性窗格下的高级设置中，存储模式设置为 Import。选择 DirectQuery 表将显示存储模式值为 DirectQuery。

Power BI 中实际上有三种存储模式：Import、DirectQuery 和 Dual。导入表被缓存到内存中，并且对语义模型的此类表的查询通过此缓存得到满足。DirectQuery 表未被缓存，并且使用这些表的语义模型查询只能通过查询源来满足。双模式表可以根据查询以缓存或非缓存方式运行。

需要注意的是，在将表设置为导入模式后，随后不可能再将该表转换为 DirectQuery 或 Dual 存储模式。

## There’s more…

复合模型还引入了聚合表的概念，如第 3 章 Leveraging Aggregation Tables 食谱中所述。

正如该食谱的 There's more... 部分所述，微软随后通过自动聚合扩展了此功能。自动聚合使用机器学习技术自动优化 DirectQuery 模型，以便语义模型的查询可以从缓存的聚合中得到满足，而不是访问源数据系统，从而提高性能。

自动聚合适用于一小部分数据源，包括：

+   - SQL Server 2019 或更高版本
+   - Azure SQL Database
+   - Azure Synapse Dedicated SQL pool
+   - Google BigQuery
+   - Snowflake
+   - Databricks
+   - Amazon Redshift

要了解如何启用自动聚合，请执行以下步骤：

+   1. 确保已安装、配置并正在运行数据网关。参见第 1 章，Creating an On-Premises Gateway。
+   2. 完成第 2 章的 Using DirectQuery 食谱并将报表发布到 Premium、PPU、Fabric 或 Embedded 工作区。
+   3. 在 Power BI Service 中，导航到工作区，单击步骤 2 中发布的语义模型旁边的省略号 (...)，然后选择 Settings。
+   4. 展开 Gateway and cloud connections 部分并确保网关连接已配置，方法是单击 Actions 下的箭头图标。将数据源映射到现有连接或选择 Add to gateway，添加连接，然后重复步骤 3 和 4 以映射数据源。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_439_0.png)

Figure 10.3: Mapping the data source to gateway connection

## Improving DAX Measure Performance

正如先前的食谱中针对特定列和关系可以优化性能一样，经常使用的 DAX 度量也可以针对性能改进。现有的 DAX 度量可能包含低效的数据访问方法，会生成额外的、不必要的查询或主要在单个 CPU 线程中执行。修改度量以更好地利用多线程存储引擎并避免或减少不必要的查询和迭代，可以在不对模型进行侵入性、结构性修改的情况下带来显著的性能改进。

在本食谱中，使用 Power BI Desktop 的 Performance analyzer 功能捕获和分析 Power BI 可视化执行的 DAX 查询。然后在 DAX Studio 中进一步分析这些查询。

### Getting ready

为准备本食谱，请执行以下步骤：

+   1. 下载并安装 DAX Studio：https://daxstudio.org/downloads/。
+   2. 下载并打开本书记述的 GitHub 仓库中的文件 Improving DAX Measure Performance Start.pbix（参见 Technical Requirements 部分）。
+   3. 在 Calculations 表中创建以下四个度量：

```
Internet Sales = SUM('Internet Sales'[SalesAmount])

Internet Sales (TOTALYTD) = TOTALYTD([Internet Sales], 'Dates'[Date])

Internet Sales (CALCULATE YTD) =
    CALCULATE(
        [Internet Sales],
        'Dates'[CalendarYear] = MAX('Dates'[CalendarYear]) &&
        'Dates'[Date] <= MAX('Dates'[Date])
    )

Internet Sales (NC YTD) =
    VAR __Date = MAX('Dates'[Date])
    VAR __Year = YEAR(__Date)
    VAR __Table =
        SUMMARIZE(
            ALL('Internet Sales'),
            'Internet Sales'[OrderDate],
            "__Year", YEAR([OrderDate]),
            "__Sales", [Internet Sales])
    VAR __Result = SUMX(FILTER(__Table, [__Year] = __Year && [OrderDate] <= __Date), [__Sales])
    RETURN
        __Result
```

3. 在 Performance analyzer 窗格中选择 TI 结果并选择 Copy query：

```
// DAX Query
DEFINE
    VAR __DS0Core =
        SUMMARIZECOLUMNS(
            ROLLUPADDSUBTOTAL('Dates'[Date], "IsGrandTotalRowTotal"),
            "Internet_Sales__TOTALYTD_", 'Calculations'[Internet Sales (TOTALYTD)]
        )
    VAR __DS0PrimaryWindowed =
        TOPN(502, __DS0Core, [IsGrandTotalRowTotal], 0, 'Dates'[Date], 1)
EVALUATE
    __DS0PrimaryWindowed
ORDER BY
    [IsGrandTotalRowTotal] DESC, 'Dates'[Date]
```

4. 在 DAX Studio 中，在顶部工具栏中启用 Server Timings、Query Plan 和 Clear on Run 选项。

5. 在 DAX Studio 跟踪运行时，单击 Run 或按 F5 键并注意 Server Timings 窗口中的性能。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_443_0.png)

Figure 10.6: DAX Studio server timings for the time intelligence measure

虽然 DAX 传统主义者多年来一直赞扬 DAX 的 CALCULATE 函数以及时间智能函数，但更现代的方法，例如使用 No CALCULATE 方法（由 Brian Julius 提出）可以显著提高 DAX 查询性能。凭借其对 VAR 语句、简单 FILTER 函数、SUMMARIZE 函数和 X 聚合器（SUMX、AVERAGEX 等）的关注，No CALCULATE 方法也往往更容易调试和学习，适合 DAX 的新手用户，因为 CALCULATE 函数的细微差别和复杂性以及 DAX 时间智能函数的黑盒性质。

## There’s more…

DAX 度量可以优化的另一个示例是此示例，其中度量必须按复杂条件进行筛选，例如日期范围。执行与本食谱相同的分析，但使用以下三个度量：

```
Last 7 Days 1 =
    VAR __Date = MAX('Dates'[Date])
    VAR __MinDate = __Date - 6
    VAR __Table = FILTER(ALL('Internet Sales'), [OrderDate] >= __MinDate && [OrderDate] <= __Date)
    VAR __Result = SUMX(__Table, [Internet Sales])
RETURN
    __Result

Last 7 Days 2 =
    VAR __Date = MAX('Dates'[Date])
    VAR __MinDate = __Date - 6
    VAR __Table1 = FILTER(ALL('Internet Sales'), [OrderDate] <= __Date)
    VAR __Table2 = FILTER(__Table1, [OrderDate] >= __MinDate )
    VAR __Result = SUMX(__Table2, [Internet Sales])
RETURN
    __Result

Last 7 Days 3 =
    VAR __Date = MAX('Dates'[Date])
    VAR __MinDate = __Date - 6
    VAR __Table1 = SUMMARIZE(ALL('Internet Sales'), [OrderDate], "Sales", [Internet Sales])
    VAR __Table2 = FILTER(__Table1, [OrderDate] <= __Date)
    VAR __Table3 = FILTER(__Table2, [OrderDate] >= __MinDate )
    VAR __Result = SUMX(__Table3, [Sales])
RETURN
    __Result
```

