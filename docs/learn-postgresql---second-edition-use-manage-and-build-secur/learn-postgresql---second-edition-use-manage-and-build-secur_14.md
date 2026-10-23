## 索引

## A

- 访问控制列表 (ACLs) 307, 323-327
  - 检查 345, 346
- 高级语句窗口函数 167
  - 框架子句 167
- 聚合函数 130-133
  - EXCEPT 运算符 135
  - INTERSECT 运算符 136
  - UNION ALL 运算符 133, 134
  - UNION 运算符 133, 134
- ALTER ROLE 语句 310
  - 按角色配置参数 312, 313
  - SESSION_USER 与 CURRENT_USER 对比 311, 312
  - 用于重命名现有角色 310
- ANALYZE 命令 492-494
- 任意精度数据类型 186, 188
- 归档和复制设置 586, 587
- 异步物理复制
  - 执行 614, 615
- 异步复制 609, 617-619
  - 副本，监控 619, 620
- 原子性、一致性、隔离性和持久性 (ACID) 3, 360
- 审计
  - 按角色 530-532
  - 按会话 528-530
  - 实现 524, 525
  - PgAudit，配置 527
  - PgAudit，安装 525
  - PostgreSQL，配置为 PgAudit 526
- auto-explain 494-498
- 自动 VACUUM 410-412
- autovacuum 工作进程 410

## B

- 备份 536
  - 优点 536
  - 缺点 537
- 平衡树 (B-Tree) 462
- 基础备份
  - 管理 678-681
- 基本语句窗口函数
  - CUME_DIST 165
  - DENSE_RANK 162
  - FIRST_VALUE 160
  - LAG 163
  - LAST_VALUE 161
  - LEAD 164
  - NTILE 165, 166
  - ORDER BY 子句 159
  - PARTITION BY 函数，使用 157, 158
  - RANK 161, 162
  - ROW_NUMBER 函数 159
  - WINDOW 子句，使用 157, 158
- 位图堆扫描 455
- 位图索引扫描 455
- 块范围索引 (BRIN) 463
- 布尔数据类型 183, 184

## C

- 级联复制 623-626
- CASE 语句 222, 223
- 字符数据类型 188
  - 定长数据类型 188, 189
  - 变长数据类型 190, 191
  - 无数据类型变长 191, 192
- 检查点
  - 手动发出 403
  - 节流 402, 403
- 检查点配置参数 399, 400
  - checkpoint_timeout 400, 401
  - max_wal_size 400, 401
- 检查点 398, 399
- 克隆集群 567, 568
- 集群 6, 10
  - 连接到 31
  - 连接字符串 39
  - 管理 22
  - pg_ctl 命令 22-28
  - PostgreSQL 进程 28-30
  - psql 命令行客户端 33, 35
  - 模板数据库 31-33
- 集群配置 574, 575
  - 配置上下文 580
  - 查找配置错误 578, 579
  - 配置文件，嵌套 579
  - 检查配置参数 576-578
  - 生成器 589-592
  - 从实时系统修改 588, 589
  - 设置 581
- 集群监控 592, 593
  - 检查数据库 596, 597
  - 检查索引 597, 598
  - 检查锁 594-596
  - 运行查询和会话信息 593, 594
  - 统计信息 599
  - 检查表 597, 598
- 列级权限 333-337
- 逗号分隔值 (CSV) 560
- 公共表表达式 (CTEs) 105, 460
  - 概念 145, 146
  - 探索 145
- 压缩 548, 549
- 条件语句 220
  - CASE 语句 222, 223
  - IF 语句 220, 221
- 配置上下文 580
  - backend 581
  - internal 581
  - postmaster 581
  - sighup 581
  - superuser 581
  - superuser-backend 581
  - user 581
- 配置错误
  - 查找 578, 579
- 配置文件
  - 探索 46-48
  - 嵌套 579
- 配置参数
  - 检查 576-578
- 连接字符串 39
- 上下文 580
- 连续备份
  - 管理基础备份 678-681
  - 管理 677
  - 检查 stanza 677
  - 创建 stanza 677
