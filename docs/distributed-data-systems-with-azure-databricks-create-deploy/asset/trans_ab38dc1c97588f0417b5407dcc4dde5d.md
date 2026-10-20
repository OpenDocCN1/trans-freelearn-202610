

## 通过分区加速查询

你可以通过在处理过程中对列进行分区来加速查询和 DML 操作。我们也可以将此应用于 Delta 表，通过在我们想要用作分区的列上使用 PARTITIONED BY 命令，在许多情况下该列是日期列。如果我们将 LOCATION 指定为 Databricks 文件系统的路径，这将在 metastore 中创建表。如果表不在 metastore 中，删除表不会删除 location 中指定的文件。让我们来看一下：

- 在这里，我们在 Azure Databricks metastore 中创建了一个表，并按 input_date 列对其进行分区：
```
CREATE TABLE data (
    input_date DATE,
    record_id STRING,
    record_type STRING,
    record_data STRING)
PARTITIONED BY (date)
LOCATION '/mnt/delta_files/data'
```

同样，我们也可以使用 Python 来执行此操作。

- 以下命令执行相同的操作，在 metastore 中创建表：
```
df.write.format("delta").partitionBy("date").saveAsTable("data")
```

- 我们也可以通过指定表的路径来执行相同的操作：
```
df.write.format("delta").partitionBy("date").save("/mnt/delta_files/data")
```

如果表是在已托管 Delta 表路径的位置创建的，这可能导致以下情况。如果仅指定表名和位置，它将被理解为我们只是导入了数据，并且创建的表将继承该路径中已存在的表的模式、分区和属性：

- 以下示例显示了如果 `/mnt/delta/data` 已托管一个表会发生什么情况：

```
CREATE TABLE data
USING DELTA
LOCATION '/mnt/delta/data'
```

这也展示了 Delta Lake 的自动模式推断是如何工作的。

如果我们指定了模式和分区，Delta 表将检查输入数据是否与现有数据匹配。如果不匹配，将引发异常。

## 查询表的过去状态

正如我们之前提到的，查询表的过去状态是 Delta Lake 的一项功能，在处理数据被错误操作、恢复已删除记录、审计以及受监管行业时非常有用。它还允许我们编写使用不同时间状态的查询，得益于快照隔离。

本节描述了如何使用这些方法查询表的旧版本。

### 使用时间旅行查询表

要查询表的先前状态，我们可以执行以下操作：

- 我们可以使用 AS OF 命令，使用 SQL 语法：

```
SELECT * FROM table_name TIMESTAMP AS OF timestamp_expression
```

table_name 参数是表的名称。我们还可以传递任何可以转换为时间戳或日期字符串的表达式，以获取我们所需数据的确切快照。

- 另一种查询表过去状态的方法是使用 VERSION AS OF 命令。为此，我们需要传递一个特定的版本，该版本可以从 DESCRIBE HISTORY 的输出中获得。两种方法的完整示例如下：

```
SELECT * FROM table_name VERSION AS OF table_version
```

这里，table_version 是可以通过对我们想要查询的表使用 DESCRIBE HISTORY 获得的值。

- 如果我们想在 Azure Databricks 笔记本中的 Python 代码块中使用此功能，我们可以将 DataFrameReader 选项设置为表的一个特定时间点或版本。为此，我们需要传递 timestampAsOf 选项：

```
df = spark.read.format("delta").option("timestampAsOf", timestamp_string).load("/mnt/delta/data")
```

这里，'/mnt/delta/data' 是表的位置。

访问特定版本也适用。我们可以通过将 versionAsOf 传递给 DataFrameReader 选项来做到这一点：

```
df = spark.read.format("delta").option("versionAsOf", version).load("/mnt/delta/data")
```

使用时间旅行从表的早期版本中选择数据非常容易。用户可以通过使用版本历史编号（如下面的代码所示，选择 VERSION AS OF 0 表数据时），或通过时间戳来查看表的历史并查看该点的数据是什么样的：

- 我们可以通过读取数据的 HISTORY 属性使用 DESCRIBE 来学习如何获取可用版本并查看表的历史：

```
DESCRIBE HISTORY data
```

- 如果我们想访问表的快照以查看数据在某个时间点是什么样子的，我们可以使用 TIMESTAMP 结合 AS OF：

```
SELECT * FROM loan_by_state_delta TIMESTAMP AS OF '2020-07-14 16:30:00'
```

