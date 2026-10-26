

另一个需要考虑的因素是领先指标和滞后指标。领先指标是指一个时间序列中的值与另一个时间序列相关，但二者之间存在若干周或月的时间偏移。换句话说，一个指标可能上升，并与另一个指标的上升相对应，但第二个指标需要经过一段时间或延迟后，才会反映出第一个指标的变化。

在业务中，找到领先指标极具价值，因为它们能提供预测性指标，帮助企业根据对未来行为的预期做出决策并采取行动。要找到领先指标，相关性分析中的值必须按照适当的时间段数相互错开。通常，这意味着要在多个时间偏移下进行相关性计算，从而确定一个指标是领先还是滞后于另一个指标。

### 另请参阅

- *斜率和截距方程说明：* http://bit.ly/2tdzrgA
- *去季节化相关系数：* https://bit.ly/2SeTSsp
- *LINEST：* https://bit.ly/3znLRqL
- *LINESTX：* https://bit.ly/3zoVOEv

## 分组和分箱

Power BI 中的分组和分箱功能会创建分组列。这些列的粒度较低，可像模型中的其他列一样使用，从而简化报表可视化和自助式分析。此外，分组可在 Power BI Desktop 中管理和编辑，为语义模型所有者提供了灵活的选项，以便快速响应不断变化的需求或偏好。

本操作示例展示了如何在 Power BI Desktop 中使用分组和分箱功能。

### 准备工作

- 1. 从以下 GitHub 存储库下载 Grouping and Binning Start.pbix：https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition。
- 2. 在 Power BI Desktop 中打开 Grouping and Binning Start.pbix。
- 3. 编辑 AdventureWorks 查询，并将 Source 行更改为指向已安装 AdventureWorksDW2022 数据库的 SQL Server。
- 4. 在 **Home** 选项卡的功能区中，单击 **Close & Apply**。
- 5. 保存所做的更改。

### 如何进行分组和分箱

要实现此操作示例，请执行以下步骤：

- 1. 右键单击 Customers 表中的 `DateFirstPurchased` 列，然后选择 **New Group**。
- 2. 将 **Name** 设置为 `Customer First Purchase Calendar Quarter`，将 **Group type** 设置为 **Bin**，将 **Bin type** 设置为 **Size of bins**，将 **Bin size** 设置为 3，从下拉列表中选择 **Months**，然后单击 **OK**。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_411_0.png)

*图 9.23：FirstPurchaseDate 列的 Groups 对话框，分箱大小为 3 个月*

- 3. 创建一个 **Matrix** 可视化对象，分析这些季度客户分箱的销售额：在 Customer 表中，将新建的 `Customer First Purchase Calendar Quarter` 列放入 **Rows**；将 Dates 表中的 `CalendarYear` 列放入 **Columns**；将 `Internet Sales` 表中的 `SalesAmount` 列（求和）放入 **Values**。

- 6. 使用可视化对象右上角的省略号调整排序轴，使可视化对象按 90 Days Since Last Purchase 排序，并选择升序。

### 按 90 Days Since Last Purchase 汇总 SalesAmount

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_414_0.png)

图 9.26：按 90 Days Since Last Purchase 分组的互联网销售额簇状条形图

新的分组列（90 Days Since Last Purchase）有助于确定，历史互联网销售总额中有 1,570 万美元来自过去 180 天内有过购买的客户（0 到 90 天组为 730 万美元，90 到 180 天组为 840 万美元）。

分组实际上是语义模型中的计算列，且源系统无法看到这些列，因此最终应将分组列的逻辑迁移到源数据仓库的新列中。分组对概念验证场景和短期解决方案很有帮助，因为业务用户可以轻松实现；但与其他操作示例一样，应尽量限制在 Power BI Desktop 中进行数据转换，以确保语义模型尽可能易于管理和扩展。如果没有数据仓库可用，可以使用 M 查询转换，而不是 DAX 计算列。

### 另请参阅

- *在 Power BI Desktop 中使用分组和分箱：* https://bit.ly/3xHnMpG

## 检测和分析聚类

聚类是一种数据挖掘和机器学习技术，用于根据一个或多个度量值对某个维度中的项目进行分组（聚类）。考虑到维度中不同项目（例如产品或客户）的数量，以及用于描述这些项目的度量值数量，聚类是一种强大的数据探索方法，可用于发现通过标准报表和分析技术难以识别的关系。Power BI Desktop 内置了创建聚类的支持，并允许用户像管理、修改和使用语义模型中的其他列一样，在 Power BI 报表中管理、修改和使用聚类。