- 控制结构 219
- COPY 命令 559-563
- 基于成本的优化器 453
- 崩溃 397
- 崩溃恢复 397
- 交叉连接 118
- PostgreSQL 12 中的 CTE 146, 147
  - 递归 CTE 150, 151
  - 创建递归查询 149
  - 用例 147-149
- CUME_DIST 函数 165
- CURRENT_USER
  - 与 SESSION_USER 对比 311
- 自定义格式 539

## D

- DENSE_RANK 函数 162
- 派生
  - 用于安装 PostgreSQL 14, 15
- PostgreSQL 开发环境
  - 连接数据库 72, 73
  - 设置 72
- 目录格式 539
- PGDATA 的磁盘布局
  - 探索 42, 43
- 不同的条件
  - 使用 108-111
- Docker 容器
  - 使用 631, 632
- 转储格式 549-552

## E

- 生命周期结束 (EOL) 6
- 逻辑复制环境设置 636
  - pg_hba.conf 文件 639
  - 主服务器 637, 638
  - 副本角色 637
  - 副本服务器 638, 639
- 事件触发器 264
  - 创建 265
  - 示例 265, 267
- 异常处理语句 228
- EXCEPT 运算符 135
- 现有角色
  - 检查 58-61
  - 删除 57, 58
- EXISTS 条件 116
- EXISTS/IN
  - 与 INNER JOIN 对比 120
- EXPLAIN 语句 470-472
  - ANALYZE 模式 474, 475
  - 选项 476-479
  - 输出格式 473
- 显式事务
  - 与隐式事务对比 362-367
- 扩展
  - 创建 439
  - 示例，定义 439
  - 文件，创建 440, 441
  - 删除已安装扩展 437, 438
  - 使用已安装扩展 436, 437
  - 安装 442, 443
  - 手动安装 434, 435
- 扩展升级
  - 创建 443, 444
  - 执行 445

## F

- Fedora Linux
  - 用于安装 PostgreSQL 15, 16
- filenode 44
- 文件
  - 用于代替单个角色 66
- FIRST_VALUE 函数 160
- 定点精度数据类型 186
- 外部数据包装器
  - 使用 665, 666
- 框架子句 167
  - RANGE BETWEEN start_point and end_point 174-178
  - ROWS BETWEEN start_point and end_point 168-174
- FreeBSD
  - 用于安装 PostgreSQL 16

## I

- 索引
  - 删除 468
  - 检查 465-468
  - 使无效 469
  - 重建 470
  - 类型 462, 463
- 索引节点 455
- 仅索引扫描 455
- 索引扫描 455
- 信息模式 9
- INNER JOIN
  - 使用 119
  - 与 EXISTS/IN 对比 120
- INSERT 规则 235
  - ALSO 选项 236
  - 创建 243, 244
  - INSTEAD OF 选项 237-239
- 已安装扩展
  - 删除 438
  - 通过 pgxnclient 删除 439
- 整数类型 185
- INTERSECT 运算符 136
- 内省命令 39

## J

- JavaScript 对象表示法 (JSON) 201
- 连接
  - 使用 FULL OUTER JOIN 127, 128
  - 使用 INNER JOIN 119
  - INNER JOIN 与 EXISTS/IN 对比 120
  - 使用 LATERAL JOIN 129, 130
  - 学习 117, 118
  - 使用 LEFT JOIN 121-124
  - 使用 RIGHT JOIN 125, 126
- 连接节点 455, 456
- JSON 数据类型 201-205

## L

- LAG 函数 163
- 语言
  - 探索 205
- LAST_VALUE 函数 161
- LATERAL JOIN
  - 使用 129, 130
- LEAD 函数 164, 165
- LEFT JOIN
  - 使用 121-125
- like 条件
  - 使用 106, 107
- limit 条件
  - 使用 111, 112
- 列表分区 274
  - 示例 274
- 逻辑备份 536
  - 限制备份数据量 547, 548
  - 自动化备份 558, 559
  - 压缩 548, 549
  - COPY 命令 559-563
  - 转储格式 549-552
  - 探索 537, 538
  - 转储整个集群 555, 556
  - 并行备份 556-558
  - pg_restore 549-552
  - 执行选择性恢复 552-554
  - 转储单个数据库 539-543
  - 恢复单个数据库 543-547
