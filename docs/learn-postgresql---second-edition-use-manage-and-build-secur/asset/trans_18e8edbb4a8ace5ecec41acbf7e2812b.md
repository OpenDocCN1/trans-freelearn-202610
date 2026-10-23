

```
BEGIN
forumdb=> SELECT 'transaction 2';
    ?column?
-------------
 transaction 2
(1 row)

forumdb=> ROLLBACK;
ROLLBACK
forumdb=> SELECT pg_sleep( 2 );
 pg_sleep
---------

(1 row)
forumdb=> BEGIN;
BEGIN
forumdb=> SELECT pg_sleep( 0.120 );  --repeat 10 times
 pg_sleep
---------

(1 row)
forumdb=> ROLLBACK;
ROLLBACK
```

在日志中，你会发现类似如下的内容：

```
LOG:  duration: 0.047 ms  statement: BEGIN;
LOG:  duration: 0.253 ms  statement: SELECT 'transaction 2';
LOG:  duration: 0.068 ms  statement: ROLLBACK;
LOG:  duration: 2003.742 ms  statement: SELECT pg_sleep( 2 );
LOG:  duration: 121.593 ms  statement: SELECT pg_sleep( 0.120 );
LOG:  duration: 121.464 ms  statement: SELECT pg_sleep( 0.120 );
LOG:  duration: 120.459 ms  statement: SELECT pg_sleep( 0.120 );
LOG:  duration: 121.448 ms  statement: SELECT pg_sleep( 0.120 );
LOG:  duration: 121.455 ms  statement: SELECT pg_sleep( 0.120 );
LOG:  duration: 121.416 ms  statement: SELECT pg_sleep( 0.120 );
```

根据 `log_transaction_sample_rate` 参数，只有两个事务中的一个被记录下来。还要注意 `pg_sleep(2)` 被插入是因为它花费的时间超过了 500 毫秒（`log_min_duration_statement`），以及对 `pg_sleep(0.120)` 的 10 次调用中有 6 次被插入，因为 `log_transaction_sample_rate` 设置为 0.8（即 80% 的运行事务）。你可以注意到 `log_transaction_sample_rate` 并不是一个精确的值：即使配置告诉 PostgreSQL 记录 80% 的查询，系统记录的却更少（60%）。

## 要记录的内容

要记录的信息质量由一组丰富的参数配置，通常是布尔值，用于打开或关闭特定事件的记录。

一个被广泛使用且被滥用的设置是 `log_statement`：如果打开，它将记录来自每个连接对集群执行的所有语句。这可能非常有用，因为它允许你重建数据库的确切操作以及使用的语句，但另一方面，它也可能非常危险。记录每个语句可能会使私有或敏感数据出现在日志中，而日志因此可能被未经授权的人获取。此外，记录所有语句可能会迅速填满日志存储，特别是在集群处于高负载和高并发的情况下。

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_544_0.png)

> 通常，配置 `log_min_duration_statement` 设置来仅记录“慢”语句，比记录所有语句更有用。

可以通过 `log_statement` 来微调要记录的语句类别：该设置可以取 off、ddl、mod 或 all 的值。off 和 all 的含义很容易理解，但 ddl 意味着所有数据定义语言语句（例如 CREATE TABLE、ALTER TABLE 等）都会被记录，而 mod 意味着所有数据操作语句（例如 INSERT、UPDATE 和 DELETE）都会被记录。日志类别各自是前一个的超集，因此 mod 也包括 ddl，而 all 包括前者并允许记录 SELECT 类型语句。值得注意的是，如果语句包含语法错误，无论设置如何，都不会通过 `log_statement` 记录。

日志中的信息质量也由 `log_line_prefix` 参数确定。`log_line_prefix` 是一个模式字符串，用于定义在每条日志行开头插入的内容，因此可用于详细说明所记录的事件。该模式与 `sprintf(3)` 相同的方式使用一些占位符，在本书的范围内记录每个选项是不合适的。

- log_lock_waits：此布尔参数表示每次用户会话等待获取锁的时间过长时都应创建日志条目。阈值是配置参数 deadlock_timeout。

现在我们已经了解了所有关于日志记录的内容，接下来我们将使用一个名为 pgBadger 的特殊工具，从创建的日志中提取信息。

