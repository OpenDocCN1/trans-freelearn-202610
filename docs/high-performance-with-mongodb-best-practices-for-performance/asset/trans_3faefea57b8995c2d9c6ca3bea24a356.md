
Together, these features offer a robust and reliable foundation for building real-time applications that respond to data changes, while maintaining consistency and resilience, even in the face of disconnections or server failures.

## Change streams 如何工作：从写操作到事件

Change streams 建立在 MongoDB 的复制基础设施之上。理解这种关联有助于解释其能力和局限性。在高层次上，客户端应用程序通过以下步骤获知变更：

- 1. 应用程序对 MongoDB 集合执行写操作。
- 2. 该操作被记录在主节点的 oplog 中。
- 3. Change stream 功能监控 oplog 中的多数已提交条目。
- 4. 原始 oplog 条目被转换为更易于开发人员使用的格式。
- 5. 事件通过打开的游标传递给客户端应用程序。

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_193_0.png)

图 8.1：通过 change streams 通知文档变更的序列

图 8.1 显示了使用 change streams 将变更通知从 MongoDB 传递给客户端应用程序所涉及步骤的高层次视图。App_A 和 App_B 可能代表同一个应用程序。oplog 是 MongoDB 的一部分，而 change stream 游标（CS_Cursor）同时存在于 MongoDB 服务器内部以及 App_B 的客户端侧。

此过程确保所有数据库变更都被可靠捕获，并以一致的顺序传递给需要实时响应的应用程序。在分片集群中，该过程变得更加复杂，因为路由器必须在多个分片之间进行协调，同时保持事件的全局顺序。

## 事件结构和生命周期

每个 change stream 事件都包含有关发生了什么变化的信息，并提供维护连续性的工具。例如，这里是一个 products 集合中文档的 price 字段变更的 change stream 事件：

```
{
  "_id": {
    "_data": "825F65A78B00000012B022C0100296E5A1004..."  // Resume token
  },
  "operationType": "update",
  "ns": {
    "db": "inventory",
    "coll": "products"
  },
  "documentKey": {
    "_id": ObjectId("5f65a78b0000000000000001")
  },
  "updateDescription": {
    "updatedFields": {
      "price": 29.99,
      "lastModified": ISODate("2023-09-15T14:22:01.123Z")
    },
    "removedFields": ["promotion"]
  },
  "fullDocument": { // Only present with fullDocument option
    // Complete current state of the document
  }
}
```

如你所见，如果在 change stream 设置期间请求，change stream 事件可以包含完整文档。然而，这可能会显著影响性能，因此应谨慎使用。change stream 游标在其生命周期中经历以下状态：

- 1. **打开中：** 在 oplog 中建立其位置
- 2. **等待中：** 在变更发生前保持空闲
- 3. **传递中：** 向客户端发送事件
- 4. **恢复中：** 在断开连接后重新建立流
- 5. **失效：** 在某些操作后终止

理解此生命周期是构建弹性应用程序的关键。

## 有效实现 change streams

现在你已经熟悉了 change streams 的架构，让我们看看如何有效地实现一个使用它们的应用程序，以在不同场景下获得最佳性能。

## 选择合适的范围和过滤策略

MongoDB 在观察范围和过滤能力方面都提供了灵活性。这两个因素的组合对性能有显著影响。观察范围可以限制在集合、跨越数据库或扩展到整个部署。一般来说，范围越广，性能影响越大，因为消耗的资源更多，且必须处理的变更事件量更高。

表 8.1 概述了每个范围、其资源使用情况、预期的事件量以及在应用程序中可能选择它的典型用例：

| 范围 | 资源使用 | 事件量 | 用例 |
|---|---|---|---|
| 集合 | 较低 | 聚焦 | 特定业务事件、定向同步 |
| 数据库 | 中等 | 中等 | 跨集合工作流、数据库范围审计 |
| 部署 | 较高 | 最高 | 全局审计日志、多租户事件广播 |

*表 8.1：Change stream 范围和特征*

如表 8.1 所示，选择合适的观察范围是在可见性和性能之间取得平衡。集合级范围提供最有效的资源使用，适用于定向用例。扩展到数据库或部署范围会增加可观察性，但也会对数据库和应用程序提出更高要求。

## 最佳实践

选择满足你需求的最窄范围。这将通过减少数据库服务器上的资源消耗来最大化性能，并最大限度地减少应用程序需要处理的事件数量。

然而，你需要将其与 change streams 的数量取得平衡，因为每个 change stream 实际上都在解析和过滤 oplog。例如，如果一个数据库有 20 个集合，并且你想处理其中 16 个的变更，那么在数据库上有一个 change stream 比在每个集合上建立 16 个 change stream 更好。

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_196_0.png)

## 使用聚合管道进行服务器端过滤

Change streams 最有效的性能技术之一是使用聚合管道进行服务器端过滤。这种方法显著减少了网络带宽消耗和应用程序客户端处理开销。

例如，如果你只对产品文档的价格变更感兴趣，可以设置一个 change stream，使用聚合管道仅匹配 price 字段的变更：

```
// MongoDB Node.js driver code snippet
// Opening a change stream with a filter pipeline
const pipeline = [
  // Only capture update operations on the products collection
  { $match: { 'ns.coll': 'products', operationType: 'update' } },

  // Further filter to only include price updates
  { $match: { 'updateDescription.updatedFields.price': { $exists: true } } }
];

const filteredStream = collection.watch(pipeline);
```

此过滤后的 change stream 仅为 products 集合中 price 字段的更新发出事件，显著减少了通过网络发送并由客户端应用程序处理的数据量。

## 文档查找策略和性能

Change streams 提供了多种访问文档数据的选项，每个选项的性能影响各不相同：

- **仅增量**：默认行为；仅包含变更信息。
- **当前状态查找**：fullDocument: 'updateLookup' 获取当前文档。
- **变更前和变更后镜像（MongoDB 6.0+）**：提供变更前后的文档状态。此可选功能可能会影响性能，因为数据库必须捕获并存储变更前或变更后的文档。

最佳方法取决于你的具体用例。问自己以下问题来指导你的决策：

- updateDescription 中的值增量是否足以满足你的应用程序用例？
- 你能承受多少额外的网络带宽？
- 额外的文档查找延迟是否可以接受？

选择合适的选项取决于你的应用程序对准确性、性能和资源效率的需求。例如，'updateLookup' 选项将在更新发生后某个时间获取 fullDocument。如果文档很大，这可能会产生明显的性能影响。然而，它也可能需要在你的应用程序中进行仔细处理。例如，如果文档频繁变更并且有待处理的变更队列，你最终可能会读取比当前晚三个变更的版本。如果重放 change stream 也会发生这种情况；使用 'updateLookup' 时你的应用程序接收到的 fullDocument 是文档的当前版本，而不是变更时的版本。

## 构建价格监控服务

为了在实践中说明这些概念，让我们实现一个服务来监控产品价格变更并通知感兴趣的各方。以下代码设置了一个聚合管道来监视 price 字段的更新，并在变更超过定义阈值时通知应用程序的其他部分：

```
// Code snippets for MongoDB Node.js driver
// Configure the collection to using preImages (before the changes happen)
db.runCommand({ collMod: "yourCollectionName",
    changeStreamPreAndPostImages: { enabled: true }
});
```
