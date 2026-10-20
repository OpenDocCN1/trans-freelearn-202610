

## 关系和列

Skew join 优化是一种计算密集型操作，只有在需要时才应运行。这就是为什么 SKEW 的参数是列名的原因。在运行连接时，我们只需对用于执行此操作的列运行 SKEW 就很有用。

在下面的代码示例中，我们将 skew 提示应用于单个列，即 user_id：

```
SELECT /*+ SKEW('orders', user_id) */ *
FROM orders, users
WHERE user_id = our_condition
```

我们也可以将提示应用于多个列，如下所示：

```
SELECT /*+ SKEW('orders', ('user_id', 'o_region_id')) */ *
FROM orders, users
WHERE user_id = our_condition AND o_region_id= c_region_id
```

您也可以在提示中指定 skew 值。根据查询和数据，skew 值可能是已知的（例如，因为它们从不改变）或很容易查明。这样可以减少 skew join 优化的开销。否则，Delta Lake 会自动检测它们。

值也可以根据查询类型指定在提示上。传递给 SKEW 的参数越具体，优化任务的开销就越小。

在下面的代码示例中，我们在单个列上运行查询并指定单个 skew 值：

```
SELECT /*+ SKEW('orders', 'user_id', 0) */ *
FROM orders, users
WHERE user_id = our_condition
```

我们也可以传递多个 skew 值，如下所示：

```
SELECT /*+ SKEW('orders', 'user_id', (0, 1, 2)) */ *
FROM orders, users
WHERE user_id = our_condition
```

最后，我们也可以指定多个列并分别指定这些列的多个值，如下所示：

```
SELECT /*+ SKEW('orders', ('user_id', o_region_id), ((0, 1001), (1, 1002))) */ *
FROM orders, users
WHERE user_id = our_condition AND o_region_id= c_region_id
```

我们可以看到，我们可以有选择地纠正数据中的倾斜，仅在我们选择的列中应用此转换，并且这些列也符合所述条件。这样，我们可以更有效地进行优化。

## 总结

在本章中，我们讨论了如何应用 Delta Engine 功能，例如 Z-Ordering、Data Skipping 和数据缓存等，以改善我们数据的布局结构，从而整体提高查询性能。我们可以利用这些工具将文件重新组织为更少、更紧凑的文件，其中数据根据访问频率进行分布。我们还可以为数据列创建 Bloom 过滤器，以提高运行查询的速度。

在下一章中，我们将加深对如何在 Delta 表中摄取数据流的了解。

## 6
Introducing
Structured
Streaming

许多组织需要在日常流程中持续消费大量数据。因此，为了能够提取洞察并使用数据，我们需要能够在数据到达时处理这些信息，从而需要持续的数据摄取流程。这些连续应用需要克服诸如创建可靠流程以确保数据正确性等挑战，尽管可能出现流量激增、数据未及时到达、上游故障等故障，这些在处理持续传入的数据或转换数据时很常见，而这些数据没有一致的文件格式，具有不同的结构级别或需要在使用前进行聚合。

处理这些问题的最传统方式是使用定期任务执行的数据批处理，该任务处理原始流和数据并将其存储为更高效的格式以允许对数据进行查询，此过程可能会产生高延迟。

在本章中，我们将学习 Structured Streaming，这是 Apache Spark **应用程序编程接口 (API)**，它允许我们以与对静态数据表达批处理计算相同的方式对流数据进行计算。Structured Streaming 是一个容错引擎，它允许我们使用 Spark SQL 对数据执行低延迟的 **结构化查询语言 (SQL)** 类似查询的优化操作，同时确保数据一致性。

我们可以在创建 ETL 管道时使用 Structured Streaming，该管道允许我们在原始结构中传入时过滤、转换和清理数据，将数据类型格式化为更高效的存储格式，基于相关列应用高效的分区策略等等。具体来说，在本章中，我们将讨论以下主题：

