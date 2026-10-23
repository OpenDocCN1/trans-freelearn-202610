

# 检查所有配置参数

你可以通过对 pg_settings 特殊目录发出查询来检查来自实时系统的所有配置参数。该目录包含当前版本的 PostgreSQL 将接受的每个设置，以及默认值、当前值、参数目的的描述等等。

作为示例，通过以下查询，你可以收集有关每个配置参数的信息，包括它们的简短和长描述、默认值和当前值：

```
forumdb=> SELECT name, setting || ' ' || unit AS current_value, short_desc, extra_desc, min_val, max_val, boot_val, reset_val FROM pg_settings;
...
name          | authentication_timeout
current_value | 360 s
short_desc    | Sets the maximum allowed time to complete client authentication.
extra_desc    |
min_val       | 1
max_val       | 600
boot_val      | 60
reset_val     | 360
```

在前面的示例中，authentication_timeout 已被设置为 360 秒，如 setting 列所示，其值可在 1 秒到 600 秒的范围内调整；boot_val 是参数在未配置时将采用的默认值，设置为 60 秒。类似地，reset_val 列指示参数在更改后重置时将采用的值。

pg_settings 特殊目录还包含其他有用的信息，包括参数是从哪个文件和行号加载的。这些信息可用于快速找到配置参数在 postgresql.conf 文件或其他配置文件中设置的位置。作为示例，以下查询将显示每个参数是从哪里加载的：

```
forumdb=# SELECT name, setting AS current_value, sourcefile, sourceline, pending_restart FROM pg_settings;
...
name          | log_destination
current_value | stderr
sourcefile     | /postgres/16/data/postgresql.auto.conf
sourceline     | 4
pending_restart | f
```

正如你所见，在此示例安装中，log_destination 配置参数是从 /postgres/16/data/postgresql.auto.conf 文件加载的。这不是主要的 postgresql.conf 文件；它是一个 PostgreSQL 自动生成的用于处理从集群内部更改的配置参数的文件。postgresql.auto.conf 的使用将在本章后面变得清晰；目前，知道配置参数已从另一个文件加载并且 pg_settings 清楚地报告了这一点就足够了。

> 你需要拥有数据库超级用户权限才能收集额外信息，例如文件位置和行号。

pg_settings 特殊目录中可以获得的另一条重要信息是：如果在运行时更改参数会发生什么？根据参数的性质，更改可以立即应用，也可以延迟到等待特定事件甚至集群重启。pending_restart 列指示当前参数是否已从其启动时值更改以及该值是否已应用于集群。在前面的示例中，pending_restart 为 false，因此你看到的配置就是当前在集群上运行的有效配置。

查询 pg_settings 为你提供了了解当前配置状态所需的所有信息，但有一个快捷方式可以更快地获取参数的设置：SHOW 命令。特殊命令 SHOW 接受参数的名称并以人类可理解的形式报告该参数的值。作为示例，假设我们想检查 shared_buffers 内存值：

```
forumdb=> SHOW shared_buffers ;
shared_buffers
----------------
128MB
(1 row)
```

相同的信息显然可以从 pg_settings 中获得，使用更长的查询，并且需要将确切值转换为人类可读的形式（注意内存值以 8kB 块表示）：

```
forumdb=> SELECT name, setting, unit FROM pg_settings WHERE name = 'shared_buffers';
      name      | setting | unit
------------------+---------+------
shared_buffers | 16384   | 8kB
(1 row)
```

因此，如果你只需要快速查看配置参数，SHOW 是正确的命令，而如果你需要有关确切值、参数在配置文件中设置的位置等更准确和完整的信息，则应查询 pg_settings 目录。

# 查找配置错误

PostgreSQL 提供了一个非常有用的目录 pg_file_settings，它让数据库管理员可以一目了然地查看所有配置参数及其加载的文件，从而提供有关错误的信息。以下查询从目录中提取所有信息，修剪后的输出为我们提供了一些重要信息：