Delta Lake 保证在查询表的先前状态时，所有转换都可以被跟踪、重现，并且保持可逆状态。这在处理受 GDPR 或 CCPA 法规约束的数据时尤其有用，其中需要提供删除证明。我们还可以使用此方法可视化在此表上进行的所有 DML 操作，例如更新和合并。

时间旅行是对可能的人为错误或数据处理不当的保护，并提供轻松调试错误的途径，得益于其事务日志。这使我们能够回到过去，找到创建错误的机制，对该问题采取行动，并将数据恢复到准确状态。

如果我们考虑到 Delta 表会自动更新自己，在 Azure Databricks 中执行作业时，将其设置为最新版本是良好的实践。否则，如果数据在执行过程中被更新，整个过程可能会产生不同的结果。

- 我们可以通过使用以下 Python 命令执行 SQL 查询来获取最新版本：

```
latest_version = spark.sql("SELECT max(version).\
FROM (DESCRIBE HISTORY delta.`/mnt/delta/events`)").collect()
df = spark.read.format("delta").option("versionAsOf", latest_version[0][0]).load("/mnt/delta/events")
```

访问 Delta 表的特定历史点或版本[content]

## 通过分区加速查询

你可以通过在处理过程中对列进行分区来加速查询和 DML 操作。我们也可以将此应用于 Delta 表，通过在我们想要用作分区的列上使用 PARTITIONED BY 命令，在许多情况下该列是日期列。如果我们将 LOCATION 指定为 Databricks 文件系统的路径，这将在 metastore 中创建表。如果表不在 metastore 中，删除表不会删除 location 中指定的文件。让我们来看一下：

- 在这里，我们在 Azure Databricks metastore 中创建了一个表，并按 input_date 列对其进行分区：
```
CREATE TABLE data (
    input_date DATE,
    record_id STRING,
    record_type STRING,
    record_data STRING)
PARTITIONED BY (date)
LOCATION '/mnt/delta_files/data'
```

同样，我们也可以使用 Python 来执行此操作。

- 以下命令执行相同的操作，在 metastore 中创建表：
```
df.write.format("delta").partitionBy("date").saveAsTable("data")
```

- 我们也可以通过指定表的路径来执行相同的操作：
```
df.write.format("delta").partitionBy("date").save("/mnt/delta_files/data")
```

如果表是在已托管 Delta 表路径的位置创建的，这可能导致以下情况。如果仅指定表名和位置，它将被理解为我们只是导入了数据，并且创建的表将继承该路径中已存在的表的模式、分区和属性：

- 以下示例显示了如果 `/mnt/delta/data` 已托管一个表会发生什么情况：

```
CREATE TABLE data
USING DELTA
LOCATION '/mnt/delta/data'
```

这也展示了 Delta Lake 的自动模式推断是如何工作的。

如果我们指定了模式和分区，Delta 表将检查输入数据是否与现有数据匹配。如果不匹配，将引发异常。

## 查询表的过去状态

正如我们之前提到的，查询表的过去状态是 Delta Lakes 的一项功能，在处理数据被错误操作、恢复已删除记录、审计以及受监管行业时非常有用。它还允许我们编写使用不同时间状态的查询，得益于快照隔离。

本节描述了如何使用这些方法查询表的旧版本。

### 使用时间旅行查询表

要查询表的先前状态，我们可以执行以下操作：

- 我们可以使用 AS OF 命令，使用 SQL 语法：

```
SELECT * FROM table_name TIMESTAMP AS OF timestamp_expression
```

table_name 参数是表的名称。我们还可以传递任何可以转换为时间戳或日期字符串的表达式，以获取我们所需数据的确切快照。

- 另一种查询表过去状态的方法是使用 VERSION AS OF 命令。为此，我们需要传递一个特定的版本，该版本可以从 DESCRIBE HISTORY 的输出中获得。两种方法的完整示例如下：

```
SELECT * FROM table_name VERSION AS OF table_version
```

这里，table_version 是可以通过使用 DESCRIBE HISTORY 在我们想要查询的表上获得的值。

- 如果我们想在 Azure Databricks 笔记本中的 Python 代码块中使用此功能，我们可以将 DataFrameReader 选项设置为表的一个特定时间点或版本。为此，我们需要传递 timestampAsOf 选项：