- 逻辑复制 632-634
  - 禁用 651, 652
  - 环境设置 636
  - 与物理复制实例一起制作 652-657
  - 监控 641, 642





## M

- 只读，与可写对比 643-648
- 设置 639, 640
- 与物理复制对比 635
- **日志序列号 (LSN)** 397
- **循环语句** 225, 226
  - 记录类型 226, 227
- **手动编译扩展**
  - 移除 439
- **手动 VACUUM** 404-410
- **内存相关设置** 584
- **合并连接** 456
- **多版本并发控制 (MVCC)** 359, 373-379, 384-387
- **MySQL/MariaDB**
  - 迁移到 PostgreSQL
    - 使用 pgloader 684-688

## N

- **嵌套循环** 455, 486
- **网络相关设置** 585, 586
- **NEW 变量** 234, 235
- **按优化器分类的节点**
  - 成本 460, 461
  - 并行节点 457
  - 顺序节点 454
  - 工具节点 459
- **NoSQL 数据类型** 197
  - hstore 数据类型 198-201
  - JSON 数据类型 201-205
- **NOT EXISTS 条件** 116
- **NOT IN 条件** 113-115
- **NTILE 函数** 165, 166
- **数值数据类型** 184
  - 任意精度数据类型 186, 188
  - 定点精度数据类型 186

## O

- **对象标识符 (OID)** 44
- **对象**
  - 在 PGDATA 目录中 43-45
- **对象存储支持**
  - 使用 pgbackrest 675
- **偏移条件**
  - 使用 111, 112
- **OLD 变量** 234, 235
- **联机分析处理 (OLAP)** 590
- **联机事务处理 (OLTP)** 590
- **优化器**
  - 选择并行计划 458, 459
  - 设置 587
- **ORDER BY 子句** 159

## P

- **并行聚合** 458
- **并行备份** 556-558
- **并行哈希连接** 458
- **并行节点** 457
  - Gather 节点 457
  - 并行聚合 458, 459
  - 并行连接 458
  - 并行扫描 458
- **参数**
  - 探索 46-48

**Partial Aggregate 节点** 458

**PARTITION BY 函数**

使用 157, 158

**分区** 271, 292

基本概念 271, 272

案例研究 295-303

哈希分区 275

列表分区 274

范围分区 273

表继承 276-280

使用表空间 292, 293, 295

**分区维护** 288

将现有表附加到父表 290

附加分区 288

分离分区 289

**权限**

列级权限 333-337

授予 331

GRANT 语句 344

与模式中的对象相关 341

与数据库相关 343, 344

与语言相关 342

与例程相关 342, 343

与模式相关 339, 340

与序列相关 337, 338

与表相关 332

REVOKE 语句 344

撤销 331, 332

**PgAudit**

配置 527

安装 525

配置 PostgreSQL 526

**pgbackrest.conf 文件** 676

**使用 S3 Azure 的 pgbackrest 配置**

参考链接 675

**pgbackrest 工具**

配置 672

环境设置 669

功能 668

安装 671

交换公钥 669, 670

仓库 668

仓库配置 672-674

stanza 668

参考链接 668

用于灾难恢复 667

使用对象存储支持 675

**pgBadger**

调度 523, 524

**pg_basebackup 命令** 616

参考链接 616

**PGConfig**

参考链接 589

**pg_ctl 命令** 22-28

**PGDATA 目录** 8, 10

对象 43-45

表空间 45, 46

**pg env**

用于安装 PostgreSQL 18, 19

**pg_hba.conf 文件** 68, 639

规则顺序 64

语法 62, 63

**pg_hba.conf 规则**

检查 67, 68

**pgloader**

参考链接 684

用于从 MySQL/MariaDB 迁移到 PostgreSQL 684-687

**pg_restore** 549-552

pg_stat_replication
  参考链接 620
pg_stat_statements, 高级统计 600
  数据收集 601, 602
  重置数据收集 602
  安装扩展 600
  调优参数 602
pg_trgm 扩展
  探索 662-665
  参考链接 665
