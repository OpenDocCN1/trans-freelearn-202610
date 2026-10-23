## 19 个有用的工具和扩展

本章可视为本书的附录。在本章中，我们将讨论一些工具和扩展，它们能够让数据库管理员（DBA）通过减少完成工作所需的精力来最大化工作效率。

我们将讨论以下扩展：

- pg_trgm
- 外部数据包装器和 postgres_fdw 扩展

这是 PostgreSQL 的两个官方扩展。 一个可以非常有用的用于查找 PostgreSQL 可用扩展的网站是 https://pgxn.org/。

除了扩展之外，我们还将讨论 PostgreSQL DBA 的其他有用工具。 PostgreSQL 可用的工具多达数十种，但在本章中，我们将讨论：

- pgbackrest：一个强大的工具，用于管理灾难恢复和时间点恢复（PITR）
- pgloader：一个有用的工具，可轻松地从 MySQL、SQLite 和 MS SQL 迁移到 PostgreSQL；在这一节中，我们将展示一个如何以非常简单的方式从 MySQL 迁移到 PostgreSQL 的示例。

本章旨在快速概述一些最有用的 PostgreSQL 扩展和工具。

以下主题将在此处涵盖：

- 探索 pg_trgm 扩展
- 使用外部数据包装器和 postgres_fdw 扩展
- 管理 pgbackrest 工具
- 探索 pgloader 工具

在本章中，Docker 容器仅在某些部分中使用。

### 技术要求

本节有一个 Docker 容器，您可以在 learn_postgresql_16/docker-images/chapter19 找到它，因此在进入正确路径后，让我们运行：

```
chapter19$ bash run-pg-docker-pg_trgm.sh chapter19-pg_trgm

postgres@learn_postgresql:~$ psql -U forum forumdb
forumdb=>
```

### 探索 pg_trgm 扩展

现在让我们回到第 13 章《索引和性能优化》，索引部分。当我们谈论索引时，我们学习了如何通过使用索引使查询更快。然而，B-tree 索引并不索引所有类型的操作。现在让我们考虑文本数据类型（char、varchar 或 text）。现在，我们将看到 B-tree 使用 varchar_pattern_ops 操作符类，能够为以 search% 开头的句子索引文本查询，但不能为以 %search 结尾或包含 %search% 的句子索引文本查询。

1. 在深入我们的示例之前，让我们将 enable_seqscan 设置为 off，以便强制 PostgreSQL 在存在索引时使用索引。我们需要这样做，因为在我们的示例情况下，PostgreSQL 默认总是使用顺序扫描，因为我们的表中只有几条记录，并且表中存在的所有数据都存储在单个页面上：

```
forumdb=> set enable_seqscan to 'off';
SET
```

2. 在我们的数据库中，我们现在可以在 categories 表上执行此查询：

```
forumdb=> select pk,title from categories;
```

| pk | title |
|----|-------|
| 1 | Database |
| 2 | Unix |
| 3 | Programming Languages |

3. 让我们创建一个带有 varchar opclass 的 B-tree 索引，以便检查 PostgreSQL 在我们使用 like 操作符执行查询时是否使用索引访问表：

```
forumdb=> create index on categories using btree(title varchar_pattern_ops);
CREATE INDEX
```

让我们现在执行一些 like 查询：

4. 作为第一个示例，让我们使用 ‘search%’ 谓词执行 like 查询：

```
forumdb=> explain analyze select * from categories where title like 'Da%';

QUERY PLAN

---------------------------------------------------------------
Index Scan using categories_title_idx on categories
(cost=0.13..8.15 rows=1 width=68)
(actual time=0.033..0.037 rows=1 loops=1)
  Index Cond: ((title ~>=~ 'Da'::text) AND (title ~<~ 'Db'::text))
  Filter: (title ~~ 'Da%'::text)
Planning Time: 0.172 ms
Execution Time: 0.075 ms
(5 rows)
```

