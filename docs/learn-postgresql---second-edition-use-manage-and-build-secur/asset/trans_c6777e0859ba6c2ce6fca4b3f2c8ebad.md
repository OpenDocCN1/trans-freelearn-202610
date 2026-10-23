

日志轮转很有用，因为它允许你生成更小的日志文件，这些文件可以限定在特定的时间段内。一方面，这会将日志分散到多个（可能很小）的文件中；另一方面，它不会产生一个单一的（可能巨大的）日志文件，而这可能导致分析问题。

一旦你启用了日志收集器，就必须对其进行配置，以便它按照你想要的方式和位置存储日志。实际上，你可以使用以下参数来配置日志收集器进程，只需在 PostgreSQL 配置文件中为以下任一设置放置正确的值即可：

- log_directory：这是必须存储各个日志文件的目录。它可以是相对于 PGDATA 的相对路径，也可以是绝对路径（进程必须能够写入该路径）。显然，它必须是运行集群的操作系统用户具有写访问权限的路径。
- log_filename：这是指定每个日志文件（位于 log_directory 内）名称的单个文件名或模式。模式可以按照 strftime(3) 指定，以便使用日期和时间进行格式化。例如，值 postgresql-%Y-%m-%d.log 将生成带有日期（分别为年、月、日）的日志文件名，例如 postgresql-2022-07-19.log。
- log_rotation_age：这表示日志在应用自动日志轮转之前应等待的时间。例如，1d 表示 1 天，并指定日志将每天轮转一次。
- log_rotation_size：这指定日志文件在轮转到新文件之前的大小。例如，50MB 表示日志文件在达到 50 MB 大小时将被轮转。
- log_truncate_on_rotation：此布尔参数决定 PostgreSQL 在轮转时是否必须截断（即清空并重新开始）现有文件，或改为将新日志数据追加到现有文件。

请注意，日志轮转并不精确：日志文件可能会略微超过轮转大小或年龄，具体取决于集群的日志活动。

所有与轮转相关的设置都需要 logging_collector 打开：毕竟，只有 PostgreSQL 负责日志记录时，它才能管理轮转。

`postgresql.conf` 文件中的示例日志配置可能如下所示：

```conf
logging_collector  = on
log_destination    = 'stderr,csvlog,jsonlog'
log_directory      = 'log'
log_filename       = 'postgresql-%Y-%m-%d.log'
log_rotation_age   = '1d'
log_rotation_size  = '50MB'
```

使用上述设置，集群将在 log 目录（相对于 PGDATA）内为每天（或 50 MB 的信息）生成一个新的日志文件，使用日志收集器，并且每个日志文件都将标明其创建的年、月、日。请注意，作为示例，系统将同时以文本、JSON 和 CSV 格式生成日志；PostgreSQL 会智能地将后两种格式的日志文件名扩展名分别更改为 `.json` 和 `.csv`。

使用上述日志配置（也在本章的 Docker 镜像中使用），检查日志目录将产生类似于以下内容的输出：

```sh
$ ls -1 /postgres/16/data/log
postgresql-2023-07-19.csv
postgresql-2023-07-19.json
postgresql-2023-07-19.log
...
```

`.log` 文件是纯文本格式的文件，而另外两个文件具有 CSV 和 JSON 格式的条目。

## 何时记录日志

决定何时在日志中报告事件非常重要。有很多选项可以控制日志操作的触发方式，通过阈值指定。日志阈值可以采用助记值，表示日志事件将被插入日志的最小值。

最常见的值按顺序为 info、notice、warning、error、log、fatal 和 panic，其中 info 为最小值，fatal 为最高值。

例如，如果你决定 warning 是你想要接受的最小阈值，则每个具有更低阈值的日志事件（例如 info 和 notice）都不会被插入到日志中。

正如你所见，阈值随着向 error 值（如 fatal 和 panic）移动而增加，这些值始终会自动记录，因为它们代表不可恢复的问题。还有最低级别的 debug1 到 debug5，用于获取开发信息和有关进程执行的内部细节（即，它们通常在用 PostgreSQL 开发时使用）。

因此，集群将在不同时间产生不同日志事件，并且所有事件都具有不同的优先级，而这些事件将根据你配置的阈值插入到日志中。

特别是，有两个参数可用于调整日志阈值：`log_min_messages` 和 `client_min_messages`。

前者 `log_min_messages` 决定日志系统的阈值，而后者决定每个新用户连接的阈值。它们有何不同？

`log_min_messages` 指定集群必须插入到日志中的内容，而不考虑传入的用户连接及其设置。`client_min_messages` 决定客户端在连接期间必须向用户报告哪些日志事件。这两个设置都可以采用上述阈值列表中的值。

开发或测试环境的一个典型用例可能是以下内容：

```text
log_min_messages    = 'info'
client_min_messages = 'debug1'
```

使用上述配置，集群将在文本日志中仅记录 info 消息，这与进程的正常执行有关，而传入的用户连接将向用户报告更详细的开发等消息。

设置阈值并不是决定何时触发日志插入的唯一方法：还有另外几个设置可用于处理语句和实用程序的持续时间。

如果你有兴趣记录客户端执行的语句（即查询），则有以下日志参数可供调整：

- `log_min_duration_statement` 包含一个整数值，表示毫秒数。每个耗时超过设定值的语句都将被记录。因此，将此值设置为 0 意味着系统中发生的每个语句都将被转储到日志中。
- `log_min_duration_sample` 和 `log_statement_sample_rate` 是协同工作的参数。`log_min_duration_sample` 处理毫秒数，并仅记录运行时间远长于毫秒值语句的样本。换句话说，它的工作方式与 `log_min_duration_statement` 类似，但不是记录每个语句，而是仅记录其中一部分。要记录的语句比例由 `log_statement_sample_rate` 决定，后者处理 0 到 1 之间的值。
- `log_transaction_sample_rate` 是 0 到 1 之间的值，表示有多少事务将被完全记录（即事务的每个语句都将出现在日志中），而不考虑语句持续时间。

样本参数背后的想法是减少日志活动（和大小）的数量，同时仍提供有关集群中发生情况的有用见解。

为了更好地理解上述参数，请考虑以下配置：

```
log_min_duration_statement = 500
log_min_duration_sample = 100
log_statement_sample_rate = 0.8
log_transaction_sample_rate = 0.5
```

上述配置将记录运行时间超过 500 毫秒的每个语句（`log_min_duration_statement`）以及运行时间超过 100 毫秒的每个语句中的 80%（`log_min_duration_sample` 和 `log_statement_sample_rate`）。最后，它将记录每两个事务中的一个事务（`log_transaction_sample_rate`）。

你可以使用以下简单工作负载进行测试：

```
forumdb=> BEGIN;
BEGIN
forumdb=> SELECT 'transaction 1';
 ?column?
-------------
 transaction 1
(1 row)
forumdb=> ROLLBACK;
ROLLBACK
forumdb=> BEGIN;
```
