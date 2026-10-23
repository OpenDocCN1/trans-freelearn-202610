

你并不局限于使用单一的配置文件，事实上，还有特定的指令可以用来包含其他配置文件。集群的配置将在后面的章节中详细介绍。

PostgreSQL 的 HBA 文件（pg_hba.conf）是另一个文本文件，其中包含连接许可：它列出了允许连接到你的集群的数据库、用户和网络。可以把 HBA 方法看作是嵌入到 PostgreSQL 中的防火墙。举例来说，下面是 pg_hba.conf 文件的一个摘录：

```
hosts  all luca 192.168.222.1/32 md5
hostssl all enrico 192.168.222.1/32 md5
```

简而言之，前面的几行意味着用户 luca 可以使用 IPv4 地址为 192.168.222.1 的机器连接到集群中的任何数据库，而用户 enrico 可以从同一台机器连接到任何数据库，但仅限于 SSL 加密连接。所有可用的 pg_hba.conf 规则将在后面的章节中详细介绍，但就目前而言，知道此文件充当传入连接的“防火墙规则列表”就足够了。

## Summary

PostgreSQL 可以在一个集群中处理多个数据库，这些数据库存储在名为 PGDATA 的单个目录中的磁盘存储中。集群运行许多不同的进程，其中之一特别命名为 postmaster，负责为每个客户端连接生成其他进程，并跟踪维护进程的状态。

集群的配置通过基于文本的配置文件进行管理，主要文件是 postgresql.conf。可以通过在 pg_hba.conf 文本文件中放置规则来过滤传入的用户连接。

你可以通过 pg_ctl 工具与集群状态进行交互，或者根据你的操作系统，使用其他提供的程序，如 service 或 systemctl。

本章为你提供了相关信息，使你不仅能够安装 PostgreSQL，还能定期启动和停止它，将其与操作系统集成，并连接到集群。

在下一章中，你将学习如何管理用户和连接。

## Verify your knowledge

+   - What is the pg_ctl command?
+     pg_ctl 是 PostgreSQL 随附的一个命令，允许你启动、重启、停止集群并执行其他操作。它通常被用作管理整个集群的方式。有关更多详细信息，请参阅 pg_ctl 部分。
+   - What is a template database?
+     模板数据库是可以作为基础克隆另一个（新）数据库的数据库，该数据库最初将包含相同的对象。有关更多详细信息，请参阅 The template databases 部分。
+   - What is the psql command?
+     psql 是连接到 PostgreSQL 数据库的官方客户端应用程序。它是一个命令行应用程序，可用于输入 SQL 语句并从集群获取结果。它随 PostgreSQL 的每个版本一起提供。有关更多详细信息，请参阅 The psql command-line client 部分。
+   - What is a connection string?
+     连接字符串是一个 URI，指定连接到数据库所需的所有属性，通常包括用户名、主机、数据库等。有关更多详细信息，请参阅 The connection string 部分。
+   - What are the psql special commands?
+     特殊命令是所有以反斜杠符号开头的简短命令，例如 \d。它们是仅在 psql 客户端中有效的信息命令。有关更多详细信息，请参阅 A glance at the psql commands 部分。

## References

+   - PostgreSQL PGDATA 磁盘布局: https://www.postgresql.org/docs/current/storage-file-layout.html
+   - PostgreSQL initdb 官方文档: https://www.postgresql.org/docs/current/app-initdb.html
+   - PostgreSQL pg_ctl 官方文档: https://www.postgresql.org/docs/current/app-pg-ctl.html
+   - PostgreSQL 的 pgAdmin4 图形客户端: https://www.pgadmin.org/

## Learn more on Discord

要加入本书的 Discord 社区——在那里你可以分享反馈、向作者提问并了解新版本——请扫描下面的二维码：

https://discord.gg/jYWCjF6Tku

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_82_0.png)

重要的是要理解，角色是在集群级别定义的，而权限是在数据库级别定义的。这意味着同一个角色在不同的数据库中可以具有不同的特权和属性（例如，允许连接到一个数据库而不能连接到另一个数据库）。

> 由于角色是在集群级别定义的，因此它必须在整个集群中具有唯一名称。

## Managing roles

角色可以通过三种主要的 SQL 语句进行管理：CREATE ROLE 用于从头创建角色，ALTER ROLE 用于更改某些角色属性（例如登录密码），以及 DROP ROLE 用于删除现有角色。

> PostgreSQL 随操作系统工具一起提供用于管理角色：createuser 和 dropuser。这两个命令都会打开到集群的连接并执行上述 SQL 命令；因此，本章中将不解释这些工具的使用。

为了使用 SQL 语句创建新角色然后管理它们，有必要连接到集群中的一个数据库。超级用户角色 postgres 可以用于此目的，至少在初始阶段是这样，因为在数据库集群初始化时就会创建该角色。使用 postgres 角色和模板数据库是创建初始角色最常见的方式。

角色由表示角色名称的字符串标识，或者更确切地说，是该角色的账户名称。此名称必须在整个系统中唯一，这意味着你不能有两个名称相同的不同角色。名称必须由字母、数字和一些符号组成，例如下划线。

## Creating new roles

为了创建一个新的角色，无论是单个用户账户还是组容器，你需要使用 CREATE ROLE 语句。该语句具有以下简短的语法，并且有一个强制参数，即角色的用户名：

```
CREATE ROLE name [ [ WITH ] option [ ... ] ]
```

你可以在语句中指定的选项范围从账户密码、交互式登录的能力以及超级用户权限。请记住，与其他系统不同，在 PostgreSQL 中，你可以拥有任意数量的超级用户，并且每个人在集群上都具有相同的生死大权。

CREATE ROLE 语句几乎每个选项都有一个正向形式，用于为角色添加能力，以及一个负向形式（以 NO 为前缀），用于从角色中排除该能力。例如，SUPERUSER 选项添加了充当集群超级用户的能力，而 NOSUPERUSER 选项则将其从角色中移除。

在本章中，我们将重点关注登录能力，这是一组受限的选项，允许角色登录到集群。其他选项将在第 10 章《Users, Roles, and Database Security》中讨论，因为它们与角色的安全功能更相关。

> 如果你在 CREATE ROLE 时忘记了一个选项怎么办？如果改变主意想从现有角色中删除一个选项怎么办？有 ALTER ROLE 语句允许你（作为集群超级用户）在无需删除和重新创建的情况下修改现有角色。该语句将在第 10 章《Users, Roles, and Database Security》中展示，并附带一些角色的其他有趣选项。

## Role passwords, connections, and availability

每次连接到 PostgreSQL 都必须连接到特定的数据库，无论打开连接的用户是谁。连接到集群中的数据库意味着角色必须对自己进行身份验证，因此必须有一种身份验证机制，用户名和密码是最经典的。

当用户尝试连接到数据库时，PostgreSQL 会检查登录凭据以及用户的其他一些属性，以确保其被允许登录并且凭据有效。

允许你操作和管理登录尝试的主要选项如下：

+   - PASSWORD 或 ENCRYPTED PASSWORD 是等效选项，允许你为角色设置登录密码。这两个选项是为了与旧版本的 PostgreSQL 保持向后兼容而存在的，但如今集群始终以加密形式存储角色密码，因此使用 ENCRYPTED PASSWORD 与 PASSWORD 选项相比并无额外价值。
+   - PASSWORD NULL 明确强制为空（非空）密码，阻止用户使用任何密码登录。此选项可用于拒绝基于密码的身份验证。