5. 作为第二个示例，让我们使用 ‘%search’ 谓词执行 like 查询：

```
forumdb=> explain analyze select * from categories where title like '%Da%';

QUERY PLAN

---------------------------------------------------------------
Seq Scan on categories  (cost=1000000000.00..1000000001.04 rows=1 width=68) (actual time=17.278..17.283 rows=1 loops=1)
  Filter: (title ~~ '%Da%'::text)
  Rows Removed by Filter: 2
Planning Time: 0.101 ms
JIT:
```

正如这里所见，PostgreSQL 现在能够使用 like 查询创建索引访问。 对于所有类型的 like 和 ilike 查询也是如此； pg_trgm 扩展解决了此类查询的表访问索引。 有关 pg_trgm 扩展的更多信息，请参阅 https://www.postgresql.org/docs/current/pgtrgm.html。

pg_trgm 扩展在 DBA 需要优化 like 和 ilike 查询的所有情况下都方便了 DBA 的工作。现在，我们将继续下一个扩展，postgres_fdw。

### 使用外部数据包装器和 postgres_fdw 扩展

外部数据包装器允许我们像访问普通本地表一样访问托管在外部数据库上的数据。我们可以将 PostgreSQL 连接到各种数据源，我们可以将 PostgreSQL 连接到另一个 PostgreSQL 服务器，或将 PostgreSQL 连接到可以是关系型或非关系型的数据源。一旦外部数据包装器连接，PostgreSQL 就能够像本地一样读取远程表。 有用于 Oracle 和 MySQL 等知名数据库的外部数据包装器，也有用于不太知名系统的外部数据包装器。 PostgreSQL 可用的外部数据包装器的完整列表可在 https://wiki.postgresql.org/wiki/Foreign_data_wrappers 获取。

在本节中，我们将考虑一个使用 postgres_fdw 外部数据包装器的示例，该包装器用于将一个 PostgreSQL 服务器连接到另一个 PostgreSQL 服务器。

如果我们想使用 Docker 镜像，我们必须打开两个 Bash 终端，并在第一个上执行：

```
chapter19$ bash run-pg-docker.sh chapter19-postgresql_fdw
postgres@pg_fdw1:~$ psql -U forum forumdb
forumdb=> select * from categories;
```

| pk | title | description |
|----|-------|-------------|

(0 rows)

然后，在第二个 Bash 终端上，我们必须执行：

```
chapter19$ bash run-pg-docker-pg_fdw2.sh chapter19-postgresql_fdw
postgres@pg_fdw2:~$ psql -U forum forumdb
forumdb=> select * from categories;
```

| pk | title | description |
|----|-------|-------------|
| 1 | Database | Database related discussions |
| 2 | Unix | Unix and Linux discussions |
| 3 | Programming Languages | All about programming languages |

(3 rows)

我们的初始情况是两个服务器。我们有一个名为 pg_fdw1 的服务器，IP地址为 192.168.16.2，以及一个名为 pg_fdw2 的服务器，IP地址为 192.168.16.3。我们的目标将是连接服务器 pg_fdw2 到服务器 pg_fdw1，并使从服务器 pg_fdw1 查询服务器 pg_fdw2 的 category 表成为可能，就像它是本地的一样：

1. 让我们从在 pg_fdw1 服务器上安装 postgres_fdw 扩展开始。因此，作为超级用户 postgres，让我们执行：

```
postgres@pg_fdw1:~$ psql -U postgres forumdb

forumdb=# create extension postgres_fdw ;
CREATE EXTENSION
```

假设在 pg_fdw2 服务器上，pg_hba.conf 配置如下：

```
host    all             all             192.168.16.0/24
        scram-sha-256
```

2. 现在我们必须使用以下语句在 pg_fdw1 服务器上创建两个服务器之间的连接：

```
forumdb=# CREATE SERVER remote_pg_fdw2 FOREIGN DATA WRAPPER
postgres_fdw OPTIONS (host 'pg_fdw2', dbname 'forumdb');
CREATE SERVER
```

