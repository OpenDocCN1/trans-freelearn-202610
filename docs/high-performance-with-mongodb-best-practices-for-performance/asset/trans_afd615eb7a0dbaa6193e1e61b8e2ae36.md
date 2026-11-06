
```
{
  type: "like",
  timestamp: ISODate("2023-04-07T09:15:30Z"),
  targetPost: {
    id: ObjectId("5f3d7eb8c242d20df83d3a94"),
    author: "travel_adventures",
    snippet: "My day at Mount Fuji..."
  }
}
```
// 3-5 条更近期的活动
]
}

让我们回顾这个示例，了解计算模式和子集模式是如何在此应用的：

- **Computed pattern:**
  - Fields such as followerCount and followingCount are calculated ahead of time and stored directly in the user document
  - It reduces the need for on-the-fly aggregation or queries to count followers/following during profile views
- **Subset pattern:**
  - It embeds an array of recentActivities directly in the user document
  - It includes only the most recent posts, likes, or actions, optimized for quick access during profile page loads
  - More comprehensive activity history is deferred to a separate collection for less frequent queries

这种设计通过将最近的活动直接包含在用户文档中来优化个人资料页面的加载。完整的活动历史存储在单独的集合中，仅在用户查看更多活动时才会被查询。

## 场景 2：聊天系统

在以下场景中，Socialite 应用需要实现一个消息系统，能够高效地处理实时聊天和用户消息历史的访问。我们可能会想把所有消息存储在单个集合中，并反复查询以获取实时更新和历史数据，但随着消息量的增长，这会导致性能瓶颈。然而，通过 MongoDB，我们可以通过结合子集、桶化和归档模式来优化这一点。

以下示例演示了这种方法，并包含两个文档，一个来自活动消息集合，另一个来自归档消息集合：

```
```
// Active messages collection - current conversation state (for fast access)
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d7f6r"),
  participants: [
    ObjectId("5f3d7eb8c242d20df83d3a8c"),  // jane_smith
    ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a")   // john_doe
  ],
  lastMessage: {
    sender: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
    content: "See you tomorrow!",
    timestamp: ISODate("2023-04-12T18:45:22Z")
  },
  unreadCount: {
    "5f3d7eb8c242d20df83d3a8c": 0,
    "5f4e5b6d3c1e2a1b3c4d5e7a": 1
  },

  // Recent messages (subset pattern)
  recentMessages: [
    {
      sender: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
      content: "See you tomorrow!",
      timestamp: ISODate("2023-04-12T18:45:22Z"),
      read: false
    },
    {
      sender: ObjectId("5f3d7eb8c242d20df83d3a8c"),
      content: "Yes, 3 PM works for me.",
      timestamp: ISODate("2023-04-12T18:44:15Z"),
      read: true
    }
  ]
}
```
// 10-15 条最近的消息
]
}

// Archived messages collection - for older messages, using bucketing pattern
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d8f1t"),
  conversationId: ObjectId("5f4e5b6d3c1e2a1b3c4d7f6r"),
  date: ISODate("2023-04-11T00:00:00Z"),  // One bucket per day
  messages: [
    {
      sender: ObjectId("5f3d7eb8c242d20df83d3a8c"),
      content: "How about meeting tomorrow?",
      timestamp: ISODate("2023-04-11T15:30:12Z"),
      read: true
    }
    // More messages from this day
  ]
}
```

让我们回顾这个示例，了解子集、归档和桶化模式是如何在此应用的：

- **Subset pattern:**
  - The recentMessages field in the first document from the active messages collection stores the most recent 10–15 messages directly within the document
  - It enables quick access to the latest messages without the need for a separate query to a larger data store
- **Archive pattern:**
  - The second document uses the archive pattern to store older messages. The conversationId field links the bucket to a specific conversation, ensuring logical grouping by conversationId. The date field represents the day covered by the bucket, allowing for chronological organization of messages.
- This pattern ensures efficient storage and retrieval of older, less frequently accessed messages and reduces overhead when querying or analyzing historical conversations, as queries can target specific buckets based on date and conversationId.
- **Bucketing pattern:**
  - Older messages are grouped into daily buckets in the messages array in a separate collection with one document per conversation per day
  - It organizes historical messages efficiently to reduce the overhead of querying massive datasets and improves scalability for long-running conversations.

这种方法允许快速访问最近的消息，同时通过组织和高效查询历史消息的归档来保持可扩展性。

## Summary

通过将您的模式与数据使用的实际模式对齐（如何读取、写入或更新文档），您可以最大限度地减少 I/O 和资源争用。MongoDB 的灵活结构可以帮助，但只有在深思熟虑地使用、结合正确的嵌入、引用和索引时才有效。

这里有很多重要的收获。在设计模式之前，至关重要的是了解您的工作负载并考虑读写模式。确保您以文档而非表格的方式思考。拥抱文档模型的灵活性，而不是强加关系模式。记住为常见操作设计并优化具有最严格性能要求的<|message|> 操作。使用真实的数据量进行测试：适用于 100 个文档的内容可能在 1000 万个文档时失败。最后，始终要有意为之：深思熟虑的模式设计始终优于任意的数据存储。

MongoDB 中的模式设计既是一门艺术，也是一门科学。通过了解应用程序的工作负载并平衡读写效率，您可以创建既高效又可维护的模式。

MongoDB 在快速、水平可扩展的键值存储和功能丰富的数据库之间架起了桥梁。为了充分发挥其威力，请设计您的模式以利用文档模型的优势：嵌入相关数据、避免不必要的查找，并针对特定的访问模式进行优化。在下一章中，我们将基于这些模式设计原则，探索进一步增强应用程序性能的索引策略。

## 3

## 索引

最常见的数据库操作是什么？它是查询，或读取单个文档或多个文档。我们只插入一次文档，有时更新它们，而读取很多次。因此，确保这些读取尽可能快非常重要。

在本章中，您将学习如何使用索引来提高查询性能。您将首先了解索引是什么以及 MongoDB 如何使用它们。然后，您将探索查询计划以及如何解释它们以识别缓慢或低效的操作。您还将练习创建和使用索引来加速查询，并了解 MongoDB 如何选择使用哪个索引。到本章结束时，您将能够就何时以及如何使用索引来优化性能做出明智的决策。

本章将涵盖以下主题：

- Indexes and the types of indexes MongoDB supports
- Cardinality, selectivity, and how they affect indexes
- Best practices for optimizing compound indexes
- Why partial indexes and covered index queries are important for performance

## 介绍索引

让我们从一个基本示例开始。假设您有一个订单集合，并且您想要找到 ID 值为 295 的订单：

```
db.orders.find({ orderId: 295 })
```

如果没有索引，MongoDB 必须检查集合中的每个文档，以查看它是否具有 orderId 字段，然后查看其值是否为 295。对于包含数百万文档的集合，这可能非常缓慢。它导致时间复杂度为 O(n)，意味着所需时间随文档数量线性增长。
