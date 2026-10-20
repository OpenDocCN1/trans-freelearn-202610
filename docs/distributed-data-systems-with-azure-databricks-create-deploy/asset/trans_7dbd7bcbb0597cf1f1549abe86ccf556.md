

正如我们之前所做的那样，下面的示例指定了一个开始读取数据的起始时间戳：

```
streaming_events.readStream
  .format("delta")
  .option("startingTimestamp", "2020-06-15")
  .load("/mnt/streaming_events/user_events")
```

值得注意的是，表的模式始终是最新模式，因此在使用时间旅行选项时不会出现不兼容的情况。

## 流式模式

Delta Lake 允许使用两种不同的流式数据模式；即 Append 模式和 Complete 模式。第一个是默认选项，这意味着新记录会被追加到表中。让我们来看一下：

- 这也可以通过使用 outputMode 作为选项来设置，如下所示：

```
streaming_events.writeStream
  .format("delta").outputMode("append")
  .option("checkpointLocation", "/delta/streaming_events/_checkpoints/etl-from-json")
  .start("/delta/streaming_events/")
```

- 或者，我们可以使用表的名称：

```
streaming_events.writeStream
  .format("delta").outputMode("append")
  .option("checkpointLocation", "/delta/streaming_events/_checkpoints/etl-from-json")
  .table("streaming_events")
```

Complete 模式意味着表会在每批新数据中被完全替换。

- 我们也可以通过 outputMode 来设置此模式，如下所示：

```
streaming_events.writeStream
  .format("delta").outputMode("complete")
  .option("checkpointLocation", "/delta/streaming_events/_checkpoints/etl-from-json")
  .start("/delta/streaming_events/")
```

如果延迟要求较低，可以将进程设置为使用一次性触发器而不是流式源。这些触发器在我们以周期性方式而非持续流的方式工作时非常有帮助。一个例子是当我们处理仅在有新数据可用时才需要更新的报告时。

## 使用 Delta Lake 进行优化

在本节中，我们将继续使用 Azure Databricks 提供的 COVID 数据集。让我们开始吧：

- 1. 我们的第一个任务将是加载数据。为此，我们需要使用 `%fs`，它将查询数据集：
```
%fs
ls dbfs:/databricks-datasets/COVID/coronavirusdataset/
```
这应该会显示所有可用的示例数据集。

- 2. 然后，我们必须选择 PatientInfo 文件并将其加载到 spark DataFrame 中：
```
file_path = "dbfs:/databricks-datasets/COVID/coronavirusdataset/PatientInfo.csv"
df = spark.read.format("csv").load(file_path,header = "true",inferSchema = "true")
display(df)
```
在这里，我们可以看到已加载到 DataFrame 中的数据。这将用于创建要优化的 Delta 表：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_140_0.png)

图 4.2 – 我们将作为表存储的示例患者 DataFrame

3. 和往常一样，我们可以使用 describe 方法来显示 DataFrame 的底层结构，在本例中该结构由 Apache Spark 推断得出：

```
df.describe().show()
```

此命令的输出将向我们展示已加载数据的统计摘要：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_141_0.png)

图 4.3 – DataFrame 信息模式

4. 文件加载后，我们可以将它写入 Parquet 文件，后续将用于查询：

```
df.write.format("parquet").mode("overwrite").partitionBy("province").save("/tmp/covid_parquet")
```

我们这样做是为了将表写入一种格式，该格式已针对在数据湖中工作进行了优化，例如 Parquet。

5. 文件写入后，我们将再次读取它并用它来创建表。为了读取 DataFrame，我们必须运行以下命令：

```
covid_parquet = spark.read.format("parquet").load("/tmp/covid_parquet")
```

在这里，表是从 /tmp/covid_parquet 位置加载的。

6. 一旦我们将 Parquet 文件加载到 Apache Spark 中，我们就可以对数据运行查询。我们将运行两个查询。它们将基于计数和基于日期的聚合。为此，我们将使用 pyspark sql 函数：

```
from pyspark.sql.functions import count
```

```
display(covid_parquet.groupBy("date").agg(count("*").alias("TotalCount")).orderBy("TotalCount", ascending=False).limit(20))
```