3. 现在，我们必须在两个服务器之间编写一个用户映射：

```
forumdb=# CREATE USER MAPPING FOR forum SERVER remote_pg_fdw2
OPTIONS (user 'forum', password 'LearnPostgreSQL');
CREATE USER MAPPING
```

4. 现在我们必须为 forum 用户创建具有 SELECT 权限的外部表：

```
forumdb=# create foreign table forum.f_categories (
    pk integer,
```



还有很多其他工具，在 https://wiki.postgresql.org/wiki/Binary_Replication_Tools 上可以找到它们之间很好的对比。

在本节中，我们将重点介绍 pgbackrest；它是用于灾难恢复的最常用工具之一，因为它以非常简单的方式实现了在多核上的可扩展性，并且支持以压缩和加密的方式将数据保存到存储桶中。pgbackrest 工具是用于 PostgreSQL 灾难恢复和 PITR（时间点恢复）的工具，专为高负载服务器而设计。其官方网址是 https://pgbackrest.org/。

以下是该工具的一些特性：

- 支持并行备份和并行恢复。
- 可以进行全量基础备份、增量备份或差异备份。
- 我们可以选择执行本地操作或远程操作。
- 我们可以选择备份和归档过期的保留策略。
- 支持备份续传。
- 支持流式压缩和校验和。
- 对于恢复过程，我们可以使用增量恢复（delta restore）功能。
- 可以使用并行 WAL 归档。
- 支持表空间和链接。
- 支持数据加密。
- 支持对存储库存储使用 SFTP。
- 支持对 S3、GCP 和 Azure 的对象存储。

## 基本概念

pgbackrest 工具使用 stanza（配置节）的概念，并且可以使用本地或外部存储库：

- stanza 是远程服务器用于备份的配置。它是一组要备份的目标。一个 stanza 配置可以包含多个服务器，在这种情况下，第一个（pg1）是主服务器，其他服务器被视为备用服务器。
- 存储库是保存备份的本地或远程（SSH）存储；它可以加密。一个存储库可以包含多个定义，但目前仅支持第一个（repo1）。
- 在使用 pgbackrest 的用户之间进行公钥交换非常重要。最简单的做法是在 PostgreSQL 服务器的 Postgres 用户与 pgbackrest 存储库所在服务器的 Postgres 用户之间交换公钥。

## 环境搭建

在本节中，没有 Docker 容器，因此在开始测试我们的 pgbackrest 工具之前，让我们看看需要什么才能开始工作。我们将需要以下内容：

- 一个正在运行的 PostgreSQL 服务器
- 一台我们将使用 postgres 用户安装和配置 pgbackrest 工具的服务器

在本场景中，我们将继续使用我们的 pg1 PostgreSQL 服务器，其 IP 为 192.168.122.170。我们还需要添加另一台名为 pgbackrest 的服务器，IP 地址为 192.168.122.120。

## 公钥交换

现在，我们将在安装 pgbackrest 之前了解如何交换公钥：

- 1. 首先，让我们在两台服务器上为 Postgres 用户创建 ssh 密钥。作为 PostgreSQL 用户，请执行以下命令：

```
postgres@pgbackrest:~$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/postgres/.ssh/id_rsa):
Created directory '/home/postgres/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/postgres/.ssh/id_rsa
Your public key has been saved in /home/postgres/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:5BPKarhop6Z82WeWWtYM1i5gHseFHAVJEoKy8GFSHjQ postgres@pgbackrest
The key's randomart image is:
+---[RSA 4096]----+
| oE. oo+-.       |
| +oOoo. o+o      |
| o=..  o+.       |
| . .  ..+o.      |
```

