

```
-- or you can query the pg_settings
forumdb=> SELECT name, setting, unit
    FROM pg_settings
    WHERE name IN ( 'checkpoint_timeout', 'max_wal_size' );
    name              | setting | unit
----------------------+---------+------
 checkpoint_timeout | 300     | s
 max_wal_size       | 1024    | MB
(2 rows)
```

在300秒（5分钟）之后会触发检查点，除非在此期间已经产生了1,024 MB的WAL数据。这两个参数的目标是保证连续两次检查点之间既不会间隔太长时间，也不会有太多已变更的数据，从而缩小内存中数据状态与磁盘上安全存储状态之间的差异。

由于WAL段由写入事务填充，设置`max_wal_size`与数据库中被更改的用户数据量成正比。写入WAL的数据量并不完全等于用户数据产生量，因为WAL段还存储校验和，以及在恢复或副本情况下对数据库有用的其他信息，但你可以近似认为产生的WAL量反映了数据库中已变更的数据量。

因此，`max_wal_size`的含义是：一旦数据库产生了如此多的数据更改，就会触发检查点，并将内存中的状态反映并同步到磁盘。另一方面，如果数据库不够繁忙，产生的数据不足以达到`max_wal_size`，则会触发`checkpoint_timeout`，这意味着即使负载很低，也会在特定时间后同步。

在上方所示的默认配置中，每5分钟或每产生1 GB新（WAL）数据就会执行一次检查点。

请注意，只有在自上次检查点以来情况发生变化时才会执行检查点；在极少数情况下，如果数据库没有产生任何新数据（即完全没有负载），即使`checkpoint_timeout`时间已过，也不会触发检查点。实际上，如果自上次检查点以来没有任何数据更改，就没有什么可同步的，因此检查点也就根本无需执行任何工作。

## 检查点节流

检查点无需急于完成：如果系统没有将所有内存中的数据同步到磁盘，并且发生故障，系统将把所有WAL段重放到上一个已完成的检查点。基于这一考虑，PostgreSQL提供了一个配置参数，用于指示系统完成检查点的速度。

为了避免检查点执行期间出现I/O尖峰，PostgreSQL使用`checkpoint_completion_target`，其值可介于0和1之间。该参数表示检查点可以延迟写入脏缓冲区的时长。具体来说，完成检查点的时间计算为`checkpoint_timeout x checkpoint_completion_target`。

例如，如果将`checkpoint_completion_target`设置为0.2，`checkpoint_timeout`为300秒，则系统将有60秒时间写入所有数据。系统会校准所需的存储I/O带宽，以完成脏缓冲区的写入。

`checkpoint_completion_target`设置得越接近0，在检查点执行时出现的I/O尖峰就越明显，从而导致I/O带宽占用较高；另一方面，将该参数设置为接近1会避免I/O尖峰，代之以低带宽消耗的持续I/O活动。下图说明了上述概念：

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_434_0.png)

默认情况下，`checkpoint_completion_target`设置为0.9，这意味着检查点将尽可能缓慢地完成，以避免I/O尖峰和资源消耗：

```
forumdb=> show checkpoint_completion_target;
checkpoint_completion_target
-----------------------------
0.9
(1 row)
```

## 手动触发检查点

集群管理员随时可以手动启动检查点进程：PostgreSQL的CHECKPOINT语句会启动通常由`checkpoint_timeout`或`max_wal_size`触发的所有活动。

既然检查点是一种侵入性较强的操作，为什么还要有人手动执行它呢？一个原因可能是在开始流复制或文件级备份之前，确保磁盘上的所有数据都已同步。

在下一节中，你将了解VACUUM进程——这种技术让PostgreSQL能够回收未使用的空间，移除不再可见的元组。

## VACUUM

在前面的章节中，你了解了PostgreSQL如何利用MVCC存储同一数据（元组）的不同版本，这些版本可由不同事务根据其活动快照来感知。然而，相对于最新的活动版本，保留相同元组的不同版本需要额外空间，而这些空间迟早会填满你的存储。为了防止这种情况并回收存储空间，PostgreSQL提供了一个名为VACUUM的内部工具，其目标是分析存储的元组版本，并移除那些不再可感知的版本。

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_435_0.png)