前面的代码展示了如何显示对 patient_id 列进行简单计数聚合的结果，并按日期排序。以下屏幕截图向我们展示了此操作的输出：

```
from pyspark.sql.functions import count
display(covid_parquet.groupBy("date").agg(count("*").alias("TotalCount")).orderBy("TotalCount", ascending=False).limit(20))
```

![](img/7e45bd08916c2a17d4e75cbb898f41e8_142_0.png)

图 4.4 – 对 date 列的计数聚合

- 7. 根据同样的原理，我们也可以通过对 province 列进行聚合来计算每个省份的病例数：

```
display(covid_parquet.groupBy("province").agg(count("*").alias("TotalTransitedPlaces")).orderBy("TotalTransitedPlaces", ascending=False).limit(20))
```

![](img/7e45bd08916c2a17d4e75cbb898f41e8_142_1.png)

图 4.5 – 对省份的聚合

- 8. 然后，我们可以查询我们的数据。对 DataFrame 进行快速计数给出了 10,410 条记录：

```
covid_parquet.count()
```

Out[77]: 10410

图 4.6 – 行数

9. 最后，我们可以将数据以 delta 文件格式写入以创建我们的 delta 表。我们可以通过按 province 分区来完成此操作，province 是我们认为将频繁用于过滤数据的列，如下所示：

```
covid_parquet.write.format("delta").mode("overwrite").partitionBy("province").save("/delta/covid_delta/")
```

![](img/7e45bd08916c2a17d4e75cbb898f41e8_143_0.png)

图 4.7 – 以 delta 文件格式写入 DataFrame

10. 下一步是读取 delta 格式的文件以在 metastore 中创建我们的 delta 表。我们可以使用以下命令读取并显示数据：

```
covid_delta = spark.read.format("delta").load("/delta/covid_delta/")
display(covid_delta)
```

这样，我们就可以看到已加载的结果，如下所示：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_143_1.png)

图 4.8 – 将 delta 文件读入 Spark DataFrame

现在我们的 DataFrame 与 delta 文件关联，我们就可以用它来创建 delta 表并利用 Delta Lake 的优化功能。

11. 接下来，我们将使用 DataFrame 创建 delta 表。首先，我们必须运行以下命令，如果表已存在则将其删除：

```
display(spark.sql("DROP TABLE IF EXISTS covid_delta"))
```

12. 然后，我们必须通过使用 USING DELTA LOCATION 指定 delta 文件的位置来创建表：

```
display(spark.sql("CREATE TABLE covid_delta USING DELTA LOCATION '/delta/covid_delta/'"))
```

最后，我们必须应用 ZORDER 作为优化技术。这在 Delta Lake 中可用，旨在重新排序文件，使其尽可能接近。在本例中，我们的表很小，因此运行此操作不会产生显著效果。然而，当我们处理数据分散在不同文件中的大表时，它非常有用。

- 1. 我们可以通过在 delta 表上运行 OPTIMIZE 并加上 ZORDER BY 语句来运行优化过程。此语句仅应用于数据列，而不应用于分区列：

```
display(spark.sql("OPTIMIZE covid_delta ZORDER BY (date)"))
```

以下屏幕截图显示了整个优化操作的输出，其中我们通过读取 Delta 表创建并优化了一个表：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_144_0.png)

图 4.9 – 在 delta 表上执行 SQL 查询

2. 现在，我们可以在知道数据布局已根据我们将用于运行查询的过滤器进行优化的情况下运行查询。我们还知道文件数量已优化，以最小化文件读取并改进数据跳过技术：

```
display(covid_delta.groupBy("date").
  agg(count("*").alias("TotalTransitedPlaces")).
  orderBy("TotalTransitedPlaces", ascending=False).limit(20))
```

![](img/7e45bd08916c2a17d4e75cbb898f41e8_145_0.png)

图 4.10 – 按天计数聚合

3. 创建表的另一种方法是使用 SQL 命令。我们可以在 Azure Databricks 笔记本单元中运行以下命令，以删除任何现有表并创建表。我们可以通过选择所有列并按 province 分区来完成此操作：

```
%sql
DROP TABLE IF EXISTS covid_delta;
CREATE TABLE covid_delta
USING delta
PARTITIONED BY (province)
SELECT *
FROM delta.`/delta/covid_delta/`;
```

