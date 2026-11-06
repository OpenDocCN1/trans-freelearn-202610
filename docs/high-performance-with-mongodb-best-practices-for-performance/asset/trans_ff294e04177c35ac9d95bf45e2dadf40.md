
这就是 MongoDB 允许你设置均衡器窗口的原因，均衡器窗口是允许块迁移的特定时间段。例如，你可以将均衡限制在夜间时段或流量最低的非高峰期。这样可以在白天保持最佳性能，同时仍允许系统在更安全且不会产生明显影响时重新分布数据。

但即使设置了均衡窗口，你也需要保持关注。均衡器可以根据数据大小而不是访问频率进行操作。这意味着它可能无法捕捉到服务流量更多但文档更少的热点分片。定期审查不仅是数据的分布而且是负载的分布对于避免静默性能下降至关重要。balancerStatus、currentOp 和 sh.status() 命令在这里是你的好帮手，以及显示每个分片上操作数量的监控图表。最后需要记住的是，均衡器擅长在略有不平衡的分片集合中移动小块数据以恢复平衡。当你需要整个分片被数据填充时，重新分片可能更快。

## 预分片：是否、何时以及如何

大多数情况下，MongoDB 的均衡器在应对不断增长的数据并保持其分布方面做得很好。但在某些场景下，例如大规模初始导入，你不希望将所有内容插入到一个分片中然后等待均衡器追赶上来。这就是预分片发挥作用的地方。

预分片取决于你的分片键值范围。你需要创建一个脚本，使用 moveRange 命令将分片键值的完整范围拆分为多个块，并将其均匀分布到将接收新数据的分片中。如果你的分片键值是字符串，如果你想在三个分片上开始使用块，可以使用 MinKey-k、k-s 和 s-MaxKey 这样的范围。如果你的值是其他类型，请确定数据集中值的近似范围，然后相应地拆分它们。请记住，你不必非常精确，因为一旦加载足够的数据，均衡器就能轻松处理较小的失衡。你想要避免的是开始时所有数据都进入单个分片。

通常，你希望在向全新空集合进行批量导入之前，或在设置新集群的早期阶段使用预分片。预分片在你事先了解数据的分布方式，并希望有一个良好的开端时最简单。

预分片是主动的。它将你的思维从被动的负载管理转变为有意的数据规划，在正确的使用下，它是确保集群启动并保持健康的最有效方法之一。

## 移动未分片集合

数据库中的并非每个集合都会被分片。事实上，所有较小的集合都将保持未分片，只有太大而无法容纳在一个分片上的集合才应分片。就像并非每个分片集合都需要存在于每个分片上，未分片集合也不需要在一个主分片上共同存在。事实上，通常最好将它们均匀分布在你的分片中，因为即使是未分片的数据，如果导致集群分布不均，也会引起问题。想象一下用于事件的小型每日集合，其中每天你将数据转储到一个单独的未分片集合中（最终会删除旧的集合）。如果所有天的数据都在同一个分片上，该分片最终会不堪重负，并非因为分片，而是因为未分片集合的不平衡。

在这些情况下，moveCollection 命令（从 MongoDB 8.0 版本开始支持）应该是你的首选工具。它允许你手动将整个未分片集合从一个分片移动到另一个分片。它不是自动的，如果你在活动集合上运行它，移动期间会短暂阻塞写入（通常只有几秒钟），但它可以在不经过将不需要分片的集合分片的复杂性的情况下重新平衡工作负载，从而成为救星。作为奖励，如果你预见到这一点并在首次创建集合时立即执行 moveCollection，移动空集合将非常快速且成本低。

在图 6.8 中，最初 testdb 数据库中未分片的 A-H 集合全部位于 shard0。我们可以运行以下命令将其中一些移动到 shard1，以在此集群中分布数据：

// Move unsharded collections to specific shards
// Useful for balancing unsharded data across the cluster
db.adminCommand({moveCollection:"testdb.E", toShard:"shard1"})
db.adminCommand({moveCollection:"testdb.F", toShard:"shard1"})
db.adminCommand({moveCollection:"testdb.G", toShard:"shard1"})
db.adminCommand({moveCollection:"testdb.H", toShard:"shard1"})

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_170_0.png)

