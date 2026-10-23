

以类似的方式，从这个完整备份开始，我们可以制作增量备份：

```
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 --type=incr backup

2023-07-11 08:44:35.816 P00  INFO: expire command end: completed successfully (15ms)
```

我们也可以制作差异备份：

```
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 --type=diff backup

2023-07-11 08:45:40.020 P00  INFO: expire command end: completed successfully (32ms)
```

现在 info 命令将跟踪这三个备份：

```
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 info
stanza: pg1
    status: ok
    cipher: aes-256-cbc

    db (current)
        wal archive min/max (15): 00000001000000000000001/00000001000000 000000008

        full backup: 20230711-084245F
            timestamp start/stop: 2023-07-11 08:42:45 / 2023-07-11 08:42:57
            wal start/stop: 00000001000000000000004 / 00000001000000000000004
            database size: 22.0MB, database backup size: 22.0MB
            repo1: backup set size: 2.9MB, backup size: 2.9MB

        incr backup: 20230711-084245F_20230711-084431I
            timestamp start/stop: 2023-07-11 08:44:31 / 2023-07-11 08:44:35
            wal start/stop: 00000001000000000000006 / 00000001000000000000006
            database size: 22.0MB, database backup size: 8.3KB
            repo1: backup set size: 2.9MB, backup size: 496B
```

```
backup reference list: 20230711-084245F

diff backup: 20230711-084245F_20230711-084536D
    timestamp start/stop: 2023-07-11 08:45:36 / 2023-07-11 08:45:39
    wal start/stop: 000000010000000000000008 /
000000010000000000000008
    database size: 22.0MB, database backup size: 8.3KB
    repo1: backup set size: 2.9MB, backup size: 496B
    backup reference list: 20230711-084245F
```

由于我们在 pgbackrest.conf 文件中设置了 repo1-retention-full=2，pgbackrest 在执行两次备份后会删除第一个完整备份及其关联的差异或增量备份。例如，这里是执行两次完整备份的情况：

```
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 --type=full backup

2023-07-11 08:48:39.866 P00  INFO: expire command end: completed successfully (22ms)

postgres@pgbackrest:~$ pgbackrest --stanza=pg1 --type=full backup

2023-07-11 08:49:37.101 P00  INFO: expire command end: completed successfully (34ms)
```

我们将得到以下结果：

```
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 info
stanza: pg1
    status: ok
    cipher: aes-256-cbc

    db (current)
        wal archive min/max (15): 000000010000000000000001/00000001000000000000000C

        full backup: 20230711-084830F
            timestamp start/stop: 2023-07-11 08:48:30 / 2023-07-11 08:48:39
```

# DROP TABLE

现在让我们尝试在 2023-07-11 08:55:08 进行恢复，这是灾难发生之前的时间。在 pg1 服务器上，我们需要停止 postgresql 服务器：

```
# systemctl stop postgresql
```

然后我们执行 pgbackrest restore 命令：

```
root@pg1:# su - postgres
postgres@pg1:$ pgbackrest --stanza=pg1 --delta --log-level-console=info --type=time "--target=2023-07-11 08:55:08" restore

2023-07-11 08:57:39.803 P00  INFO: restore command end: completed successfully (1905ms)
```

现在让我们以 root 用户身份启动 postgresql 服务器：

```
# systemctl start postgresql
```

然后我们检查 postgresql 日志：

```
2023-07-11 08:59:06.844 P00  INFO: archive-get command end: completed successfully (532ms)
2023-07-11 08:59:06.849 UTC [8786] LOG:  restored log file "0000000100000000000000D" from archive
2023-07-11 08:59:06.898 UTC [8786] LOG:  consistent recovery state reached at 0/C000138
2023-07-11 08:59:06.898 UTC [8783] LOG:  database system is ready to accept read-only connections
2023-07-11 08:59:06.938 P00  INFO: archive-get command begin 2.46: [0000000100000000000000E, pg_wal/RECOVERYXLOG] --exec-id=8797-9e31ebc7 --log-level-console=info --log-level-file=info --pg1-path=/var/lib/postgresql/15/main --repo1-host=192.168.122.120 --repo1-host-port=22 --repo1-host-user=postgres --stanza=pg1
2023-07-11 08:59:07.274 P00  INFO: unable to find 0000000100000000000000E in the archive
2023-07-11 08:59:07.375 P00  INFO: archive-get command end: completed successfully (440ms)
```