```
|  .+o=S.  |
|  .OO= =.  |
|  o =. +. + |
| ... = .O=.  |
| .+o  .=  |
+-----[SHA256]-----+ 

postgres@pg1:~$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/postgres/.ssh/id_rsa):
Created directory '/home/postgres/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/postgres/.ssh/id_rsa
Your public key has been saved in /home/postgres/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:g/amWaxcTGsmx2WQ91U/23UcBXmDtSs fRYhqE7dMwko postgres@pg1
The key's randomart image is:
+---[RSA 4096]----+
|      .+*=| 
|     .E =.o==| 
|   o..X ..+B| 
|   . o*.o. O| 
|  o S.o... +.|
|   . * =   o .| 
|   . &     . | 
|   . %     | 
|   =       | 
+-----[SHA256]-----+
```

当我们执行 ssh-keygen 命令时，必须确保在提示输入密码短语时只按回车键。

- 2. 现在，在两台服务器上，postgres 用户的 ~/.ssh 目录中将有两个文件：

```
postgres@pgbackrest:~/.ssh$ ls -l
total 8
-rw------ 1 postgres postgres 3389 Jul 10 08:34 id_rsa
-rw-r--r-- 1 postgres postgres  745 Jul 10 08:34 id_rsa.pub
```

```
postgres@pg1:~/.ssh$ ls -l
total 8
-rw------ 1 postgres postgres 3381 Jul 10 08:34 id_rsa
-rw-r--r-- 1 postgres postgres  738 Jul 10 08:34 id_rsa.pub
```

3. 在两台服务器之间交换公钥最快的方法是使用 ssh-copy-id 命令：

```
postgres@pg1:~/.ssh$ ssh-copy-id 192.168.122.120
[... cutted...]
Number of key(s) added: 1

Now try logging into the machine, with:  "ssh '192.168.122.120'"
and check to make sure that only the key(s) you wanted were added.

postgres@pgbackrest:~/.ssh$ ssh-copy-id 192.168.122.170
[... cutted...]

Number of key(s) added: 1

Now try logging into the machine, with:  "ssh '192.168.122.170'"
and check to make sure that only the key(s) you wanted were added
```

现在，使用 postgres 用户可以在不提供密码的情况下将两台服务器连接起来。

## 安装 pgbackrest

安装之前，我们必须检查每个主机的软件源中是否存在相同版本的 pgbackrest。检查之后，让我们在 pgbackrest 服务器和 pg1 服务器上安装它。在类似 Debian 的服务器上，以 root 用户身份在两台服务器上执行以下命令：

```
root@pg1:~# apt-get update
root@pgbackrest:~# apt-get update

root@pg1:~# apt-get install pgbackrest
root@pgbackrest:~# apt-get install pgbackrest
```

如果我们使用 RHEL 服务器，则必须使用 yum 命令而不是 apt-get 命令。

```
process-max=2
repo-path=/var/lib/pgbackrest
repo1-retention-full=2
repo1-retention-archive=5
repo1-retention-diff=3
log-level-console=info
log-level-file=info
```

我们在上面看到以下选项：

- start-fast=y：强制在远程服务器上执行检查点，使 pg_start_backup() 尽快启动。
- archive-async=y：启用 WAL 的异步传输，用于 push/pull 操作。
- process-max=2：设置系统可用于传输/压缩操作的最大进程数。
- repo-path=/var/lib/pgbackrest：设置存储库的存储路径；运行 pgbackrest 命令的用户必须对该目录具有读/写权限。
- repo1-retention-full=2：要保留的全量备份数量。当全量备份过期时，与该全量备份关联的所有差异备份和/或增量备份也将过期。当未定义该选项时，系统会发出警告。如果希望无限期保留，请将该选项设置为最大值 (9,999,999)。
- repo1-retention-archive=5：表示要保留的 WAL 文件的备份数量。使备份保持一致所需的 WAL 段始终保留到备份过期为止，无论该选项如何配置。如果未设置此值，过期的归档将根据 repo-retention-full（或 repo-retention-diff）的值自动过期，该值与归档保留类型（若设置为 full 或 diff）相对应。这将确保 WAL 文件只对已经过期的备份被视为过期。
- repo1-retention-diff = 3：要保留的差异备份数量。当差异备份过期时，与该差异备份关联的所有增量备份也将过期。如果未定义，所有差异备份将一直保留，直到它们所依赖的全量备份过期为止。
- log-level-console=info/log-level-file=info 日志：日志管理设置；设置终端日志级别（log-level-console）和日志文件的日志级别（log-level-file）。





