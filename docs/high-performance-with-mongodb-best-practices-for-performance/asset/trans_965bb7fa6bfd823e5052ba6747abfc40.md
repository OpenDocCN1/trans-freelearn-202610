
运行 mongostat 时，可以像这样连接到 MongoDB 实例：

// Run from the System Command Line
mongostat --host localhost:27017 --rowcount 2
```

你将看到类似以下、每秒更新的输出：

```
insert query update delete getmore command dirty used flushes vsize res
    0     8      0      0       0      3|0   3.4% 7.4%      0 1.2G   512M
    1    12      0      0       0      5|0   3.7% 2.5%      0 1.2G   210M
```

关键字段包括以下内容：

- **操作计数器：** 显示每秒的操作数量，指示工作负载的形状和强度（insert、query、update 和 delete）。
- **内存：** 显示内存使用详情，包括映射内存、虚拟大小和常驻内存。显著变化或较高的常驻内存可能表明内存压力（mapped、vsize 和 res）。
- **命令：** 显示每秒执行的命令数（command）。

对于副本集，你可以使用 --discover 选项自动跟踪所有成员及其状态：

```
mongostat --discover
```

使用 mongostat 进行快速健康检查，以观察应用程序更改的即时影响，或在性能事件期间获得数据库活动的概览。

## mongotop：集合级读写时序

虽然 mongostat 提供服务器范围的概览，但 mongotop 专门针对每个集合的读写操作所花费的时间。这对于识别活动最繁重的集合或可能导致 I/O 瓶颈的集合非常宝贵。

与 mongostat 类似，你将 mongotop 连接到正在运行的实例：

// Run from the System Command Line
mongotop --host localhost:27017 --rowcount 2
```

输出如下所示：

```
ns              total   read    write   2025-04-30T16:30:27-04:00
mydb.users      0.1ms   0.1ms   0.0ms
mydb.orders     5.2ms   2.8ms   2.4ms
```

各列显示如下内容：

- `ns`：正在监控的命名空间（`<database>.<collection>`）
- `total`：在该间隔期间服务器对该集合执行操作所花费的总时间
- `read`：用于读操作的时间
- `write`：用于写操作的时间

使用 mongotop 精确定位热点集合，这些集合可能通过模式优化、索引改进或在活动持续较高并影响整体性能时进行分片而受益。例如，在前面的输出中，你可以看到 `mydb.orders` 集合的读写活动明显高于 `mydb.users`，这可能需要进一步调查。

## serverStatus：通过 Shell 获取全面指标

对于 MongoDB 服务器状态最详细的快照，`serverStatus` 命令是权威来源。它返回一个包含数百个指标的大型 JSON 文档，涵盖服务器操作的几乎每个方面：

// Run from the MongoDB shell
db.adminCommand({ serverStatus: 1 })

// Or using the shell helper
db.serverStatus()
```

虽然输出过于庞大无法在此完全详述，但重要部分包括以下内容：

- `opcounters`：服务器启动以来所有数据库操作的详细计数
- `connections`：有关当前、可用和排队的客户端连接的信息
- `locks`：有关锁竞争的信息（数据库、集合或全局）
- `wiredTiger`：WiredTiger 存储引擎特定的深度指标，包括以下内容：
  - 缓存使用情况（读入缓存的页面、从缓存写入的页面、脏字节）
  - 事务检查点
  - 块管理器统计信息
- `mem`：常驻和虚拟内存统计信息
- `metrics`：各种运营指标，例如文档插入/更新/删除速率、查询执行程序统计信息和复制详情

由于数据量庞大，serverStatus 不太适合实时监控，但对于深入分析、建立性能基线以及程序化监控至关重要，在程序化监控中，外部系统会提取特定指标并随时间跟踪。

## 数据库分析器：深入分析慢操作

当你需要了解单个查询的性能特征时，数据库分析器是首选工具。它捕获特定数据库的数据库操作详细信息，可帮助你识别可能影响应用程序性能的慢速或低效查询。

开始使用时，你可以使用 MongoDB Shell 中的 setProfilingLevel() 方法为特定 mongod 实例启用分析器。该方法提供三种分析级别，各自适用于不同的需求：

- **级别 0（关闭）**：分析器默认处于禁用状态，不收集任何数据
- **级别 1（慢操作）**：仅捕获超过指定阈值（slowms，默认 100 ms）的操作数据
- **级别 2（所有操作）**：捕获所有操作的数据（由于性能开销，在生产环境中使用时请谨慎）

此外，你可以配置 slowms，即慢操作阈值。任何完成时间快于此阈值的用户操作都不会记录到 mongod 日志中。还可以设置 sampleRate，以控制应记录的慢操作比例。

以下命令在 MongoDB Shell 中运行，将为当前数据库启用数据库分析器，慢操作阈值为 20 毫秒，采样率为 42%：

```
db.setProfilingLevel(1, { slowms: 20, sampleRate: 0.42 })
```

启用后，你可以查询分析器数据以分析最近的操作。例如，此命令查找并显示最近的五个分析操作：

// Run from the MongoDB shell
// Find the 5 most recent profiled operations
db.system.profile.find().sort({ ts: -1 }).limit(5).forEach(printjson)
```