```
postgres=# SELECT name, setting, sourcefile, sourceline, applied, error FROM pg_file_settings where name = 'log_destination';
-[ RECORD 1 ]---------------------------------------------
name      | log_destination
setting   | stderr
sourcefile | /postgres/16/data/postgresql.conf
sourceline | 444
applied   | f
error     |
-[ RECORD 2 ]---------------------------------------------
name      | log_destination
setting   | csvlog
sourcefile | /postgres/16/data/postgresql.conf
sourceline | 818
applied   | f
error     |
```

```
- [ RECORD 3 ]-------------------------------------
name      | log_destination
setting   | stderr
sourcefile | /postgres/16/data/postgresql.auto.conf
sourceline | 4
applied   | t
error     |
```

正如你在上面的示例安装中所见，log_destination 配置参数已被多次加载：恰好三次，来自不同的源文件。已应用的是来自 /postgres/16/data/postgresql.auto.conf 文件第 4 行的配置设置，如 applied 列的状态所报告。如果参数包含错误，error 列将提供有关问题的提示。

在同一文件或不同文件中多次定义参数并不是错误，而是一种覆盖该参数的方式。特殊视图 pg_file_settings 有助于找出为什么文件中定义的参数未被加载以及哪些参数被其他分散的定义覆盖。

如果参数因包含错误而无法应用，error 列将为你提供有关问题的提示，而 applied 列将显示为 false 值。

# 嵌套配置文件

你不必将每个参数都配置在 postgresql.conf 文件中：你可以定义自己的微小且专注的配置文件，并指示 PostgreSQL 读取并将其内容添加到配置中。借助此功能，你可以为特定任务创建一组更小更干净的配置文件，从而使维护变得更简单。

PostgreSQL 提供三个主要指令来包含其他配置文件：

- include_file: 在配置中包含单个文件
- include_dir: 包含指定目录中的所有文件
- include_if_exists: 仅在文件存在时包含文件

最后一个指令非常方便，因为如果包含的文件不存在，PostgreSQL 将抛出错误，而使用 include_if_exists，集群不会警告你如果要包含的文件尚未创建。这对于配置管理很有用，例如，你可以设置一个包含多个文件的主配置文件，并仅将这些文件分发给真正需要此类配置的系统。

为了给出一个具体的例子，考虑一个名为 `memory.conf` 的文件，它放置在 PGDATA 目录中，其内容如下：

- `shared_buffers = 321 MB`
- `work_mem = 16 MB`

然后在 `postgresql.conf` 文件末尾添加以下指令：

```
```conf
include_if_exists = 'memory.conf'
```
```

这样，一旦集群重启，`postgresql.conf` 文件将指示集群加载另一个名为 `memory.conf` 的配置文件，而该文件又会覆盖几项内存设置（请记住，配置参数的最后一次定义是被选为应用的定义）。最终结果可以通过查询 `pg_file_settings` 目录来查看：

```
```text
postgres=# SELECT * FROM pg_file_settings WHERE name IN ('shared_buffers', 'work_mem');

                    sourcefile              | sourceline | seqno |      name       |
setting | applied | error
---------------------------------------------+------------+-------+-----------------+
---------------------------------------------+----------+-------+-----------------
/postgres/16/data/postgresql.conf |    127 |   3 | shared_buffers |
128MB     | f       |
/postgres/16/data/memory.conf     |      1 |  15 | shared_buffers |
321MB     | t       |
/postgres/16/data/memory.conf     |      2 |  16 | work_mem       |
16MB      | t       |
(3 rows)
```
```

正如你所见，`shared_buffers` 参数已被覆盖并从新创建的 memory 文件中应用，而 `work_mem` 在默认安装中被注释掉了，也已从包含的文件中应用（没有覆盖任何其他定义）。

# 配置上下文

每个配置参数都属于一个所谓的 **context**，该组定义了 *何时* 可以应用对参数的更改。几个参数可以在集群生命周期期间更改并生效。然而，其他参数则不能，需要重启集群；配置参数的上下文有助于系统管理员了解更改何时生效。