The configuration file shown here is just a simple example; if we want to add some more features, we just need to add them to the configuration file.

For example, if we want to modify the compression level and increase it to level 9, we can add these lines:

```
compress = y
compress-level = 9
compress-level-network = 9
```

In the same way, if we want to add the cipher feature, we can add these lines:

```
repo1-cipher-type = aes-256-cbc
repo1-cipher-pass = LearnPostgreSQL
```

After configuring the global section, we are ready to look at how to configure the stanza. pgbackrest introduces the idea of stanzas; in practice, we can associate each stanza with a cluster database. The following is an example of a stanza; it is only a coincidence that the name of the stanza, [pg1], has the same name as the cluster. It is necessary to create a stanza for each remote PostgreSQL server on which we want to manage backups using pgbackrest. Each stanza must have a different name:

```
[pg1]
pg1-host = 192.168.122.170
pg1-host-user = postgres
pg1-path = /var/lib/postgresql/16/main
pg1-port = 5432
```

We see the following options here:

- pg1-host: This is the remote host of the PostgreSQL master server.
- pg1-host-user = postgres: When the pg1-host parameter is set, this is the user that we want to use to access the remote PostgreSQL server. This user will also be the owner of the remote pgbackrest process and it starts the connection to the PostgreSQL server. This user should be the owner of the PostgreSQL database cluster. Usually, we can leave the default user, postgres, which is why it is usually the same user for whom we made the exchange of public keys.
- pg1-path = /var/lib/postgresql/16/main: The path on the PostgreSQL cluster where the data is stored. We can find it in the data_directory parameter inside the postgresql.conf file.
- pg1-port = 5432: The listen port of the remote PostgreSQL server.

## Using pgbackrest with object store support

pgbackrest supports object stores for Azure, GCP, and Amazon S3; that means that pgbackrest is available to store all the data directly on a low-cost bucket using the data encryption we’ve seen before.

The configuration is pretty simple; for example, for a GCP bucket, we have to specify the parameters below:

```
repo1-type=gcs
repo1-path=/path_on_the_bucket
repo1-gcs-bucket=bucket_name
repo1-gcs-key=/etc/pgbackrest-key.json
```

Adding those parameters, pgbackrest will be able to store all data in a Google bucket; this feature is very useful for the DBA because it allows us to store and encrypt our data on the cloud at a small price.

In this way, we don’t have to worry about the size of the disk that contains the repository used by pgbackrest and, at the same time, since the cost per GB per bucket is very low, we can greatly increase our retention. For further information about the pgbackrest configuration with S3, Azure, and GCP buckets, you can visit the links below:

- https://pgbackrest.org/user-guide.html#azure-support
- https://pgbackrest.org/user-guide.html#s3-support
- https://pgbackrest.org/user-guide.html#gcs-support

## The PostgreSQL server configuration

Now let’s move on to the PostgreSQL server configuration. On the pg1 server, we need to modify the postgresql.conf file and we need to set the pgbackrest.conf file as well.

### The postgresql.conf file

For the postgresql.conf file, we have to set wal_level to replica or logical. It is important that the WAL level is not set to minimal, because PITR is not possible if wal_level=minimal. We also need to tell PostgreSQL the command that will send the WAL segment to the pgbackrest repository server.

Let’s add these lines at the end of the `postgresql.conf` file:

```
#PGBACKREST
archive_mode = on
wal_level = replica #logical if we have some logical replications
archive_command = 'pgbackrest --stanza=pg1 archive-push %p'
```