已分析的操作存储在为其启用了分析的每个数据库中的 system.profile capped 集合中。该集合中的每个文档都包含以下等详细信息：

- millis：操作持续时间（毫秒）
- ns：目标命名空间（database.collection）
- command：与操作关联的完整命令文档
- execStats：查询操作的详细执行统计信息（类似于 explain 操作的输出）

以下是慢查询的分析器条目示例。由于我们在连接到集群时应用程序的连接字符串中指定了 appName 值，因此操作旁边会显示应用程序名称：

```
{
  "op": "query",
  "ns": "mydb.orders",
  "command": {
    "find": "orders",
    "filter": { "status": "shipped", "orderDate": { "$gt": ISODate("2025-01-01") } },
    "sort": { "customerName": 1 },
    "limit": 100
  },
  "keysExamined": 0,
  "docsExamined": 1000000,
  "nreturned": 53,
  "responseLength": 24601,
  "millis": 2543,
  "planSummary": "COLLSCAN",
  "execStats": {
    // Detailed execution statistics
  },
  "client": "192.168.1.50",
  "user": "appuser",
  "appName" : "my-custom-microservice-01"
}
```

在此名为 my-custom-microservice-01 的应用程序示例中，我们可以立即识别出几个问题。操作耗时 2.5 秒（"millis": 2543），它执行了集合扫描（"planSummary": "COLLSCAN"），并且检查了 1000000 个文档（docsExamined）才返回 53 个文档（nreturned）。

这些分析器详情显示查询是在扫描整个集合而不是使用索引，这非常低效，尤其是在对 "status" 和 "orderDate" 进行过滤的情况下。该模式清楚地表明这些字段上没有合适的索引。为了提高性能，你应该按该顺序在 "status"、"customerName" 和 "orderDate" 上创建复合索引，从而使 MongoDB 能够在不扫描整个集合的情况下高效定位匹配文档。

## 生产环境中 MongoDB 分析器的最佳实践

在生产环境中以级别 2（分析所有操作）启用 MongoDB 数据库分析器可能会引入显著的性能开销。在大多数生产场景中，最佳做法是使用级别 1 分析、设置 sampleRate，或仅在故障排除期间的短暂、有针对性的时间内启用完整分析。调整实时环境中的分析器设置时，始终监控系统影响。

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_344_0.png)

使用分析器数据精确定位低效查询、检测缺失索引并指导模式改进。通过分析慢操作的模式，你可以进行有针对性的优化，显著提高应用程序性能。

## 与外部监控系统的集成

虽然 MongoDB 提供了强大的内置工具，如 mongostat、mongotop 和 serverStatus 命令，并且 MongoDB Atlas 提供全面的托管监控套件，但现代应用程序环境很少孤立运行。为了实现真正的端到端可观测性，将 MongoDB 的性能信号集成到更广泛的外部监控和可观测性平台中至关重要。

此集成使你能够执行以下操作：

- 将数据库行为与应用程序性能和基础设施健康状况相关联
- 创建涵盖整个技术栈的统一仪表板
- 在所有组件中实施一致的告警
- 获得性能分析的历史背景
- 支持跨系统边界的根本原因分析

本质上，通过将 MongoDB 的性能数据输入到组织更广泛的可观测性工具中，你可以获得更全面的视图，更轻松地将数据库性能与整体应用程序健康状况联系起来，集中管理告警，并在整个技术栈中进行全面的根本原因分析。
