
## 单字段索引

MongoDB 中的单字段索引是在集合的单个字段上创建的索引。以下是如何在单个字段上创建基本索引的方法：

```
db.collection.createIndex({ fieldName: 1 })
```

在“什么是索引？”部分，我们已经看到过使用 {orderId:1} 的示例，它可以加速针对特定订单 ID 的查询，以及按订单 ID 进行的排序或范围查询。

每个常规集合中都存在一个单字段索引，即 _id 索引，它强制唯一性，尽管在运行命令列出索引时不会明确显示唯一属性：

// Command to show all indexes on collection
db.collection.getIndexes()
// output showing _id index only
[ { "v" : 2, "key" : { "_id" : 1 }, "name" : "_id_" } ]
```

这就是查看特定集合中存在哪些索引的方法。现实生活中你创建的大多数索引都不是单字段索引；然而，每个常规集合默认至少有一个索引：_id 字段上的唯一索引，它是主键。

## 复合索引

复合索引是在多个字段上创建的。以下是创建三个字段索引的示例：

```
db.collection.createIndex({ status: 1, country: 1, zipString: 1 })
```

复合索引存储每个文档的多个字段值，因此每个条目都有多个值连接在一起。B 树按组合值对数据进行排序。让我们看一个按 status、country 和 zip 索引的示例：

| 集合中 status、country 和 zip 的值 | 这些值在索引中存储的顺序 |
|---|---|
| "processing", "US", "10007" | "done", "FR", "" |
| "done", "US", "95401" | "done", "US", "14850" |
| "done", "FR", "" | "done", "US", "90210" |
| "done", "US", "90210" | "done", "US", "95401" |
| "done", "US", "14850" | "processing", "US", "10007" |

表 3.1：索引排序如何影响文档检索

首先，按 status 排序记录，然后按 country，最后按 zip。你可能已经看出，在创建复合索引时字段的顺序非常重要。它允许你仅对第一个字段或其任何前缀字段进行查询时使用复合索引。稍后会详细介绍。

## 多键索引

多键索引在技术上并不是你显式创建的单独索引类型；你仍然是在一个或多个字段上创建索引。但如果这些字段中的任何一个包含数组，MongoDB 会为数组中的每个元素创建索引条目，并将索引标记为多键索引。以下是一个简单示例：

// If documents have: { tags: ["electronics", "sale", "new"] }
db.collection.createIndex({ tags: 1 })
// Creates index entries for "electronics", "sale", and "new"
// all of which point to the same recordID
```

如果任何被索引的字段包含数组，单字段、复合和其他索引都可以被标记为多键。我们将在本章剩余部分以及第 12 章《高级查询和索引概念》中再次探讨其影响。

## 稀疏索引

如果你指定索引应为稀疏索引，则只有包含被索引字段的文档才会被插入到 B 树中。这与常规（非稀疏）索引形成对比，后者会为集合中的每个文档建立索引，即使文档不包含被索引的字段。

以下是如何在 MongoDB 中创建稀疏索引的方法：

// sparse is an attribute of index being created
db.collection.createIndex({a:1}, {sparse:true})
```

上述语句在 a 字段上创建索引，仅包含存在 a 字段的文档。

由于 MongoDB 查询语言（MQL）的空值语义，字段缺失的非稀疏索引文档与字段存在且值为 null 的文档被索引的方式相同。在稀疏索引中，null 条目表示字段存在且被设置为 null。我们将在第 12 章《高级查询和索引概念》中进一步探讨这一点。

## 通配符索引

通配符索引允许你在事先不知道字段名称的情况下指定索引。在动态模式中这种情况并不少见：

// Index fields inside attr subdocument
db.collection.createIndex({ "attr.$**": 1 })
// Sample document
// { ...,
//   attr: {
//     genre: "Mystery",
//     author: "John Smith",
//     pages: NumberInt(298),
//     ...
//   }
// }
// query using the index
db.collection.find({"attr.pages":{$gt:500}})
```

这允许你使用 attr 下存储的任何属性的索引进行查询，而无需为每个属性创建单独的索引。通配符索引默认情况下为稀疏索引。

## 部分索引

有时，你可能只想对文档的子集进行索引和查询。部分索引仅对匹配指定过滤器的文档建立索引。此命令在两个字段上创建索引，但仅对 status 等于 "processing" 的文档建立索引：

```
db.orders.createIndex(
  { country: 1, dateOrdered: 1 },
  { partialFilterExpression: { status: "processing" } }
)
```

包含 {status:"processing"} 以及其他查询谓词的查询将能够使用此部分索引。

使用部分索引可以显著减小索引大小并节省资源。当你的查询仅针对集合中的一小部分时，这尤其有用。例如，如果你经常只查询活跃文档，而活跃文档只占数据的一小部分，那么具有部分过滤器 {active:true} 的索引将既紧凑又高效。当文档更新为 {active:false} 时，它会自动从索引中移除。

这不是所有索引类型和属性的详尽列表；你可以在 https://www.mongodb.com/docs/manual/indexes/ 的 MongoDB 服务器文档中阅读更多相关信息。

## 设计高效索引

索引是 MongoDB 性能优化策略的基石，能够在大型数据集上实现快速高效的查询和排序。然而，设计有效的索引不仅仅需要理解其基本功能；它要求仔细考虑查询模式、字段基数和操作需求。关于低基数字段和不等式查询的常见误解常常误导开发者，但正确的索引策略可以大幅提升性能。复合索引、索引压缩、覆盖查询和部分索引为优化数据库操作提供了强大的工具，而 **Equality, Sort, Range**（ESR）准则简化了围绕字段排序的复杂决策。

在本节中，我们将探讨索引设计的基石原则，涵盖基数与选择性、复合索引设计、前缀压缩、部分索引以及索引如何支持聚合管道等关键主题，为你提供设计可最大化应用程序性能的索引的工具。

## 基数与选择性

存在一种常见的误解，即低基数字段（具有少量唯一值的字段）不应包含在索引中。这完全不正确。

**基数**在数据库中指的是某个字段可以拥有的不同值的数量。布尔字段的基数为二：true 或 false。唯一 ID 字段的基数等于集合中的文档数量。姓氏的基数高于名字。基数对于查询有效性的重要性不如选择性，即查询中特定值在集合中匹配的文档数量。

例如，假设一个 orders 集合具有 status 字段，其值可以是 "processing" 或 "done"。在任何给定时间，status 为 "processing" 的订单少于 0.01%，因为大多数订单都是 "done"。在 status 上建立索引对于查询处理中的订单非常优秀，因为它具有高度选择性，即使该字段基数较低。另一方面，使用该索引查找所有已完成的订单帮助有限，因为它匹配集合中 99.99% 的所有文档。