```
df = spark.read.format("delta").option("timestampAsOf", timestamp_string).load("/mnt/delta/data")
```

这里，'/mnt/delta/data' 是表的位置。

同理适用于访问特定版本。我们可以通过将 versionAsOf 传递给 DataFrameReader 选项来做到这一点：

```
df = spark.read.format("delta").option("versionAsOf", version).load("/mnt/delta/data")
```

使用时间旅行从表的早期版本中选择数据非常容易。用户可以通过使用版本历史编号（如下面的代码所示，选择 VERSION AS OF 0 表数据时），或通过时间戳来查看表的历史并查看该点的数据是什么样的：

- 我们可以通过读取数据的 HISTORY 属性使用 DESCRIBE 来学习如何获取可用版本并查看表的历史：

```
DESCRIBE HISTORY data
```

- 如果我们想访问表的快照以查看数据在某个时间点是什么样子的，我们可以使用 TIMESTAMP 结合 AS OF：

```
SELECT * FROM loan_by_state_delta TIMESTAMP AS OF '2020-07-14 16:30:00'
```

Delta Lake 保证在查询表的先前状态时，所有转换都可以被跟踪、重现，并且保持可逆状态。这在处理受 GDPR 或 CCPA 法规约束的数据时尤其有用，其中需要提供删除证明。我们还可以使用此方法可视化在此表上进行的所有 DML 操作，例如更新和合并。

时间旅行是对可能的人为错误或数据处理不当的保护，并提供轻松调试错误的途径，得益于其事务日志。这使我们能够回到过去，找到创建错误的机制，对该问题采取行动，并将数据恢复到准确状态。

如果我们考虑到 Delta 表会自动更新自己，在 Azure Databricks 中执行作业时，将其设置为最新版本是良好的实践。否则，整个过程可能会产生不同的结果，如果数据在执行过程中被更新。

- 我们可以通过使用以下 Python 命令执行 SQL 查询来获取最新版本：

```
latest_version = spark.sql("SELECT max(version).\
FROM (DESCRIBE HISTORY delta.`/mnt/delta/events`)").collect()
df = spark.read.format("delta").option("versionAsOf", latest_version[0][0]).load("/mnt/delta/events")
```

访问 Delta 表的特定历史点或版本的另一种方法是使用 @ 语法。这是在 SQL 和 Python 执行期间指向表特定状态的一种方式。

- 一个示例是以下 SQL 命令，其中我们使用时间戳选择表在某个时间点的所有行：

```
SELECT * FROM events@20200101000000000
```

- 我们也可以以相同的方式指定版本，只需在版本前添加 v 作为前缀：

```
SELECT * FROM events@v114
```

同理适用于 Python，其中 @ 语法直接传递给我们想要加载的表的路径。

- 以下是如何访问特定时间戳的示例：

```
spark.read.format("delta").load("/mnt/delta/events@20200101000000000")
```

- 相同的方法适用于版本：

```
spark.read.format("delta").load("/mnt/delta/events@v114")
```

此时，你可能会想知道你能回溯多远。默认情况下是过去 30 天，只要你没有在想要访问的 Delta 表上运行 VACUUM 命令；否则，你将无法回溯超过过去 7 天。

- 这种行为可以通过设置 delta.logRetentionDuration 和 delta.deletedFileRetentionDuration 属性来配置。例如，我们可以通过运行以下命令将 delta.deletedFileRetentionDuration 设置为 30 天：

```
delta.deletedFileRetentionDuration = "interval 30 days".
```

这样，我们就可以轻松地为任何 delta 表的可用历史建立保留策略。

利用时间旅行可以在我们对数据执行了错误操作并希望恢复先前状态时提供一个故障保护。在下一节中，我们将学习如何在 Delta Lake 中存储和处理数据时利用此功能。

## 处理过去和现在的数据

能够访问表的先前时间状态在我们想要恢复已被删除或移除的信息时尤其有用，或者当我们想要监控数据随时间增长时。让我们来看一下：

- 以下是此功能的一个示例。在这里，我们通过传递所需的时间戳并合并所有匹配的用户 ID 来插入之前已删除的记录。这些记录全部来自 7 天前：

```
MERGE INTO target_table trg
    USING source_table TIMESTAMP AS OF date_sub(current_date(), 7) src
    ON src.user_id = trg.user_id
    WHEN MATCHED THEN UPDATE SET *
```