With the second line, we say to PostgreSQL that the WAL segments will be archived on the pg1 stanza of the repository server using the pgbackrest command. After restarting PostgreSQL, these new lines will be available. As the root user, let’s perform a restart of the PostgreSQL service:

```bash
# systemctl restart postgresql
```

### The pgbackrest.conf file

Now, after modifying `postgresql.conf`, let’s go to modify the `pgbackrest.conf` file of the PostgreSQL server. Let’s remember that the PostgreSQL server has ip= 192.168.122.170, and that the IP of the disaster recovery server is 192.168.122.170. Let’s now edit the `/etc/pgbackrest.conf` file; delete what is present and add these lines:

```ini
[global]
backup-host=192.168.122.120
backup-user=postgres
backup-ssh-port=22
log-level-console=info
log-level-file=info

[pg1]
pg1-path = /var/lib/postgresql/16/main
pg1-port = 5432
```

As for the repository configuration, the file is composed of sections: a global section and a section for each stanza.

For the global section, we have the following options:

- `backup-host`: The repository host
- `backup-user`: The user used for the backup
- `backup-ssh-port`: The ssh port
- `log-level-console=info` and `log-level-file=info`: As we’ve seen in the previous section

For the stanza section, we have the following options:

- `pg1-path = /var/lib/postgresql/16/main`: The path on the PostgreSQL cluster where the data is stored. We can find it in the `data_directory` parameter inside the `postgresql.conf` file.
- `pg1-port = 5432`: The listen port of the remote PostgreSQL server.

## Creating and managing continuous backups

Now that we have our system well configured, let’s start to manage our backups.

### Creating the stanza

The first thing we have to do is create the stanza on the repository server. To do this, as a postgres user, let’s perform this command:

```text
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 stanza-create
2023-07-11 08:34:48.439 P00  INFO: stanza-create command begin 2.46:
--compress-level-network=9 --exec-id=2602-fbad976e --log-level-
console=info --log-level-file=info --pg1-host=192.168.122.170 --pg1-host-
user=postgres --pg1-path=/var/lib/postgresql/16/main --pg1-port=5432
--repo1-cipher-pass=<redacted> --repo1-cipher-type=aes-256-cbc --repo1-
path=/var/lib/pgbackrest --stanza=pg1
2023-07-11 08:34:49.558 P00  INFO: stanza-create for stanza 'pg1' on
repo1
2023-07-11 08:34:49.741 P00  INFO: stanza-create command end: completed
successfully (1305ms)
```

Now our stanza is created. If we go to `/var/lib/pgbackrest`, we can find the directory structure that will be used by the continuous backup system:

```text
postgres@pgbackrest:/var/lib/pgbackrest$ ls -l
total 8
drwxr-x--- 3 postgres postgres 4096 Jul 11 08:34 archive
drwxr-x--- 3 postgres postgres 4096 Jul 11 08:34 backup
```

### Checking the stanza

After creating our stanza, let’s check whether the system is ready to accept the continuous backup by performing this:

```bash
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 check
```

```
[....]

completed successfully (2868ms)
```

If everything is OK, we will receive a completed successfully message (as seen above); now we are ready to manage continuous backup.

### Managing base backups

As we previously mentioned, pgbackrest is able to handle full backups, differential backups, and incremental backups with a simple command-line statement.

To create a full base backup, we can do this:

```shell
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 --type=full backup
```

When we press the *Enter* key on the keyboard, if everything is OK, we get this message:

```text
2023-07-11 08:42:58.125 P00   INFO: expire command end: completed
successfully (46ms)
```

Now, if we want information about our repository, we can use the info command as follows:

```text
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 info
stanza: pg1
    status: ok
    cipher: aes-256-cbc

    db (current)
        wal archive min/max (15): 000000010000000000000001/00000001000000
00000004

        full backup: 20230711-084245F
                timestamp start/stop: 2023-07-11 08:42:45 / 2023-07-11
08:42:57
                wal start/stop: 000000010000000000000004 /
000000010000000000000004
                database size: 22.0MB, database backup size: 22.0MB
                repo1: backup set size: 2.9MB, backup size: 2.9MB
```