- 使用 Structured Streaming API
- 在处理连续数据流时使用 Azure Databricks 中可用的不同源
- 从查询故障中恢复
- 优化流查询
- 触发流查询执行
- 在流数据帧上可视化数据
- Structured Streaming 示例

不用多说，我们将从讨论 Structured Streaming 模型开始，并讨论如何利用其架构在处理数据流时提高性能。

## 技术要求

本章需要您拥有可用于在示例上工作的 Azure Databricks 订阅，以及附加到运行中集群的笔记本。

让我们开始更详细地了解 Structured Streaming 模型，以了解在 Azure Databricks 中处理数据流有哪些可用替代方案。

## Structured Streaming 模型

Structured Streaming 模型基于一个简单但强大的前提：对数据执行的任何查询都会产生与在给定时间运行的批处理作业相同的结果。该模型通过在数据湖中、引擎内以及在与外部系统一起工作时处理到达的数据来确保一致性和可靠性。

如前几章所述，要使用 Structured Streaming，我们只需使用 Spark 数据帧和 API，说明 I/O 位置即可。

Structured Streaming 模型通过将所有新到达的数据视为附加到无界表的新的行来工作，从而使我们有机会像保留所有输入一样对数据运行批处理作业，而无需这样做。然后我们可以将流数据查询为静态表并将结果输出到数据接收器。

Structured Streaming 能够做到这一点，得益于一个名为 **Incrementalization** 的功能，该功能在我们每次运行查询时规划流执行。这也有助于确定每次新数据到达时需要保持更新的状态。

我们使用 Structured Streaming 数据帧时使用特定触发器来定义表何时更新。每次触发器被激活时，Azure Databricks 都会通过为我们的数据源中到达的每个文件创建新行来增量更新表。

最后，最后一个需要定义的部分是输出模式。这些模式允许我们确定如何以三种不同的方式增量写入外部系统，例如 **Simple Storage Service (S3)** 或数据库，概述如下：

- **Append mode**：仅追加新行。此模式不允许更改现有行。
- **Complete mode**：每次更新时，整个表都会重写到外部存储。
- **Update mode**：仅将自上次触发以来发生更改的行写入外部存储，这允许就地更新，例如 SQL 表。

Structured Streaming 输出表基于顺序流提供一致的结果，并且在与输出接收器一起工作时也确保了故障容错。例如，使用 Complete 输出模式时，乱序数据将通过更新 SQL 表中的记录来处理。我们可以建立一个阈值以避免过旧的数据更新表。

Structured Streaming 是一个容错系统，允许查询按增量方式在流数据上运行，如以下示例所述：

- 例如，如果我们想读取持续到达 S3 位置的 JavaScript Object Notation (JSON) 文件，我们编写以下代码：
```
input_df = spark.readStream.json("s3://datalogs")
```
- 我们可以对 Spark 数据帧进行操作并进行基于时间的聚合，最后写入 SQL 数据库，如下所示：
```
input_df.groupBy($"message", window("update_time", "1 hour")).count()
.writeStream.format("jdbc").start(jdbc_conn_string)
```
- 如果我们将其作为批处理操作运行，唯一的变化将是关于如何读写数据。例如，要仅读取一次数据，运行以下代码：
```
input_df = spark.read.json("s3://datalogs")
```
- 然后，我们可以使用标准 Spark DataFrame API 进行操作并写入 SQL 数据库，如以下代码片段所示：
```
inputDF.groupBy("message", window("update_time", "1 hour")).count()
.writeStream.format("jdbc").save(jdbc_conn_string)
```

在下一节中，我们将详细了解如何更详细地使用 Structured Streaming API，包括如何执行交互式查询和窗口聚合，以及我们将讨论映射和过滤操作。

## 使用 Structured Streaming API

Structured Streaming 集成到 PySpark API 中并嵌入到 Spark DataFrame API 中。它在处理流数据时提供易用性，并且在大多数情况下，从静态数据上的计算迁移到流计算只需要很少的更改。它提供执行窗口聚合和设置执行模型参数的功能。

