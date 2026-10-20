

同样地，我们可以使用 delta 前缀和表路径来指定一个 Delta 表，如以下代码片段所示：

```
CACHE SELECT col1, col2, col3
FROM delta.`our_table`
WHERE our_condition
```

在下面的截图示例中，我们在笔记本的 SQL 单元格中运行此操作，并将条件指定为 COVID 数据集中的特定省份：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_164_0.png)

图 5.9 - 指定要缓存的 Delta 表的一部分

这将仅记录符合我们条件以供缓存的指定列，从而防止后续查询扫描远程文件。

在处理 Delta 表时这样做并非必要，因为数据在查询时会被缓存，但当你需要提升查询性能时，这会很有用。

一个简单的例子是缓存名为 data 的表，如下所示：

```
CACHE SELECT * FROM user_data
```

或者，我们也可以对会话期间会频繁使用的部分数据执行此操作，如下所示：

```
CACHE SELECT user_id, user_cohort FROM user_data WHERE user_cohort=10
```

接下来，我们将了解如何配置 Delta 缓存的行为。

## 配置 Delta 缓存

建议始终为 worker 实例使用 Delta 缓存加速的集群，这些实例是运行 SSD 磁盘的优化实例。这些实例已默认配置为与 Delta 缓存一起工作，但你可以在会话创建期间使用 Apache Spark 选项手动配置其参数：

- 要设置最大磁盘使用量，默认设置为 worker 本地存储的一半，可以使用 maxDiskUsage 选项，以字节为单位，如以下示例所示，我们将其设置为 50g：
  
  ```
  spark.databricks.io.cache.maxDiskUsage 50g
  ```
- 我们还可以使用 maxMetaDataCache 选项指定以字节为单位的元数据最大分配空间，如以下示例所示，其中设置为 1g：
  
  ```
  spark.databricks.io.cache.maxMetaDataCache 1g
  ```
- 我们还可以禁用压缩，默认情况下是启用的，如下所示：
  
  ```
  spark.databricks.io.cache.compression.enabled false
  ```
- 要禁用 Delta 缓存，我们可以使用以下 Scala 命令：
  
  ```
  spark.conf.set("spark.databricks.io.cache.enabled",
  "false")
  ```

值得注意的是，禁用 Delta 缓存时，我们不会丢失已存储在 worker 本地存储中的数据，但会避免添加更多数据。

在下一节中，你将阅读有关 DFP 的内容，DFP 是 Delta Lake 的一项功能，可提高查询性能。

## 使用 DFP 优化查询

DFP 是 Delta Lake 的一项功能，可自动跳过与查询无关的文件。它是 Azure Databricks 中的默认选项，通过收集 Delta Lake 中文件的数据来实现，无需在查询中显式声明应跳过文件，通过利用数据的粒度来提高性能。

DFP 关于进程是否启用、表的最小大小以及触发进程所需的最小文件数等行为，可以通过以下选项管理：

- `spark.databricks.optimizer.dynamicPartitionPruning`（默认值为 true）：DFP 是否启用。
- `spark.databricks.optimizer.deltaTableSizeThreshold`（默认值为 10 GB）：激活 DFP 的 Delta 表的最小大小。
- `spark.databricks.optimizer.deltaTableFilesThreshold`（默认值为 1000）：表示在连接的探测侧 Delta 表的文件数，用于触发 DFP。如果文件数较少，则触发该进程不值得。

DFP 可用于非分区表、非选择性连接以及对它们执行 DML 操作时。

DFP 带来的改进与数据的粒度水平相关。因此，在使用 DFP 时应用 Z-Ordering 可能是一个好主意。

## 使用 DFP

为了首先了解我们如何应用 DFP，让我们看一个我们手动使用字面量语句以及另一个 Delta Lake 功能 **动态分区修剪 (DPP)** 的示例。

在物理规划阶段，Delta Lake 为查询创建可执行计划。该计划将计算分布在多个 worker 的集群中。该计划经过优化，仅扫描与查询相关的文件。

如果我们要运行查询但希望根据条件显式过滤结果。更具体的例子是选择我们表中的 col1、col2 和 col3，并基于最大值对其中一列应用过滤条件，如以下代码片段所示：

```
SELECT col1,col2,col3
FROM our_table
WHERE col1> max_value
```

Delta Lake 会保留我们列中的最小值和最大值信息，因此它会根据我们的过滤条件自动跳过与查询无关的文件。

这是可能的，因为过滤包含字面条件（在我们的例子中是最大值条件），因此 Delta Lake 可以规划和优化文件扫描。如果过滤条件是连接的一部分，Delta Lake 无法规划要跳过哪些文件，因为过滤条件是另一个查询的结果。这称为 DPP，是 Delta Lake 的先前功能，与 DFP 一样，旨在提高我们查询的性能。

以下类型的查询，即我们先连接两个或多个表，在使用基于星型模式的数据模型时非常常见：

```
SELECT col1, col2, col3
FROM fact_table src
JOIN dim_table trg
ON src.id_col=trg.id_col
WHERE col1=our_condition
```

在这种情况下，条件作用于维度表而不是事实表。Delta Lake 将不得不扫描事实表上的所有文件，扫描维度表中的所有文件，应用过滤条件，然后执行连接。

在这种情况下，DPP 不会起作用，但如果我们启用了 DFP，它将重新排序查询的执行计划，以便能够预先检查两个表上的条件，这使得有可能跳过两个表上的文件并提高查询性能。

DFP 在 Azure Databricks 中默认启用，并根据以下规则应用于查询：

- 连接策略为 BROADCAST HASH JOIN。
- 连接类型为 INNER 或 LEFT-SEMI。
- 内表是 Delta 表。
- 内表中的文件数量足够多，以证明应用 DFP 是合理的。

根据所选选项，DFP 将在连接操作中应用。

## 使用 Bloom 过滤器

Bloom 过滤器是一种基于条件高效过滤数据库中记录的方法。它们具有概率性质，用于测试元素是否属于集合。我们可能会遇到假阳性，但不会遇到假阴性。这些过滤器被开发为数学结构，用于在扫描的数据量不切实际时应用，并基于哈希技术。

## 理解 Bloom 过滤器

如本节介绍中所述，Bloom 过滤器是用于测试元素是否属于某个类别的概率数据结构。该结构是一个固定长度的位数组，使用哈希函数填充，将信息映射为 1 和 0。数组的长度取决于容忍的假阳性数量。Bloom 过滤器具有恒定复杂度，相对于项目而言所需空间非常少。

为了将数据映射到这种概率数据结构，使用哈希函数来获取数据的指纹，即该哈希（几乎）是唯一的，可用于比较或识别数据。这些哈希函数是单向操作，必须始终为相同的数据提供相同的输出，并且——如果可能——输出值必须均匀且随机分布，以便相似的输入不会给出相同的结果。

这些哈希之后可以按位比较以检查相似性。

在下面的截图示例中，我们比较 W 元素是否属于 {x, y, z} 组。为此，我们将比较哈希 W 的结果哈希数组所具有的位置与组中所有哈希共同填充的位置。如果 W 哈希中的所有 1 都在组的哈希填充的位置中，则 W 必须在集合中。如果 W 哈希中的任何位落在 0 中，则它们不属于该集合：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_168_0.png)

图 5.10 – 比较哈希以检查成员资格（图片取自 https://en.wikipedia.org/wiki/Bloom_filter#/media/File:Bloom_filter.svg）

在这种情况下，W 元素不在集合中，因为它哈希到包含 0 的位位置。