在本操作示例中，将根据销售额、订单数以及距离上次购买的天数创建客户聚类。我们会创建 DAX 度量值来支持此分析，并创建散点图可视化对象以进一步分析这些聚类。

### 准备工作

- 1. 从以下 GitHub 存储库下载 Detecting and Analyzing Clusters Start.pbix：https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition。
- 2. 在 Power BI Desktop 中打开 Detecting and Analyzing Clusters Start.pbix。
- 3. 编辑 AdventureWorks 查询，并将 Source 行更改为指向已安装 AdventureWorksDW2022 数据库的 SQL Server。
- 4. 在 Home 选项卡的功能区中单击 Close & Apply。
- 5. 保存所做的更改。

### 如何检测和分析聚类

要实现此操作示例，请执行以下步骤：

- 1. 在 Table 视图中，在 Calculations 表中创建以下度量值：

```
Today = DATE(2014,1,28)
Internet Gross Sales = SUM('Internet Sales'[SalesAmount])
Internet Sales Discounts = SUM('Internet Sales'[DiscountAmount])
Internet Net Sales = [Internet Gross Sales] - [Internet Sales Discounts]
Internet Sales Orders = DISTINCTCOUNT('Internet Sales'[SalesOrderNumber])
Last Purchase Date = LASTNONBLANK('Dates'[FullDateAlternateKey],[Internet Net Sales])
Days Since Last Purchase = DATEDIFF([Last Purchase Date],[Today],DAY)
```

- 2. 在 **Report 视图**中，使用 Customers 表中的 CustomerAlternateKey 列，以及 Internet Net Sales、Internet Sales Orders 和 Days Since Last Purchase 度量值创建表格可视化对象。

- 3. 选择表格可视化对象，单击省略号（**...**），然后选择 **Automatically find clusters**。

- 4. 在 **Clusters** 对话框中，将聚类命名为 Customers (RFM)，并输入说明 Clusters for All CustomerAlternateKey, Internet Net Sales, Internet Sales Orders, Days Since Last Purchase，然后单击 **OK** 按钮。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_416_0.png)

- 5. 在 `Calculations` 表中再创建三个度量值，以帮助描述所创建的聚类：

```
Average Customer Sales =
AVERAGEX(DISTINCT(Customers[CustomerAlternateKey]),[Internet Net Sales])
Average Customer Orders =
AVERAGEX(DISTINCT(Customers[CustomerAlternateKey]),[Internet Sales Orders])
Average Days Since Last Purchase =
AVERAGEX(DISTINCT(Customers[CustomerAlternateKey]),[Days Since Last Purchase])
```

- 6. 在新页面上创建散点图可视化对象，将 Internet Net Sales 度量值用作 X 轴，将 Internet Sales Orders 度量值用作 Y 轴，并将 Products 表中的 EnglishProductName 列用作 Values。

- 7. 选择散点图可视化对象，单击省略号（...），选择 Automatically find clusters，然后单击 OK 按钮。

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_419_0.png)

图 9.30：自动添加到散点图图例中的聚类

### 另请参阅

- 应用聚类技术：https://bit.ly/3gVwKK3

## 预测和异常检测

标准的 Power BI 报表和仪表板可视化对象非常适合对历史数据或实时数据进行描述性分析和诊断性分析，但组织最终需要预测性分析和规范性分析来帮助指导涉及未来结果的决策。Power BI Desktop 提供了时间序列预测工具，内置预测建模功能，使报表作者能够快速创建自定义预测、评估预测准确性，并构建直观的可视化对象，将实际数据或历史数据与预测结果结合起来。

本操作示例包含两个完整的预测示例。第一个示例使用自动日期层次结构，构建未来三个月的月度预测。第二个示例构建未来八周的周度预测，并评估将预测应用于近期数据时的准确性。最后，还将介绍一个使用异常检测的示例。

### 准备工作

要为本操作示例做好准备，请执行以下步骤：

- 1. 从以下 GitHub 存储库下载 Forecasting and Anomaly Detection Start.pbix：https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition。
- 2. 在 Power BI Desktop 中打开 Forecasting and Anomaly Detection Start.pbix。
- 3. 编辑 AdventureWorks 查询，并将 Source 行更改为指向已安装 AdventureWorksDW2022 数据库的 SQL Server。
- 4. 在 Home 选项卡的功能区中单击 Close & Apply。