pgxnclient
  用于移除已安装扩展 439
物理备份 536, 537
  克隆集群 567, 568
  探索 563, 564
  执行手动物理备份
    564-566
  pg_verifybackup 566
物理复制
  与逻辑复制对比 635
物理复制实例
  用于制作逻辑复制 652-657
物理复制技术
  异步复制 609
  同步复制 609
纯文本格式 539
PL/pgSQL 函数 211
时间点恢复 (PITR) 537, 569, 661
  管理 681-683
策略 347
多态 SQL 函数 210, 211
postgres_fdw 扩展
  使用 665-667
POSTGRES (POST-Ingres) 4
PostgreSQL 3, 4
  安装组件 11
  配置为 PgAudit 526
  处理一致性 393
  设置开发环境 72
  历史 4, 5
  安装 10, 11
  从二进制包安装 12
  从源码安装 17
  在衍生版上安装 14, 15
  在 Fedora Linux 上安装 15, 16
  在 FreeBSD 上安装 16
  在 GNU/Linux Debian 上安装 14, 15
  在 Ubuntu 上安装 14, 15
  通过 pgenv 安装 18, 19
  使用 Docker 镜像安装 13, 14
  生命周期 6
  从 MySQL/MariaDB 迁移
    使用 pgloader 684-688
  处理持久性 393
  参考链接 4
  发布策略 5
  参考链接 661
  用户与组对比 52, 53
  版本号 6
  从零开始使用 75
PostgreSQL 15
  公共模式 74
PostgreSQL 16 5
PostgreSQL 客户端 11
postgresql.conf 文件 637, 675
PostgreSQL contrib 包 11
PostgreSQL 文档 11
PostgreSQL 函数
  编写 211, 212

PostgreSQL 全球开发组 (PGDG) 1
记录类型 226, 227
PostgreSQL PL/Perl 11
递归 CTE 150, 151
PostgreSQL PL/Python 11
递归查询
  创建 149
PostgreSQL PL/Tcl 11
副本角色 637
PostgreSQL 进程 28-30
副本服务器
  提升为主服务器 626, 627
PostgreSQL 服务器 11
INSERT 的 RETURNING 子句
PostgreSQL 服务器配置 675
  pgbackrest.conf 文件 676
  postgresql.conf 文件 675
  删除语句 145
  学习 140
PostgreSQL 术语
  探索 6-10
  MERGE 语句 142, 144
postmaster 10, 23
  更新多条记录 141, 142
主服务器
  返回查询元组 141
  提升副本服务器 626, 627
  更新语句 144
  以同步方式复制 621
RIGHT JOIN
  使用 125, 126
进程信息设置 585
psql 命令行客户端 33, 35
  通过 psql 输入 SQL 语句 35, 37
角色级别
  管理传入连接 61, 62
psql 命令 38, 39
角色密码加密 352
psql 方法
  使用 80
角色 52, 308
  ALTER ROLE 语句 310
  可用性 54, 55
  连接 54, 55
  继承 323
  从其他角色继承 316-319
  检查 313-315
  管理 53
  创建新角色 53
  密码 54, 55
  解析权限 319-322
  与新对象相关的属性 308
  与复制相关的属性 309
  与 RLS 相关的属性 309
  与超级用户相关的属性 309
  作为组使用 55-57
Q
查询调优
  示例 480-491
R
随机查找 396
范围分区 273
  示例 273
RANK 函数 161, 162
真实事务标识符 371, 373

行级安全 (RLS) 307, 309, 346-351
ROW_NUMBER 函数 159
规则 234
  探索 234
  在 DELETE 上管理 243
  在 INSERT 上管理 242
  在 UPDATE 上管理 242
S
保存点 387-390
模式 74
search_path 变量 75
安全套接层 (SSL)
  为集群配置 353
  连接到集群 354, 355
  连接 353
段 396
SELECT 语句
  探索 105, 106
半连接查询 117
顺序节点
  索引节点 455
  连接节点 455, 456
  顺序扫描 (Seq Scan) 454
  工具节点 454
顺序扫描 (Seq Scan) 454
SESSION_USER
  与 CURRENT_USER 对比 311, 312
