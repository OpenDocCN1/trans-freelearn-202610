

## COPY INTO SQL 命令

如果我们想将数据从文件复制到 Delta 表中，可以使用 COPY INTO 命令，这是一个可用于将文件加载到目录中的选项。每次运行该命令时，它都会跳过已加载的文件。让我们来看一下：

- 作为示例，如果我们想将数据加载到已创建的表中，可以执行以下操作：

```
COPY INTO target_table
FROM 'source_path'
```

- 否则，我们可以通过将前缀 delta 传递给目标路径来引用表的路径：

```
COPY INTO delta.`target_path`
FROM 'source_path'
```

COPY INTO 命令还有几个选项，我们可以在其中指定从文件位置的特定列、格式选项和复制选项。

要加载的源文件的格式应为 CSV、JSON、AVRO、ORC 或 PARQUET。FILES 指要加载的文件名列表，其长度可达 1000。它不能与 PATTERN 一起指定，PATTERN 是一个正则表达式模式，用于标识要从源目录加载的文件。

FORMAT_OPTIONS 和 COPY_OPTIONS 是可以传递给 Apache Spark 的选项，对于后者只有 'force'，如果启用，则无论文件是否已加载之前，它们都会加载手头的文件：

- 我们可以在以下命令中看到此选项的使用。在这里，我们将数据插入表中，从源路径中选择 key、index、textData 和 constant_value 列，指定 CSV 作为格式，指定所需文件的模式，并添加格式选项以便我们可以添加标题：

```
COPY INTO delta.`target_table_path`
FROM (SELECT key, index, textData, 'constant_value'
FROM 'source_path')
FILEFORMAT = CSV
PATTERN = 'data_folder/file_[a-g].csv'
FORMAT_OPTIONS('header' = 'true')
```

COPY INTO 命令在 SQL 中常用使用，也是将数据摄取到 Delta Lake 表中的受支持方式。

在下一节中，我们将讨论如何使用 Auto Loader 自动摄取数据，Auto Loader 是 Delta Lake 的一项功能，可自动将数据摄取到 Delta 表中。

## Auto Loader

在处理数据湖时，如何摄取数据的问题会非常频繁地出现。如果文件来自不同的来源，例如数据流或物联网设备，如何读取文件就会成为一个问题，这可能导致我们需要实现另一个服务，例如 Event Hub，然后从那里将其存储在数据湖中。另一种变通方法是从数据湖以批处理或按时间分区的方式读取文件，这会增加复杂性并损害性能。

Auto Loader 是 Azure Databricks 的一项功能，通过以增量的方式加载文件来帮助解决这些问题，只要文件到达存储中即可。它使用名为 cloudFiles 的结构化源来实现这一点。在指定要工作的路径时，可以将其设置为处理该目录中已存在的文件。它也可以通过目录列表或文件通知来检测到达的文件。

Auto Loader 是 Azure Databricks 的一项功能，允许我们在文件到达存储时处理文件。这是通过名为 cloudFiles 的流式源完成的，该源读取到达目录中的文件。我们也可以处理该目录中已存在的文件。我们有两种方法来检测文件到达存储目录：列出目录中的文件和使用文件通知系统。在这两种模式下，Auto Loader 都会跟踪已处理的文件。

文件通知选项基于来自 Azure Event Grid 订阅的通知来检测到达的文件，这些通知针对存储账户中的文件事件。请注意，当您选择此选项时，所有必需的服务，例如所选的存储账户和 Azure Event Grid，都会自动创建，并且需要稍后手动清理。

在这两种模式下，文件都只会被处理一次，即使文件被追加或覆盖也是如此。如果文件被追加或覆盖，就不明确正在处理的是哪个版本的 file。因此，最好仅使用 Auto Loader 来摄取不可变文件。让我们来看一下：

- 在以下 Python 命令中，我们正在读取一个流并使用 useNotifications 选项，同时设置为 true 以激活文件通知：

```
df = spark.readStream.format("cloudFiles") \
.option("cloudFiles.useNotifications","true") \
.schema(<schema>) \
.load(<input-path>)
```

- 更一般地说，我们可以使用以下 Python 命令将流读入 DataFrame：
- 然后，一旦我们读取了流，我们就可以通过指定检查点位置将其写入 delta 表：

