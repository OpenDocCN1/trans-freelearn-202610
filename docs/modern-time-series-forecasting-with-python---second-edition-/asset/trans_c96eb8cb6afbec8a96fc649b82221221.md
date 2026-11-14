
截至 pandas 1.1.0 版本，`week_of_year` 已被弃用，因为它在年底/年初会产生不一致的结果。取而代之的是，采用了 ISO 日历标准（常用于政府和商业领域），我们可以访问 ISO 日历来获取 ISO 周数。

## 切片与索引

当我们将日期列设为 DataFrame 的索引时，真正的乐趣才开始。这样做后，你可以使用 `pandas` 支持的所有花哨的切片操作，但作用于 datetime 轴上。让我们看看其中的几个：

```
# Setting the index as the datetime column
df.set_index("date", inplace=True)
# Select all data after 2010-01-04(including)
df["2010-01-04":]
# Select all data between 2010-01-04 and 2010-02-06(not including)
df["2010-01-04": "2010-02-06"]
# Select data 2010 and before
df[: "2010"]
# Select data between 2010-01 and 2010-06(both including)
df["2010-01": "2010-06"]
```

除了语义信息以及智能索引和切片外，`pandas` 还提供了用于创建和操作日期序列的工具。

## 创建日期序列与管理日期偏移量

如果你熟悉 Python 中的 `range` 和 NumPy 中的 `np.arange`，那么你就会知道它们通过提供起点和终点来帮助我们创建 `integer`/`float` 序列。`pandas` 为 datetime 提供了类似的功能 —— `pd.date_range`。该函数接受开始和结束日期，以及频率（每日、每月等），并创建中间的日期序列。让我们看看创建日期序列的几种方法：

```
# Specifying start and end dates with frequency
pd.date_range(start="2018-01-20", end="2018-01-23", freq="D").astype(str).tolist()
# Output: ['2018-01-20', '2018-01-21', '2018-01-22', '2018-01-23']
# Specifying start and number of periods to generate in the given frequency
pd.date_range(start="2018-01-20", periods=4, freq="D").astype(str).tolist()
# Output: ['2018-01-20', '2018-01-21', '2018-01-22', '2018-01-23']
# Generating a date sequence with every 2 days
pd.date_range(start="2018-01-20", periods=4, freq="2D").astype(str).tolist()
# Output: ['2018-01-20', '2018-01-22', '2018-01-24', '2018-01-26']
# Generating a date sequence every month. By default it starts with Month end
pd.date_range(start="2018-01-20", periods=4, freq="M").astype(str).tolist()
# Output: ['2018-01-31', '2018-02-28', '2018-03-31', '2018-04-30']
# Generating a date sequence every month, but month start
pd.date_range(start="2018-01-20", periods=4, freq="MS").astype(str).tolist()
# Output: ['2018-02-01', '2018-03-01', '2018-04-01', '2018-05-01']
```

我们还可以使用 `pd.TimeDelta` 向日期添加或减去天数、月数和其他值：

```
# Add four days to the date range
(pd.date_range(start="2018-01-20", end="2018-01-23", freq="D") + pd.Timedelta(4, unit="D")).astype(str).tolist()
# Output: ['2018-01-24', '2018-01-25', '2018-01-26', '2018-01-27']
# Add four weeks to the date range
(pd.date_range(start="2018-01-20", end="2018-01-23", freq="D") + pd.Timedelta(4, unit="W")).astype(str).tolist()
# Output: ['2018-02-17', '2018-02-18', '2018-02-19', '2018-02-20']
```

`pandas` 中有许多此类别名，包括 `W`、`W-MON`、`MS` 等。完整列表可参见 [https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries-offset-aliases](https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries-offset-aliases)。

在本节中，我们了解了一些可以在 datetime 索引上执行的有用功能和操作，并知道了如何操作包含 datetime 列的 DataFrame。现在，让我们回顾一些处理缺失数据的技术。

## 处理缺失数据

在处理实际的大型数据集时，你必然会遇到缺失数据。如果它不是时间序列的一部分，它可能是你收集和映射的附加信息的一部分。在我们急于求成、用平均值填充或删除这些行之前，让我们先考虑几个方面：

*   首要考虑因素应该是我们担心的缺失数据究竟是不是真的缺失。为此，我们需要考虑 `Data Generating Process` (`DGP`)（生成时间序列的过程）。例如，让我们看看当地超市的销售情况。你获得了过去 2 年的 `point-of-sale` (`POS`) 交易数据，并将其处理为时间序列。在分析数据时，你发现有几种产品在几天内没有任何交易。现在，你需要考虑的是这些缺失数据是真的缺失，还是这种缺失本身在传递某种信息。如果某天某个产品没有交易，在处理数据时它会显示为缺失数据，尽管它并非真正缺失。这告诉我们该商品当天没有销售，你应该用零来填充此类缺失数据。

*   现在，如果你发现每周日的数据都缺失 —— 即缺失具有模式性。这变得棘手，因为如何填补此类空缺取决于你打算使用的模型。如果你用零填补这些空缺，一个依赖近期过去预测未来的模型可能会被误导，尤其是对周一的预测。然而，如果你告知模型前一天是周日，模型仍然可以学会区分这种差异。

*   最后，如果你发现畅销产品某天销量为零，会怎样？这可能是由于 POS 机故障、数据录入错误或缺货等原因造成的。这类缺失值可以用一些技术进行插补。

让我们看看由澳大利亚堪培拉 ACT 政府发布、遵循 CC BY 4.0 国际许可的空气质量数据集（[https://www.data.act.gov.au/Environment/Air-Quality-Monitoring-Data/94a5-zqnn](https://www.data.act.gov.au/Environment/Air-Quality-Monitoring-Data/94a5-zqnn)），并了解如何使用 `pandas` 插补此类值（还有更复杂的技术可用，本章后面将全部介绍）。

> 从业者提示
>
> > 当使用 `read_csv` 等方法读取数据时，`pandas` 提供了一些处理缺失值的便捷方式。`pandas` 默认将许多值（如 `#N/A`、`null` 等）视为 `NaN`。我们可以使用 `na_values` 和 `keep_default_na` 参数来控制此允许的 `NaN` 值列表。

我们选择了 `Monash` 地区和 `PM2.5` 读数，并人为引入了一些缺失值，如下图所示：

![图 2.3 – 空气质量数据集中的缺失值](media/file16.jpg)

图 2.3 – 空气质量数据集中的缺失值

现在，让我们看看几种可用于填充缺失值的简单技术：

*   `Last Observation Carried Forward or Forward Fill`（最近观测值前延或前向填充）：这种插补技术采用最后一个观测值，并用它来填充所有缺失值，直到找到下一个观测值。这也称为前向填充。我们可以这样做：

```
df['pm2_5_1_hr'].ffill()
```

*   `Next Observation Carried Backward of Backward Fill`（下一个观测值后延或后向填充）：这种插补技术采用下一个观测值，并回溯用该值填充所有缺失值。这也称为后向填充。让我们看看如何在 `pandas` 中实现：

```
df['pm2_5_1_hr'].bfill()
```

*   `Mean Value Fill`（均值填充）：这种插补技术也非常简单。我们计算整个序列的平均值，发现在哪里有缺失值，就用平均值填充：

```
df['pm2_5_1_hr'].fillna(df['pm2_5_1_hr'].mean())
```

让我们绘制使用这三种技术得到的插补曲线：

![图 2.4 – 使用前向、后向和均值填充插补缺失值](media/file17.jpg)

图 2.4 – 使用前向、后向和均值填充插补缺失值