设置，集群配置 581
  归档和复制设置 586, 587
  内存相关设置 584
  网络相关设置 585, 586
  优化器设置 587
进程信息设置 585
统计收集器 587, 588
vacuum 和 autovacuum 相关设置 587
WAL 设置 582, 583
Slony
  参考链接 633
槽技术 616
SQL 38
SQL 函数 206
  返回元素集合 207, 208
  返回表 208, 209
SQL 方法 81
SQL 语句
  通过 psql 输入 35, 37
STABLE 函数 217
标准数据类型 182, 183
备用服务器
  以同步方式复制 621, 622
语句执行 452
  按优化器分类的节点 454
  优化器 452, 453
统计
  更新 491-494
统计收集器 587, 588
流复制 612, 613
  准备环境设置 610-612
  管理 612
子查询
  EXISTS 条件 116
  IN 条件 113-115
  NOT EXISTS 条件 116
  NOT IN 条件 113-115
  使用 112





synchronous replication 609, 620
  cascading replication 623-626
  delayed replication 626
synchronous replication, PostgreSQL settings 621
  primary server 621
  standby server 621, 622

## T

table inheritance 276-280
  tables, dropping 280
table manipulation statements 90
  data, deleting 99, 100
  data, inserting 90-93
  data, selecting 90-93
  data, updating 98, 99
  NULL values 94, 95
  NULL values, sorting with 96, 97
  table, creating from another table 97, 98
tables
  creating 89, 90
  dropping 78
  EXISTS option 85, 86
  listing 76
  managing 84, 85
  temporary tables, managing 86-88
  unlogged tables, managing 88
tablespaces 45, 46
tar format 539
template databases 31-33
timeline 397
timestamp data type 195, 197
transaction
  time within 368, 369
transaction identifiers 361, 369, 370
transaction isolation levels 380
  dirty read 379
  phantom read 379
  read committed 379, 381
  read uncommitted 379, 381
  repeatable read 380, 381
  serializable 380-383
  unrepeatable read 379
transactions 360-362
triggers
  managing 249, 250
  on INSERT 252-256
  on UPDATE / DELETE 257-264
  TG_OP variable 257
  trigger syntax 250, 251
tuple freezing 370

## U

Ubuntu
  used, for installing PostgreSQL 14, 15
UNION ALL operator 133, 134
UNION operator 133, 134
UPDATE rules 240
  creating 247-249
  new_tags table, creating 240
  tables, creating 241
UPSERT statement
  using 137
  using, in PostgreSQL 137-139
user groups 52, 53
users 52, 53
  normal users 74
  superusers 74
utility nodes 459

## V

VACUUM 403

automatic VACUUM 410-412

manual VACUUM 404-410

vacuum and autovacuum-related settings 587

virtual transaction identifier 371, 373

VOLATILE function 217

## W

WAL archiving 569

wal_keep_segments option 615

wal_level directive 610

reference link 610

WAL-replay 397

WAL segments 609

WAL settings 582, 583

WindowAgg node 460

WINDOW clause

using 157, 158

window functions 155

advanced statement window functions 167

basic statement window functions 156, 157

Write-Ahead Logs (WALs) 8, 10, 29, 359, 393-397, 563

as rescue method, in event of crash 397, 398

checkpoints 398, 399

## X

xid 361

XID wraparound problem 369, 370

# Download a free PDF copy of this book

Thanks for purchasing this book!

Do you like to read on the go but are unable to carry your print books everywhere? Is your eBook purchase not compatible with the device of your choice?

Don’t worry, now with every Packt book you get a DRM-free PDF version of that book at no cost.

Read anywhere, any place, on any device. Search, copy, and paste code from your favorite technical books directly into your application.

The perks don’t stop there, you can get exclusive access to discounts, newsletters, and great free content in your inbox daily

Follow these simple steps to get the benefits:

+   1. Scan the QR code or visit the link below

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_742_0.png)

https://packt.link/free-ebook/9781837635641

+   2. Submit your proof of purchase

+   3. That’s it! We’ll send your free PDF and other benefits to your email directly