The info command tells us about WAL segments, the full backup start time, the original database size, and the repository backup size.





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





现在让我们退出 psql 客户端并执行 pgloader 命令：

```
forumdb=# \q
ppostgres@pg-destination:~$ pgloader mysql://root:LearnPostgreSQL@mariadb-source/forumdb pgsql://postgres@127.0.0.1/forumdb

table name          errors   rows    bytes    total time
-------------------  -------  ------  -------  ----------
fetch meta data            0      22                0.104s
Create Schemas             0       0                0.000s
Create SQL Types           0       0                0.004s
Create tables              0      10                0.056s
Set Table OIDs             0       5                0.020s
-------------------  -------  ------  -------  ----------
forumdb.categories         0       3    0.1 kB       0.020s
forumdb.users              0       2    0.1 kB       0.016s
forumdb.j_posts_tags       0       0                0.012s
forumdb.tags               0       0                0.016s
forumdb.posts              0       0                0.012s
-------------------  -------  ------  -------  ----------
COPY Threads Completion    0       4                0.024s
Index Build Completion     0      11                0.048s
Create Indexes             0      11                0.092s
Reset Sequences            0       4                0.012s
Primary Keys               0       4                0.004s
Create Foreign Keys        0       6                0.004s
Create Triggers            0       0                0.000s
Set Search Path            0       1                0.000s
Install Comments           0       0                0.000s
-------------------  -------  ------  -------  ----------
Total import time          ?       5    0.2 kB       0.184s
```

使用上面的简单命令，我们已将 mariadb forumdb 数据库迁移到 postgresql forumdb 数据库。现在让我们去检查是否已全部迁移；让我们重新连接到 postgresql 服务器上的 forumdb：

```
postgres@pg-destination:~$ psql forumdb
```

现在我们可以使用名为 forumdb 的用户访问 postgresql 数据库 forumdb：

```
postgres@pg-destination:~$ psql -U forumdb forumdb

forumdb=> select * from categories;
 pk |      title      |          description
----+-----------------+-------------------------------
  1 | Database        | Database related discussions
  2 | Unix            | Unix and Linux discussions
  3 | Programming Languages | All about programming languages
(3 rows)

forumdb=> select * from users;
 pk | username  |    gecos     |      email
----+-----------+--------------+--------------------
  1 | fluca1978 | Luca Ferrari | fluca1978@gmail.com
  2 | sscotty71 | Enrico Pirozzi | sscptty71@gmail.com
(2 rows)
```

## 摘要

在本章中，我们探索了一些 PostgreSQL 可用的扩展和工具。我们选择不提供 PostgreSQL 可用的一切的概述，而是专门聚焦于一些能够节省 DBA 时间的工具和扩展。我们谈到了 pgbackrest，这是一个用于管理恢复和 PITR 的非常有用的工具。我们还谈到了 pgloader，这是一个用于从其他 DBMS 迁移到 PostgreSQL 的强大工具。然后我们展示了一个从 MariaDB 迁移到 PostgreSQL 的简单示例。

## 验证你的知识

- 如果 myfield 是 mytable 表的 varchar(200) 字段，语句 create index on mytable(myfield) 是否会改善类似 foo% 的 where 条件查询？
  
  不会，上面的语句不会改善类似 ‘foo%’ 的 where 条件查询；要使其成为可能，我们必须使用 create index on mytable using btree(my field varchar_pattern_ops);。请参阅 *Exploring the pg_trgm extension* 部分了解更多详情。
- 是否可以使用索引和各种 like 和 ilike 查询？

可以，使用 pg_trgm。参阅 *Exploring the pg_trgm extension* 部分了解更多详情。

- 什么是时间点恢复 PITR？
  
  在给定保留期的情况下，时间点恢复是指能够恢复到过去任意时间点的能力。参阅 *Disaster recovery with pgbackrest* 部分了解更多详情。

