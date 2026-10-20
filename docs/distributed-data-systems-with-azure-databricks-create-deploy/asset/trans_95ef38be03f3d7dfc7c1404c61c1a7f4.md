

正如我们在前几章中讨论的那样，在 Azure Databricks 中，数据流由 Spark 数据帧表示。我们可以通过检查数据帧的 `isStreaming` 属性是否设为 `true` 来验证该数据帧是否为数据流。为了使用结构化流，我们可以将步骤总结为读取、处理和写入，如下例所示：

1. 我们可以读取正在倾倒的数据流，例如 S3 存储桶中的数据。以下示例代码展示了如何使用 `readStream` 方法，并指定我们正在读取存储在 S3 **统一资源接口 (URI)** 中传递的 **逗号分隔值 (CSV)** 文件：
2. 现在，你可以使用通常的数据帧操作来转换数据，就像它是静态的一样。例如，如果我们想统计每小时的动作类型，我们可以通过按 `message` 和 1 小时时间窗口对数据进行分组来实现，如以下代码片段所示：
3. 最后，我们将此表写入接收器并通过使用 **Java 数据库连接 (JDBC)** 连接字符串创建到 SQL 数据库的连接来启动流式计算，如以下代码片段所示：

```
input_df=spark.readStream.csv("s3://data_stream_uri")
```

生成的 PySpark 数据帧 `input_df` 将是输入表，随着新文件到达输入目录，会不断用新行扩展。我们假设此表有两个列——`update_time` 和 `message`。

```
counts_df = input_df.groupBy($"message", window($"update_time", "1 hour")).count()
```

新的数据帧 `counts_df` 是我们的结果表，它具有 message、window 和 counts 列，并在查询启动时持续更新。请注意，即使 `input_df` 是静态表，此操作也会产生每小时计数结果。这是一种功能，可在静态数据集上安全地测试业务逻辑，并在不更改逻辑的情况下无缝迁移到流数据。

```
final_query = counts_df.writeStream.format("jdbc").start("jdbc://...")
```

返回的查询是 `StreamingQuery` 查询，它是后台运行的活动流式执行的句柄。它以后可用于管理和监控我们的流查询的执行。除了这些基础知识之外，结构化流中还可以执行更多操作。

## 映射、过滤和运行聚合

结构化流允许使用映射、过滤、选择和其他方法来转换数据。在前面的示例中，我们使用了其中一个功能：通过 Spark API 对数据进行基于时间的聚合。正如我们在前面的示例中所看到的，窗口等聚合可以表示为对数据帧的简单 group by 操作。下一节将举例说明此类操作的使用，但值得记住的是，结构化流允许我们以与普通 PySpark 数据帧相同的方式执行映射、过滤和其他数据整理方法。

在下一节中，我们将看到一些使用结构化流执行这些操作的示例。

## 事件时间上的窗口聚合

在处理具有时间维度的<|message|> 时，计算操作通常需要按特定时间段进行窗口化，在滑动窗口等情况下会相互重叠。例如，我们可能需要对向前滑动 5 分钟的 1 小时窗口执行操作。

窗口是使用 PySpark 数据帧中的 window 函数指定的。例如，通过执行以下操作将方法更改为滑动窗口方法（在结构化流模型部分之前给出的示例中显示）：

```
input_df.groupBy("message", window("update_time", "1 hour", "5 minutes")).count()
```

按照我们之前示例的结构，结果信息格式为（hour, message, count）；现在，它将具有形式（window, message, count）。到达的乱序数据将被处理，结果将相应更新，如果我们使用外部数据接收器以完整模式，则数据也会在那里更新。

## 合并流式和静态数据

正如我们之前提到的，使用结构化流意味着使用 Spark 数据帧，这很简单，允许我们将流式和静态数据组合使用。例如，如果我们有一个名为 users 的表，并且我们想将其附加到流式数据帧，我们可以执行以下操作，在其中引入一个名为 users 的表并将其附加到我们的 input_df 数据帧：

```
users_df = spark.table("users")
input_df.join(users_df, "user_id").groupBy("user_name", hour("time")).count()
```

