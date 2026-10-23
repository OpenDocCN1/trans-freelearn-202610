

然而，有一点很重要需要注意：`txid_current()` 函数的使用总是会物化一个 xid，即使事务尚未获得一个。为此，PostgreSQL 提供了另一个名为 `txid_current_if_assigned()` 的 introspection 函数，如果事务仍处于虚拟 xid 空间中，并且因此尚未执行任何可写操作，则该函数返回 NULL。需要注意的是，除非事务操作了某些数据，否则 PostgreSQL 不会分配真实的 xid，这可以通过以下工作流程轻松证明：

```
forumdb=> BEGIN;
BEGIN

forumdb=> SELECT txid_current_if_assigned();
txid_current_if_assigned
-----------------------------

(1 row)

forumdb=> SELECT count(*) FROM tags;
 count
-------
     7

(1 row)

forumdb=> SELECT txid_current_if_assigned();
txid_current_if_assigned
-----------------------------

(1 row)

forumdb=> UPDATE tags SET tag = upper( tag );
UPDATE 7
forumdb=> SELECT txid_current_if_assigned();
txid_current_if_assigned
-----------------------------
       4837
```

```
(1 row)

forumdb=> SELECT txid_current();
 txid_current
---------------
       4837

(1 row)

forumdb=> ROLLBACK;
ROLLBACK
```

在事务开始时，没有分配 xid，事实上，`txid_current_if_assigned()` 返回 NULL。即使在数据读取（即 SELECT）之后，xid 也未被分配。然而，一旦事务执行某些写活动（例如 UPDATE），xid 就会被分配，并且 `txid_current_if_assigned()` 和 `txid_current()` 的结果相同。

## 多版本并发控制

如果两个事务，无论是隐式还是显式，尝试对同一数据执行冲突的更改会发生什么？PostgreSQL 必须确保数据始终一致，因此，它必须有一种锁定（即阻塞和保护）受冲突更改影响的数据的方法。锁是一种沉重的机制，会限制系统的并发性：锁越多，事务获取锁的等待时间就越长。为缓解此问题，PostgreSQL 实现了 MVCC，这是一种在企业级数据库中广泛使用的技术。

MVCC 规定，系统不得在数据库中修改现有元组，而必须复制元组，应用更改并使原始元组失效。你可以将此与 ZFS 等操作系统文件系统中使用的方法复制时写入进行比较。

为了更好地理解这意味着什么，让我们假设 categories 表有三个元组，并且我们更新其中一个，以更改其描述。

发生的情况是，一个源自我们要应用 UPDATE 的元组的新元组被插入到表中，并且原始元组被使无效：

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_406_0.png)

图 11.1：更新元组会创建新元组并使旧元组失效

为什么 PostgreSQL 和 MVCC 要处理这项额外的工作，而不是对元组进行原地更新？原因在于，这样数据库可以处理同一元组的多个版本，并且每个版本在特定的时间窗口内有效。这意味着修改数据所需的锁更少，因为数据库能够同时处理同一数据的多个版本，并且不同的事务可能会看到不同的值。

为了使 MVCC 正常工作，PostgreSQL 必须处理快照的概念：快照表示某个事务被允许感知数据的时限。快照从根本上说是定义当前事务可用数据边界的事务 xid 范围：数据库中标记有该范围内 xid 的每一行都可被当前事务感知和使用。换句话说，每个事务都会看到数据库中所有可用数据的一个专用子集。MVCC 机制以及决定元组是否可见的测试比上述描述复杂得多，但其核心思想如上所述。

特殊的函数 `txid_current_snapshot()` 返回定义当前事务时间边界的最小和最大事务标识符。使用几个并行会话很容易演示该概念。

在第一个会话中，让我们运行一个显式事务，提取标识符和快照以供将来参考，并执行一个操作：

```
-- session 1
```

```
```sql
forumdb=> BEGIN;
BEGIN

forumdb=> SELECT txid_current(), txid_current_snapshot();
 txid_current | txid_current_snapshot
--------------+-----------------------
         4928 | 4928:4928:

(1 row)

forumdb=> UPDATE tags SET tag = lower( tag );
UPDATE 5
```
```

正如前面的示例所示，事务编号为 4928，其快照仅限于自身，这意味着事务将看到数据库中已合并的所有内容。

现在，让我们暂停此会话片刻并打开另一个连接到同一数据库的会话 - 执行一个包装在隐式事务中的单个 INSERT 语句并获取其 xid 的信息：

```
```sql
forumdb=> INSERT INTO tags( tag ) VALUES( 'KDE' ) RETURNING txid_current();
 txid_current
--------------
         4929

(1 row)
```
```

单次事务已被分配 xid 4929，这当然是前一个显式事务之后可用的下一个 xid（系统未运行其他并发事务以使编号更易于跟踪）。

返回第一个会话，并再次检查事务快照的信息：

```
```sql
-- session 1
forumdb=> SELECT txid_current(), txid_current_snapshot();
 txid_current | txid_current_snapshot
--------------+-----------------------
         4928 | 4928:4930:

(1 row)
```
```

这次，事务将其快照从自身扩展到事务 4930，而事务 4930 尚未开始（`txid_current_snapshot()` 将其上界报告为非包含）。换句话说，当前事务现在可以看到从在其之后开始的事务 4929 合并的数据。这如果事务查询表会更明确：

```
-- session 1
forumdb=> SELECT xmin, tag FROM tags;
    xmin    |  tag
------------+-------
  4928 | linux
  4928 | bsd
  4928 | java
  4928 | perl
  4928 | raku
  4929 | KDE
(6 rows)
```

正如你所见，除了最后一个之外，所有元组都是由当前事务生成的，最后一个是由 xid 4929 生成的。但前一个事务只是故事的一部分；在第一个事务仍未完成时，让我们从另一个并行会话检查同一表：

```
forumdb=> SELECT xmin, tag FROM tags;
    xmin    |  tag
------------+-------
  4922 | linux
  4923 | BSD
  4924 | Java
  4925 | Perl
  4926 | Raku
  4929 | KDE
(6 rows)
```

除了最后一个元组之外，所有元组都有不同的描述，最值得注意的是 xmin 的值与事务 4928 看到的不同。这意味着什么？这意味着虽然表经历了几乎所有元组的完全重写（除了最后一个元组之外的所有元组的 UPDATE），但其他并发事务仍然可以访问表中的数据，而不会被锁阻塞。这就是 MVCC 的本质：每个事务都会感知存储的不同视图，并且视图的有效性取决于与事务关联的时间窗口（快照）。

迟早，存储上的数据必须被合并，因此，当事务 4928 完成其工作的 COMMIT 时，表中的数据将成为从那时起每个事务都会感知的事实：

```
-- session 1
forumdb=> COMMIT;
COMMIT

-- out from the transaction now
-- we all see consolidated data
forumdb=> SELECT xmin, tag FROM tags;
 xmin | tag
-------+------
4928 | linux
4928 | bsd
4928 | java
4928 | perl
4928 | raku
4929 | KDE
(6 rows)
```

MVCC 并不能总是防止使用锁：如果两个或多个并发事务开始操作同一组数据，系统必须应用有序的更改，因此必须强制对每个并发事务进行锁定，以便只有一个可以继续。使用与前面类似的两个并行会话很容易证明这一点：

```
-- session 1
forumdb=> BEGIN;
BEGIN
forumdb=> SELECT txid_current(), txid_current_snapshot();
 txid_current | txid_current_snapshot
---------------+------------------------
 4930 | 4930:4930:
(1 row)

forumdb=> UPDATE tags SET tag = upper( tag );
UPDATE 6
```