- PostgreSQL 是否有工具可以帮助我们管理连续备份和时间点恢复？
  
  是的，它有多个工具可以帮助我们管理连续备份和 PITR，其中之一叫做 pgbackrest。参阅 *Disaster recovery with pgbackrest* 部分了解更多详情。

- 是否可以直接连接到另一台 PostgreSQL 服务器？
  
  可以，使用 PostgreSQL 外部数据包装器扩展。参阅 *Using foreign data wrappers and the postgres_fdw extension* 部分了解更多详情。

## 参考文献

- Pg_trgm 官方文档：https://www.postgresql.org/docs/current/pgtrgm.html
- 外部数据包装器 Wiki 页面：https://wiki.postgresql.org/wiki/Foreign_data_wrappers
- PostgreSQL 外部数据包装器官方文档：https://www.postgresql.org/docs/current/postgres-fdw.html
- PgBackrest 官方文档：https://pgbackrest.org
- PgLoader 官方文档：https://pgloader.readthedocs.io

## 在 Discord 上了解更多

要加入本书的 Discord 社区——在那里你可以分享反馈、向作者提问并了解新版本——请扫描下面的二维码：

https://discord.gg/jYWCjF6Tku

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_722_0.png)

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_723_0.png)

packt.com

订阅我们的在线数字图书馆，即可完全访问超过 7,000 本书籍和视频，以及帮助你规划个人发展和推进职业生涯的行业领先工具。更多信息，请访问我们的网站。

## 为什么订阅？

- 使用来自 4,000 多名行业专业人士的实用电子书和视频，花更少时间学习，花更多时间编码
- 使用专为你打造的技能计划改善学习
- 每月获得一本免费电子书或视频
- 完全可搜索，便于轻松访问重要信息
- 复制、粘贴、打印和书签内容

在 www.packt.com，你还可以阅读一系列免费技术文章，注册多种免费新闻通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

## 其他书籍
你可能会喜欢

如果你喜欢这本书，你可能对 Packt 的这些其他书籍感兴趣：

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_725_0.png)

## Mastering PostgreSQL 15 - Fifth Edition

Hans-Jürgen Schönig

ISBN: 9781803248349

- 充分利用 PostgreSQL 中的索引功能并优化查询性能
- 使用存储过程并管理备份和恢复
- 掌握复制和故障转移技术

- 提高数据库服务器的安全性并有效处理加密
- 排查 PostgreSQL 实例以解决常见和不常见的各种问题
- 轻松地将数据库从 Oracle 迁移到 PostgreSQL

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_727_0.png)

## SQL for Data Analytics - Third Edition

Jun Shan, Matt Goldwasser, Upom Malik, Benjamin Johnston

ISBN: 9781801812870

- 使用 SQL 清理、准备和合并不同数据集
- 使用 GROUP BY 子句汇总基本统计信息
- 使用 WINDOW 函数执行高级统计计算
- 将数据导入数据库以与其他表合并
- 将 SQL 查询结果导出到各种来源
- 分析 SQL 中的特殊数据类型，包括地理空间、日期/时间 和 JSON 数据
- 优化查询并自动化任务
- 思考数据问题并使用 SQL 找到答案

## Packt 正在寻找像你这样的作者

如果你有兴趣成为 Packt 的作者，请访问 authors.packtpub.com 并立即申请。我们已与成千上万的开发人员和技术专业人士合作，就像你一样，帮助他们与全球技术社区分享他们的见解。你可以提交通用申请、申请我们正在招聘作者的特定热门主题，或提交你自己的想法。

## 分享你的想法

现在你已经完成了 *Learn PostgreSQL*，我们很想听听你的想法！如果你是从 Amazon 购买的本书，请点击此处直接前往该书在 Amazon 的评论页面并分享你的反馈，或在你购买的网站上留下评论。

你的评论对我们和技术社区都很重要，将帮助我们确保提供卓越质量的内容。