```
df = spark.readStream.format("cloudFiles").
option(<cloudFiles-option>, <option-value>) \
.schema(<file_schema>) .load(<input-path>)
```

```
df.writeStream.format("delta").
option("checkpointLocation", <checkpoint-path>).
start(<output-path>)
```

创建新流时，会在该目录中创建检查点。如果在服务重新启动时更改了检查点，将假定创建了新流。文件通知过程更适合处理大量数据时使用。这是因为文件在到达时只处理一次，无需列出整个目录，而且多个流查询可以同时在同一个输入目录中运行。

文件通知的一个缺点是需要启动更多资源来检测存储中到达的文件。它更适合管理大量文件时使用，也是一种更可扩展的解决方案。

目录列表是 Auto Loader 的默认模式。此模式按其名称所述，列出目录以识别到达其中的文件。它不需要配置新的权限，适用于少量文件定期到达存储的情况。然而，如果文件数量迅速增加，列出整个目录很快就会变得困难。

如果需要更改为文件通知模式，您将需要重新启动流处理过程。

在下一节中，我们将学习如何通过批处理表操作来优化我们刚刚获取的数据上执行的 DDL 操作。

## 批处理表读写

在对存储在并发较高的数据库中的多个大型表执行诸如 merge 和 update 等 DDL 操作时，事务日志可能会被阻塞并导致数据仓库出现实际中断。所有 SQL 语句都是原子的，这意味着耗时较长的修改会使数据在进程执行期间一直被锁定，这对于实时数据库来说可能是一个问题。为了减少这些操作的计算负担，我们可以优化其中一些操作，使其可以在更小、更易于处理的批处理上运行，这些批处理仅在短时间内锁定资源。

让我们看看如何借助 Apache Spark API 提供的选项在 Delta Lake 中实现批处理读写。

### 创建表

我们可以通过使用 Apache Spark DataFrameWriter 或使用 CREATE TABLE 等 DDL 命令来创建 Delta Lake 表。让我们来看一下：

- Delta Lake 表是在 metastore 中创建的，表的路径也保存在那里，尽管这不是表的真实来源，表的真实来源仍然在 Delta Lake 中：

```
CREATE TABLE data (
    input_date DATE,
    record_id STRING,
    record_type STRING,
    record_data STRING)
USING DELTA
```

- 我们也可以使用 Python 执行此操作。以下命令在 metastore 中创建一个名为 "data" 的表：

```
df.write.format("delta").saveAsTable("data")
```

- 我们也可以通过识别其路径来创建此表：

```
df.write.format("delta").save("/mnt/delta/data")
```

在这里，我们可以看到 Delta 表可以通过常见的 SQL 命令创建，也可以通过简单地使用 PySpark API 创建，这对于减少代码所需的变化以便适应使用 Delta 表非常方便。

下一节将展示在表创建后读取表的多种方式。

### 读取 Delta 表

Delta 表可以通过两种不同的方式加载：

- 首先，我们可以像在任何 SQL 环境中一样指定 metastore 中的表；即使用 SELECT：

`SELECT * FROM data`

这里，data 是我们想要读取的表。

```
SELECT * FROM data
```

这里，data 是我们想要读取的表。

- 另一种加载表的方式是指定 Delta 文件的路径。我们可以通过将 delta 作为表位置的前缀来做到这一点：

`SELECT * FROM delta.'/mnt/delta_tables/data'`

这里，'/mnt/delta_tables/data' 是表的位置。

```
SELECT * FROM delta.'/mnt/delta_tables/data'
```

这里，'/mnt/delta_tables/data' 是表的位置。

- 像往常一样，我们也可以使用 Python 来执行此操作。要按名称查询表，我们可以运行以下命令：

`spark.table("data")`

这里，data 是我们想要读取的表。

```
spark.table("data")
```

这里，data 是我们想要读取的表。

- 另外，我们也可以通过引用其路径来查询同一表：

`spark.read.format("delta").load("/mnt/delta_tables/data")`

这里，'/mnt/delta_tables/data' 是表的位置。

```
spark.read.format("delta").load("/mnt/delta_tables/data")
```

这里，'/mnt/delta_tables/data' 是表的位置。

需要注意的一点是，每次我们从表中读取时，我们都在查看表的最新快照，无需使用 REFRESH 命令。您还可以指定分区语句以更高效地读取数据，正如我们在以下各节中将看到的那样。