> 请记住：当不再有任何活动事务可以引用该版本（即该元组版本位于其快照中）时，该元组就是不可感知（不可见）的。不可感知的元组通常被称为死元组，表示在数据库生命周期中它已不再需要。

VACUUM可能是一项I/O密集型操作，因为它必须回收和释放磁盘空间，因此可能是一种侵入性操作。为此，你不应过于频繁地手动运行VACUUM，PostgreSQL还提供了一个名为autovacuum的后台任务，它可以根据当前数据库活动替你运行VACUUM。

以下小节将向你展示手动和自动VACUUM。

## 手动VACUUM

手动VACUUM可以针对单个表、表列的子集或整个数据库运行，其概要如下：

```
VACUUM [ FULL ] [ FREEZE ] [ VERBOSE ] [ ANALYZE ] [ table_and_columns [, ...] ]
```

VACUUM主要有三种版本，它们执行逐步更强力的数据重构：

- *普通* VACUUM（默认）进行轻微的空间回收，这意味着它会丢弃死元组版本，但不会对表进行碎片整理，因此最终效果是不会回收空间。
- VACUUM FULL 会重写整个表，丢弃死元组并消除碎片，从而也积极回收磁盘空间。
- VACUUM FREEZE 将已整合的元组标记为冻结，以防止 xid 回卷问题。

VACUUM不能在一个事务中执行，也不能在函数或过程中执行。附加选项VERBOSE和ANALYZE分别提供详细输出并执行表内容的统计信息更新（这对于性能提升很有用）。

为了查看VACUUM的效果，让我们构建一个简单的示例。首先，确保autovacuum已设置为off。如果没有，请编辑`$PGDATA/postgresql.conf`配置文件，将该参数设置为off，然后重启集群。之后，检查tags表的大小：

```
forumdb=> SHOW autovacuum;
 autovacuum
-------------
 off
(1 row)
```

```
forumdb=> SELECT relname, reltuples, relpages, pg_size_pretty( pg_relation_size( 'tags' ) )
FROM pg_class WHERE relname = 'tags' AND relkind = 'r';
 relname | reltuples | relpages | pg_size_pretty
---------+-----------+----------+----------------
 tags    |         6 |        1 | 8192 bytes
(1 row)
```

如你所见，该表只有六个元组，在磁盘上仅占用一个大小为8 KB的数据页。现在，让我们向表中填充大约100万个随机元组：

```
forumdb=> INSERT INTO tags( tag )
SELECT 'FAKE-TAG-#' || x
FROM generate_series( 1, 1000000 ) x;
INSERT 0 1000000
```

由于我们已停止autovacuum，PostgreSQL不知道表的真实大小，因此我们需要执行手动ANALYZE，告知集群表中新增的数据：

```
forumdb=> ANALYZE tags;
ANALYZE
forumdb=> SELECT relname, reltuples, relpages, pg_size_pretty( pg_relation_size( 'tags' ) )
FROM pg_class WHERE relname = 'tags' AND relkind = 'r';
 relname | reltuples | relpages | pg_size_pretty
---------+-----------+----------+----------------
 tags    | 1.00001e+06 |     6370 | 50 MB
```

现在是时候使我们插入的所有元组失效了，例如，使用UPDATE覆盖它们（由于MVCC，这将使元组产生副本）：

```
forumdb=> UPDATE tags SET tag = lower( tag ) WHERE tag LIKE 'FAKE%';
UPDATE 1000000
```

该表现在仍然有大约100万个有效元组，但大小几乎翻倍，因为每个元组现在都有两个版本，其中一个已死亡：

```
forumdb=> ANALYZE tags;
ANALYZE
forumdb=> SELECT relname, reltuples, relpages, pg_size_pretty( pg_relation_size( 'tags' ) )
```

