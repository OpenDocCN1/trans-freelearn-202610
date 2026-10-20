

我们也可以定义一个特定的时间间隔，利用 `processingTime` 触发器选项。下面的代码示例中，我们设置了一个 10 秒的微批处理时间间隔：

```
streaming_df.writeStream \
    .format("console") \
    .trigger(processingTime='10 seconds').start()
```

如前所述，我们可以设置一次性触发器，仅处理一次所有新数据，如以下代码片段所示：

```
streaming_df.writeStream \
    .format("console") \
    .trigger(once=True).start()
```

最后，我们可以设置一个连续触发器，该触发器将具有 2 秒的检查点时间间隔以保存数据，以便在发生故障时恢复。此代码在以下片段中说明：

```
streaming_df.writeStream
    .format("console")
    .trigger(continuous='1 second').start()
```

Structured Streaming 中的触发器允许我们对查询在处理在定义的时间间隔内到达的新数据时的行为进行精细控制。

在本章的最后一节中，我们将深入探讨 Azure Databricks 中可用于可视化数据的工具。

## 在流式数据帧上可视化数据

在使用 Structured Streaming 数据帧处理数据流时，我们可以使用 `display` 函数可视化实时数据。此函数与其他可视化函数不同，因为它允许我们指定诸如 `processingTime` 和 `checkpointLocation` 等选项，这是由于数据的实时性质。这些选项用于管理我们可视化的确切时间点，并且在生产环境中应始终设置，以便确切知道我们看到的数据的状态。

在下面的代码示例中，我们首先定义一个 Structured Streaming 数据帧，然后使用 `display` 函数每 5 秒处理时间显示一次数据的状态，在特定的检查点位置：

```
streaming_df = spark.readStream.format("rate").load()
display(streaming_df.groupBy().count(), processingTime = "5 seconds", checkpointLocation = "<checkpoint-path>")
```

具体来说，此函数的可用参数如下：

- `streamName` 参数是特定流查询，即我们想要可视化的查询。
- `processingTime` 参数定义了流查询执行的频率时间间隔。如果未指定，Azure Databricks 将在上一个进程完成后检查新数据的可用性——这种行为可能导致不必要的成本增加。因此，建议在可能的情况下显式设置此选项。

`checkpointLocation` 参数是写入检查点数据的路径。如果未指定，位置将是临时的。建议设置此参数，以便在发生任何故障时流可以从中断处重新启动。

Azure Databricks `display` 函数支持多种绘图类型。我们可以通过在执行函数的单元格左上角从下拉菜单中选择所需的绘图类型来选择不同类型的绘图，如以下屏幕截图所示：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_202_0.png)

图 6.1 – `display` 函数的可用选项

我们有创建不同类型绘图的选项，例如条形图、散点图和线形图，并具有为这些可视化设置选项的额外功能。如果我们点击 Plot Options 菜单，将会弹出一个窗口，我们可以根据所选类型自定义绘图，如以下屏幕截图所示：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_203_0.png)

图 6.2 – 自定义绘图的可用选项

`display` 选项在可视化数据时提供了易用性，尤其是在流查询方面。提供的选项允许我们轻松拖放创建每种类型绘图所需的变量，选择执行的聚合类型并修改其设计的多个方面。

下一节将包含一个示例，我们将在其中模拟数据流并使用 Structured Streaming 运行流查询。

## Structured Streaming 示例

在本示例中，我们将研究如何利用我们在前几节中获得的有关 Structured Streaming 的知识。我们将通过使用示例数据集之一来模拟传入的数据流，其中包含小的 JSON 文件，在真实场景中，这些文件可能是我们想要处理的数据流。我们将使用这些文件来计算时间戳操作的计数和窗口计数等指标。让我们先看看 structured-streaming 示例数据集的内容，如下所示：

```
%fs ls /databricks-datasets/structured-streaming/events/
```

你会发现该目录中有大约 50 个 JSON 文件。你可以在以下屏幕截图中看到其中一些：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_204_0.png)

图 6.3 – structured-streaming 数据集的 JSON 文件

我们可以使用 `fs head` 选项查看其中一个 JSON 文件的内容，如下所示：

```
%fs head /databricks-datasets/structured-streaming/events/file-0.json
```

我们可以看到文件中的每一行都有两个字段，即 `time` 和 `action`，因此下一步将是尝试使用 Structured Streaming 交互式分析这些文件，如下所示：

- 1. 我们将采取的第一步是定义数据的模式并将其存储在 JSON 文件中，以便以后用于创建流数据帧。此代码可在以下片段中看到：

```
from pyspark.sql.types import *
input_path = "/databricks-datasets/structured-streaming/events/"
```

- 2. 由于我们已经知道数据的结构，因此不需要推断模式，所以我们将使用 PySpark SQL 类型来定义未来流数据帧的模式，如以下代码片段所示：

```
json_schema = StructType([StructField("time",
    TimestampType(),
    True),
    StructField("action",
    StringType(),
    True)])
```

- 3. 现在，我们可以使用之前定义的模式构建静态数据帧并加载 JSON 文件中的数据，如下所示：

```
static_df = (
  spark
    .read
    .schema(json_schema)
    .json(input_path)
)
```

- 4. 最后，我们可以使用上一节中提到的 `display` 函数可视化数据帧，如下所示：

```
display(static_df)
```

输出如下：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_206_0.png)

图 6.4 – 流数据帧

5. 我们现在准备运行计算，以通过基于 1 小时窗口聚合对数据进行分组来了解开单和关单的数量。我们将使用星号 (*) 从 PySpark SQL 函数导入所有内容，以获取窗口函数等，如以下代码片段所示：

```
from pyspark.sql.functions import *
static_count_df = (
    static_df
    .groupBy(
        static_df.action,
        window(static_df.time, "1 hour"))
    .count()
)
static_count_df.cache()
```

6. 接下来，我们将数据帧注册为名为 `data_counts` 的临时视图，如以下代码片段所示：

```
static_count_df.createOrReplaceTempView("data_counts")
```

7. 最后，我们可以直接使用 SQL 命令查询表。例如，要计算所有小时的总计数，我们可以运行以下 SQL 命令：

```
%sql
select action, sum(count) as total_count from data_counts
group by action
```

输出如下：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_207_0.png)

图 6.5 – 对流数据帧的 SQL 查询

8. 我们也可以使用窗口计数按时间和操作创建时间线，如下所示：

```
%sql
select action, date_format(window.end, "MMM-dd HH:mm") as time, count from data_counts order by time, action
```