*图 6.8：移动未分片集合*

## 将分片集合块共同定位

正如在基于区域的分片部分所述，有两种情况，为了性能考虑，禁用均衡器并手动（通过脚本）将两个或多个集合在相同的分片上共置，使用保持相关数据在一起的分片键范围可能是有益的。第一个能显著帮助的情况是多文档事务，其中单分片事务的性能明显优于多分片事务，另一个情况是两个集合之间使用 $lookup 操作的聚合，其中将相关范围保留在同一分片上允许管道本地执行，而无需将数据通过网络发送到另一个分片。

一个例子是 TPC-C 基准测试商务数据的情况，其中包含仓库、地区、客户和订单的集合。我们可以在这些集合中的每一个上使用范围分片，它们共享前缀以将相同的值定位在相同的分片上：

| 集合 | 分片键 |
|---|---|
| WAREHOUSE | w_id |
| DISTRICT | w_id, d_id |
| CUSTOMER | w_id, d_id, c_id |
| ORDERS | w_id, d_id, c_id, o_id |

*表 6.2：TPC-C 基准测试的集合及其分片键*

为了实现共置，我们将为这些集合禁用均衡器，然后在相同的前缀（w_id）值上拆分它们，并将具有相同值的块范围移动到相同的分片。或者，我们可以使用以下命令将相同前缀（w_id）值上的区域范围与同一区域关联，并让均衡器相应地拆分和移动块范围：

// Colocating sharded collection chunks together for TPC-C benchmark
sh.shardCollection("db.WAREHOUSE", {w_id:1})
sh.shardCollection("db.DISTRICT", {w_id:1, d_id:1})
sh.shardCollection("db.CUSTOMER", {w_id:1, d_id:1, c_id:1})
sh.shardCollection("db.ORDERS", {w_id:1, d_id:1, c_id:1, o_id:1})

// Associate each shard to a unique zone
sh.addShardToZone("shardA", "ZoneA")
sh.addShardToZone("shardB", "ZoneB")
sh.addShardToZone("shardC", "ZoneC")

// For each collection, associate the range with the same prefix to the same zone
sh.addTagRange("db.WAREHOUSE",{w_id:MinKey},{w_id:33},"ZoneA")
sh.addTagRange("db.DISTRICT", {w_id:MinKey,d_id:MinKey},
{w_id:33, d_id:MinKey},"ZoneA")
sh.addTagRange("db.CUSTOMER", {w_id:MinKey,d_id:MinKey,c_id:MinKey},
{w_id:33,d_id:MinKey,c_id:MinKey},"ZoneA")
sh.addTagRange("db.ORDERS", {w_id:MinKey,d_id:MinKey,c_id:MinKey,
o_id:MinKey}, {w_id:33, d_id:MinKey,c_id:MinKey,o_id:MinKey},"ZoneA")

sh.addTagRange("db.WAREHOUSE", {w_id:33},{w_id:66},"ZoneB")
sh.addTagRange("db.DISTRICT", {w_id:33,d_id:MinKey},
{w_id:66,d_id:MinKey},"ZoneB")
sh.addTagRange("db.CUSTOMER", {w_id:33,d_id:MinKey,c_id:MinKey},
{w_id:66,d_id:MinKey, c_id:MinKey},"ZoneB")
sh.addTagRange("db.ORDERS", {w_id:33,d_id:MinKey,c_id:MinKey,o_id:MinKey},
{w_id:66,d_id:MinKey,c_id:MinKey,o_id:MinKey},"ZoneB")

sh.addTagRange("db.WAREHOUSE", {w_id:66}, {w_id:MaxKey},"ZoneC")
sh.addTagRange("db.DISTRICT", {w_id:66,d_id:MinKey},
{w_id:MaxKey, d_id:MaxKey},"ZoneC")
sh.addTagRange("db.CUSTOMER", {w_id:66,d_id:MinKey,c_id:MinKey},
{w_id:MaxKey, d_id:MaxKey, c_id:MaxKey},"ZoneC")
sh.addTagRange("db.ORDERS", {w_id:66,d_id:MinKey,c_id:MinKey,o_id:MinKey},
{w_id:MaxKey,d_id:MaxKey,c_id:MaxKey,o_id:MaxKey},"ZoneC")