## 从日志中提取信息 — pgBadger

由于日志中可以包含丰富的各种信息，因此可以自动化日志信息分析和提取。有多个工具旨在实现此目的，其中最受欢迎和最完整的是 pgBadger。

pgBadger 是一个自包含的 Perl 5 应用程序，它仔细读取并从 PostgreSQL 日志中提取信息，生成一个包含其在日志中发现的所有信息的汇总的 Web 仪表板。该应用程序的目标是为你提供更有用的日志洞察，而无需手动搜索特定信息。

使用 pgBadger 并非强制；你的集群在没有它也能正常工作，你仍然能够在日志中查找信息和问题。然而，使用 pgBadger 可以为你提供有关服务器所做操作的更有用提示。

需要注意的是，使用 pgBadger 以及进行任何自动或手动日志分析，都不会提供实时信息，而只是对服务器过去活动的回顾。

在以下小节中，你将学习如何安装和使用 pgBadger。

## 安装 pgBadger

pgBadger 需要在运行的系统上安装 Perl 5，这是它唯一的依赖项。你可以在 PostgreSQL 集群运行的同一主机上运行 pgBadger，也可以在远程系统上运行（将在后面的小节中展示）。在本节中，我们假设 pgBadger 将安装并在与 PostgreSQL 集群相同的机器上执行。

安装 pgBadger 最简单的方法是通过操作系统的包管理器，例如在 GNU/Debian 和基于 Ubuntu 的系统上如下：

```shell
$ sudo apt install pgbadger
```

也可以通过以下步骤从源码安装 pgBadger：

```shell
$ wget https://github.com/darold/pgbadger/archive/v12.0.tar.gz
$ tar xzvf v12.0.tar.gz
$ cd pgbadger-12.0
$ perl Makefile.PL
$ make
$ sudo make install
```

安装 pgBadger 后，你可以通过输入以下命令测试它是否正常工作：

```shell
$ pgbadger --version
pgBadger version 12.0
```

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_547_0.png)

## 为 pgBadger 使用配置 PostgreSQL 日志记录

pgBadger 足够智能，可以在许多情况下理解 PostgreSQL 日志，但在某些情况下，你需要指定一些配置选项，以便 PostgreSQL 生成更易于理解的日志。

首先，pgBadger 需要访问 PostgreSQL 日志，这意味着你应该使用 `logging_collector` 来生成日志。如果你更改 `log_line_prefix`，你应该将相同的配置设置传递给 pgBadger，以便它能够正确解析日志前缀。最后，你应该启用尽可能多的日志上下文。

以下是使 PostgreSQL 生成 pgBadger 能够正确理解的日志的配置参数示例：

```conf
logging_collector  = on
log_destination   = 'stderr,csvlog,jsonlog'
log_directory     = 'log'
log_filename      = 'postgresql-%Y-%m-%d.log'
log_rotation_age  = '1d'
log_rotation_size = '50MB'
```

现在是第一次启动 pgBadger 的时候：

```
$ pgbadger -o /data/html/first_report.html \
    /postgres/16/data/log/postgresql-2023-07-19.log
[====================>] Parsed 261891612 bytes of 261891612 (100.00%),
queries: 1428472, events: 27
LOG: Ok, generating html report...
```

第一个参数 -o 指定我们希望存储报告的文件名。pgBadger 每次运行只生成一个文件，因此如果你想生成另一份报告而不覆盖现有报告，需要更改文件名。

第二个参数是要分析的 PostgreSQL 日志文件；你也可以指定 JSON 或 CSV 文件，pgBadger 会相应地解析它们。

程序运行几秒钟，或根据日志文件大小运行几分钟，并报告它在日志文件中找到的一些统计信息（在此示例中为 140 万条语句）。你可以很容易地检查生成的报告文件：

> 如果你要分析大日志文件或多个文件，可以使用 pgBadger 的并行模式，使用 -j 选项后跟要启动的并行进程数。例如，传递 -j 4 意味着每个日志文件将被分成四部分，每部分由一个单独的进程分析。得益于并行性，你可以利用机器的所有核心，更快地获得大量日志的结果。

```
$ ls -l -s /data/html/first_report.html
1172 /data/html/first_report.html
```