我们也可以通过查询创建此静态数据帧，并在同一批和流式操作中运行它。

## 交互式查询

在结构化流中，我们可以直接使用 Spark JDBC 服务器将计算结果用于交互式查询。有一种使用专为少量数据设计的小内存接收器的选项，我们可以将结果写入 SQL 表，然后查询其中的数据。例如，我们可以创建一个名为 message_counts 的表，可用作稍后查询的小型数据接收器。

我们通过创建内存中表来执行此操作，如下所示：

```
message_counts.writeStream.format("memory")
.queryName("user_counts")
.outputMode("complete")
.start()
```

然后，我们使用 SQL 语句查询它，如下所示：

```
%sql
select sum(count) from message_counts where message='warning'
```

在下一节中，我们将讨论可以从中读取流数据的所有不同源。

## 使用连续流使用不同的源

数据流可以来自各种源。结构化流提供从 Delta 表、发布/订阅（pub/sub）系统（如 Azure Event Hubs）等源提取数据的支持。我们将在下一节中回顾其中一些源，以了解如何将这些数据流连接到 Azure Databricks 中运行的作业。

## 使用 Delta 表作为流源

如上一章所述，你可以使用 readStream 和 writeStream Spark 方法将结构化流与 Delta Lake 结合使用，特别关注克服处理小文件相关问题、管理批处理作业以及高效检测新文件的问题。

当 Delta 表用作数据流源时，对该表执行的所有查询都将处理该表上的信息以及流启动后到达的任何数据。

在下一个示例中，我们将按如下方式将路径和表加载到数据帧中：

```
df = spark.readStream.format("delta").load("/mnt/delta/data_events")
```

或者，我们可以通过引用 Delta 表以相同的方式执行此操作，如下所示：

```
df = spark.readStream.format("delta").table('data_events')
```

结构化流在 Delta Lake 中的一个功能是，我们可以控制每次触发器激活时要考虑的新文件的最大数量。我们可以通过将 maxFilesPerTrigger 选项设置为要考虑的文件的所需数量来控制此项。另一个要设置的选项是对每次微批处理中要处理的数据量的速率限制。这是通过使用 maxBytesPerTrigger 选项控制的，该选项控制每次触发器激活时处理的字节数。要处理的字节数将大致与此选项中指定的数量相匹配，但可能会略微超过此限制。

需要提及的一点是，如果我们尝试向用作源的 Delta 表追加列或修改其架构，结构化流将失败。如果需要引入更改，我们可以通过以下两种方式之一进行操作，如下所述：

- 在进行更改后，我们可以删除输出和检查点并从头重新启动流。
- 我们可以使用 `ignoreDeletes` 选项忽略导致数据删除的操作，或使用 `ignoreChanges` 使流不会因源表中的删除或更新而中断。

例如，假设我们有一个名为 `user_messages` 的表，我们从中流出数据，其中包含按日期分区、按日期分区的 date、`user_email` 和 message 列。如果我们需要在分区边界处删除数据，这意味着使用分区列上的 `WHERE` 子句进行删除，文件已按该列分段。因此，删除操作只会从元数据中删除这些文件。

因此，如果你只想使用分区列上的 `WHERE` 子句删除数据，可以使用 `ignoreDeletes` 选项来避免流中断，如以下代码片段所示：

```
data_events.readStream
  .format("delta").option("ignoreDeletes", "true")
  .load("/mnt/delta/user_messages")
```

但是，如果你必须基于 `user_email` 列删除数据，则需要使用以下代码：

```
data_events.readStream
  .format("delta").option("ignoreChanges", "true")
  .load("/mnt/delta/user_messages")
```

如果你使用 UPDATE 语句更新 `user_email` 变量，则包含该记录的文件将被重写。如果将 `ignoreChanges` 选项设置为 true，则该文件将被再次处理，将新记录和其他已处理的记录插入同一文件，从而在下游产生重复。因此，建议实现一些逻辑来处理这些传入的重复记录。

为了指定 Delta Lake 流源的起点而不处理整个表，我们可以使用以下选项：

- `startingVersion`：开始的 Delta Lake 版本
- `startingTimestamp`：开始的时间戳

