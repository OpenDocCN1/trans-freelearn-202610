
这里有一些日志消息片段，来自于使用了索引来满足多个查询谓词子句的查询，但随后仍需检查文档中的额外字段以确定是否应将其返回给客户端：

```
"keysExamined":977,"docsExamined":977,"nreturned":4
"keysExamined":69070,"docsExamined":147,"nreturned": 0
```

虽然 keysExamined 与 docsExamined 的比率可以告诉你索引在缩小查询需要检查的文档数量方面的效率如何，但 nreturned 显著更低仍然表明必须直接对文档应用额外的过滤器。这仅适用于查询，而不适用于聚合，因为聚合可以通过将其聚合为更少的总组来返回更少的文档。

## Waiting for disk or other resources

日志中可能最有用的指标之一是 storage，在快速查询的情况下它会为空。然而，如果任何索引或数据页不在存储引擎缓存中，与之相关的指标将出现在 storage 字段中。即使有效使用索引的查询也很慢，通常也是因为数据尚未在 WiredTiger 缓存中，而该字段将指示是否如此。

以下是三个查询的部分日志消息可能的样子：

```
storage:{data:{bytesRead:8674010,timeReadingMicros:6297}},durationMillis:105
storage:{data:{bytesRead:15353,timeReadingMicros:918415}},durationMillis:919
storage:{data:{bytesRead:31033716,timeReadingMicros:82948}},
durationMillis:145
```

从数据库的角度来看，这表明数据并非来自其缓存。这些数据仍可能已从文件系统缓存中读取。

在不同情况下，根本原因可能是磁盘异常缓慢（第二个示例行）、大量数据未存在于缓存中，或内存排序必须将数据溢出到磁盘。你可以通过在日志消息中查找 "usedDisk":true 来跟踪此问题，或通过检查 explain 输出中的更详细信息。

同样，日志消息可以表明慢查询使用了最佳执行计划，但仍因其他不可用的资源而变慢，例如等待其他缓慢操作。尝试优化该查询毫无意义，因为需要解决的是整个系统或其他缓慢操作。

## How to influence query execution

有时，无论你多么努力为查询创建完美的索引，查询规划器仍会选择不同的索引。你可以通过两种方式影响执行，以强制选择你偏好的索引，或完全阻止不期望的查询执行。

## Using hint

你可以将 hint 指令与索引的名称或定义一起传递给任何查询。这指示查询引擎完全绕过查询缓存和规划过程，而改用指定的索引进行查询。使用 hint 时需注意以下几点。指定不存在的索引将返回错误。指定稀疏、部分或不兼容的索引可能会返回不完整的结果，如果索引不包含满足查询谓词的所有文档。

在应用程序代码中使用 hint 时，务必谨慎操作：定期检查它是否仍有必要。通常最好删除导致查询引擎做出次优选择的额外索引，但在无法做到时，使用 hint 可以提供快速解决方案。

以下是强制 find 通过 hint 按名称使用索引的代码示例：

```
db.coll.find({<filter>}).hint("indexName")
```

谨慎使用 hints。随着数据和索引的演变，始终验证其持续的相关性。

## Using query settings

MongoDB 8.0 版本引入了一个名为 persistent query settings 的新功能。此功能允许你通过在服务器上自动添加 hints 来控制查询执行，而无需对应用程序进行代码更改、定义拒绝过滤器并配置其他字段。Rather than relying on a hint being explicitly added to a particular query shape in the application code（如果查询在系统的多个部分被调用，则存在遗漏某些位置的风险），你可以配置服务器在观察到特定查询形状时自动应用 hint。

此命令为特定数据库、集合和查询形状设置索引 hint：

```
db.adminCommand({
  setQuerySettings: {
    find: "collName",
    filter: {<filter>},
    $db: "dbName"
  },
  settings: {
    indexHints: {
      ns: {db: "dbName", coll: "collName"},
      allowedIndexes: ["indexName"]
    }
  }
})
```

当你想完全阻止有问题的查询运行时，修改 settings 字段以包含 reject:true 指令。有关 setQuerySettings 命令的更多信息，请访问 https://www.mongodb.com/docs/manual/reference/command/setquerysettings/。

## Other options

另一个值得注意的设置是启动 mongod 时的 --no tablescan 选项，它像一把大锤子一样存在，尽管通常不推荐在生产环境中使用。此设置可以阻止任何具有查询过滤器但没有可用索引的查询。这可能在测试或暂存系统中启用以确定是否有任何查询意外运行集合扫描是安全的，但在生产中使用这种不加区分的工具可能不是好主意。你可以在生产中监控此类查询，然后决定使用查询设置将其从系统中阻止，或添加索引来支持它们。

## MQL semantics and indexes

如第 3 章《Indexes》所述，索引旨在支持 MongoDB 查询语言（MQL）的 find 语义，但这意味着什么？MQL 有多个组件，其中一些特定于聚合，一些特定于更新，一些特定于 find 命令过滤器，被称为 match expressions。这是在为 find、update 和 delete 命令指定查询谓词时使用的语法，也是 $match 聚合阶段期望的语法。索引支持 match expressions 的一些特殊性，这会影响索引被特定查询使用的效率。理解这一点可以帮助你发现、调试和重写难以优化的查询，并从一开始就编写更高效的查询。

## Challenges with arrays and multikey indexes

存储数组的字段为查询规划和索引使用带来了独特的挑战。包含可能为数组的字段的文档需要特别考虑，以避免成为非预期性能问题的来源。

## Equality is not just equality

在 MQL 查询中指定相等（$eq）于某个值——显式或隐式——意味着 *相等* 或 *数组中某个元素等于该值*。

例如，`db.coll.findOne({a:5})` 匹配 `{a:5}` 以及 `{a:[1, 5, 10]}`。

这意味着对字段 a 的索引包含条目 5 必须指向 a 为 5 的文档，以及 a 为包含 5 的数组的文档。条目看起来完全相同，但一旦任何数组元素被索引，索引就会获得 multikey 属性，这告诉查询引擎 a 可能是数组。因此，在返回 a 的值时，不能仅从索引返回；必须获取文档以返回字段的实际值。

请注意，在聚合中，$eq 表达式没有这种细微差别。在 `{$eq:[v1, v2]}` 中，如果两个值相等，结果为 true，否则为 false。聚合表达式不会深入数组以匹配单个值。

## $elemMatch

我们如何索引数组不仅仅影响返回潜在数组字段；它还影响我们如何使用索引来检查 $elemMatch 条件。$elemMatch 操作符检查 *是否至少有一个数组元素满足查询条件*。

例如，查询 `{a:{$elemMatch:{$eq:5}}}` 将匹配 `{a: [1, 4, 5]}` 但不会匹配 `{a: 5}`，因为这里的 5 不是数组元素。

同样，`{a:{$gt:5, $lt:10}}` 将匹配 `{a: [1, 4, 11]}`，因为两个条件都为真；a 有一个小于 10 的元素，它也有一个大于 5 的元素。

为了查找同时满足两个条件的单个元素，我们必须写成 `{a:{$elemMatch:{$gt:5, $lt:10}}}`。现在，含义变为：*a 必须有一个同时大于 5 且小于 10 的单个元素*。

鉴于我们在 B-tree 中索引数组值，在某些情况下索引可能无法像你期望的那样高效地回答查询。一个简单的 `{a:{$elemMatch:{$eq:5}}}` 查询可以使用索引找到 a 为标量 5 或包含 5 的数组的所有文档。