```
2023-07-11 08:59:07.379 UTC [8786] LOG:  recovery stopping before commit of transaction 737, time 2023-07-11 08:55:13.072211+00
2023-07-11 08:59:07.379 UTC [8786] LOG:  pausing at the end of recovery

2023-07-11 08:59:07.379 UTC [8786] HINT:  Execute pg_wal_replay_resume()
to promote.
```

正如我们所见，为了结束我们的 PITR 过程，PostgreSQL 建议我们执行 pg_wal_replay_resume()。因此，让我们进入 PostgreSQL 环境并执行以下操作：

```
postgres=# select pg_wal_replay_resume();
pg_wal_replay_resume
----------------------

(1 row)
```

现在如果我们去检查我们的数据库 db1，users 表现在存在并且数据库现在处于 2020-05-30 16:23:38 时的状态：

```
postgres=# \d
       List of relations
 Schema |  Name  | Type  |  Owner  
--------+--------+-------+----------
 public | users  | table | postgres
(1 row)
```

最后，我们可以执行以下操作：

```
postgres=# select count(*) from users ;
 count 
-------
 10000
(1 row)
```

我们现在已恢复灾难发生前的状况。

正如我们所见，使用 pgbackrest 管理连续备份和 PITR 非常简单。连续备份和 PITR 在复杂生产环境的设置中绝不应该缺失。这可以保护我们免受不必要的数据删除，并为我们提供“最后手段”，以防主服务器和副本服务器都不可用时使用。

# 使用 pgloader 从 MySQL/MariaDB 迁移到 PostgreSQL

在本节中，我们将看到如何以非常简单的方式将数据库从 MySQL/MariaDB 世界迁移到 PostgreSQL 世界。我们将使用的工具称为 pgloader。有关本节的进一步信息的参考资料可在 https://pgloader.io 找到。

本节有两个 Docker 容器可用，一个内部包含 mariadb 服务器，另一个内部包含 postgresql 服务器。名为 mariadb-source 的 mariadb 服务器包含在 *第 4 章* 中使用的 forumdb 数据库的副本；名为 pg-destination 的 postgresql 服务器包含一个空的 forumdb 数据库。我们的目标将是使用 pgloader 工具将 forumdb 数据库的所有内容从 mariadb-source 服务器迁移到 postgresql-destination 服务器。

让我们打开两个 Bash 终端，并在第一个中执行：

```
```bash
chapter19$ bash run-pg-docker-mariadb.sh
```
```

容器启动后，让我们使用 LearnPostgreSQL 作为密码执行以下语句：

```
```text
root@mariadb-source:~# mysql -D forumdb -p
[...]
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [forumdb]>
```
```

现在我们进入了 forumdb 数据库，我们可以在其上执行查询：

```
```text
MariaDB [forumdb]> show tables;
+------------------+
| Tables_in_forumdb |
+------------------+
| categories       |
| j_posts_tags     |
| posts            |
```
```

```
```text
| tags                 |
| users                |
+----------------------+

5 rows in set (0.000 sec)

MariaDB [forumdb]> select * from categories;
+----+----------------------+-----------------------------+
| pk | title                | description                 |
+----+----------------------+-----------------------------+
|  1 | Database             | Database related discussions|
|  2 | Unix                | Unix and Linux discussions  |
|  3 | Programming Languages| All about programming languages |
+----+----------------------+-----------------------------+
3 rows in set (0.002 sec)

MariaDB [forumdb]> select * from users;
+----+----------+--------------+----------------------+
| pk | username | gecos        | email                |
+----+----------+--------------+----------------------+
|  1 | fluca1978| Luca Ferrari | fluca1978@gmail.com  |
|  2 | sscotty71| Enrico Pirozzi | sscptty71@gmail.com |
+----+----------+--------------+----------------------+
2 rows in set (0.001 sec)
```
```

正如我们所见，数据库与我们在 *第 4 章，基本语句* 中使用的一样。

现在让我们转到第二个 Bash 终端并执行：

```
```bash
chapter19$ bash run-pg-docker-postgresql.sh
```
```

容器启动后，让我们执行：

```
```bash
postgres@pg-destination:~$ psql forumdb
```
```

正如下方所示，我们的 postgresql 数据库是空的：

```
```text
postgres@pg-destination:~$ psql forumdb
forumdb=# \d
Did not find any relations.
```
```

