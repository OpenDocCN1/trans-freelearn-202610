

## 通过 SSL 连接到集群

连接 PostgreSQL 时，如果基于主机的访问具有 hostssl 条目，客户端将自动切换到 SSL 连接；否则，它将默认为标准的明文连接。

如果 pg_hba.conf 中有 host 行，则意味着它可以接受 SSL 和明文连接。因此，你需要在发起连接时强制连接使用 SSL。在 psql 中，这只能通过使用连接字符串并指定 sslmode=require 参数来启用。服务器如果接受连接，将报告所使用的 SSL 协议：

```
$ psql "postgresql://forum@localhost:5432/forumdb?sslmode=require"
psql (16.0)
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

forumdb=>
```

如果你省略 sslmode 参数或使用标准的 psql 连接参数，只要 pg_hba.conf 文件中存在匹配的 hostssl 行，连接就会被转为 SSL。例如，以下三个连接产生相同的结果（加密连接）：

```
$ psql -h localhost -U forum forumdb
psql (16.0)
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

forumdb=> \q

$ psql "postgresql://forum@localhost:5432/forumdb"
psql (16.0)
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

forumdb=> \q

$ psql "postgresql://forum@localhost:5432/forumdb?sslmode=require"
psql (16.0)
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

forumdb=>
```

同样，你可以通过设置 sslmode=disable 来指定完全不使用 SSL 连接。这时，如果 pg_hba.conf 有 hostssl 模式，连接将被拒绝，而如果 pg_hba.conf 文件有 host 行，则会作为非加密连接提供服务：

```
$ psql "postgresql://forum@localhost:5432/forumdb?sslmode=require"
psql: error: could not connect to server: FATAL:  no pg_hba.conf entry for host "127.0.0.1", user "forum", database "forumdb", SSL off
```

从错误信息中，你可以清楚地看到 pg_hba.conf 文件中没有接受明文（host 模式）连接的行，或者反过来说，只有 hostssl 行。

最后，如果你要求连接使用 SSL 但 PostgreSQL 服务器未配置为使用 SSL，将报告关于不匹配的错误信息：

```
$ psql "postgresql://forum@localhost:5432/forumdb?sslmode=disable"
psql: error: could not connect to server: server does not support SSL, but SSL was required
```

## 总结

在本章中，我们了解到 PostgreSQL 提供了非常丰富的用于管理与角色相关权限的基础设施。内部而言，PostgreSQL 通过 ACL 来处理不同数据库对象的权限，每个 ACL 都包含关于权限集合、被授予权限的用户以及授予此类权限的用户的信息。就表格数据而言，甚至可以定义基于列的权限和行级权限，以阻止用户访问特定数据子集。

权限可以通过嵌套角色以动态继承的方式授予，或按需授予，让你有机会微调角色应如何使用特权。

最后，当配置得当后，服务器可以通过 SSL 处理网络连接，从而加密所有网络流量和数据。

在下一章中，你将全面了解事务以及 PostgreSQL 如何在并发场景中管理它们，为你的数据提供坚如磐石的稳定性。

## 验证你的知识

- 什么是角色？
  
  角色可以是单个用户或一组具有集群及其数据库访问权限的用户。角色是授予访问权限和定义权限的基本单位。有关更多详细信息，请参阅 *理解角色* 部分。
- INHERITS 子句有什么作用？
  
  INHERITS 子句使一个角色继承，即即时动态地获取其继承角色所授予的所有权限。如果没有 INHERITS 子句，角色仍然具有其所属角色的权限，但需要显式 SET ROLE 才能使用此类权限。有关更多详细信息，请参阅 *从其他角色继承的角色* 部分。
- 什么是访问控制列表（ACL）？
  
  ACL 是附加到数据库对象的权限集合的规范，是 PostgreSQL 实现和存储权限的方式。有关更多详细信息，请参阅 *ACLs* 部分。
- 添加角色权限或移除角色权限的语句是什么？
  
  GRANT 语句为角色添加权限，而 REVOKE 语句从角色移除权限。特殊关键字 ALL 可用于授予或撤销对象的全部可用权限，而多个权限可以在两个命令中同时指定。有关更多详细信息，请参阅 *授予和撤销权限* 部分。
- 什么是行级安全（RLS）？
  
  RLS 是一种根据执行查询的角色来限制查询结果集的方式。RLS 可应用于读取查询和写入查询。有关更多详细信息，请参阅 *RLS* 部分。

## 参考

- CREATE ROLE 语句官方文档：https://www.postgresql.org/docs/current/sql-createrole.html
- ALTER ROLE 语句官方文档：https://www.postgresql.org/docs/current/sql-alterrole.html
- DROP ROLE 语句官方文档：https://www.postgresql.org/docs/current/sql-droprole.html
- GRANT 语句官方文档：https://www.postgresql.org/docs/current/sql-grant.html
- REVOKE 语句官方文档：https://www.postgresql.org/docs/current/sql-revoke.html
- PostgreSQL pg_roles 目录详情：https://www.postgresql.org/docs/current/view-pg-roles.html
- PostgreSQL pg_authid 目录详情：https://www.postgresql.org/docs/current/catalog-pg-authid.html
- PostgreSQL ACL 文档：https://www.postgresql.org/docs/current/ddl-priv.html
- PostgreSQL 基于主机的访问规则详情：https://www.postgresql.org/docs/current/auth-pg-hba-conf.html
- PostgreSQL ACL 实用函数：https://www.postgresql.org/docs/current/functions-info.html

## 在 Discord 上了解更多

要加入本书的 Discord 社区——在那里你可以分享反馈、向作者提问并了解新版本——请扫描下方二维码：

https://discord.gg/jYWcJf6Tku

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_389_0.png)

## 11

## 事务、MVCC、WAL 和检查点

本章向你介绍事务，这是每个企业级数据库系统的基本组成部分。事务是数据库管理多个操作的方式，使其如同单个原子操作。PostgreSQL 具有非常丰富且符合标准的事务机制，允许用户具体定义事务属性，包括嵌套事务。

PostgreSQL 严重依赖事务来保持数据在并发连接和并行活动中的一致性，并且得益于 **预写日志（WALs）**，PostgreSQL 尽最大努力保持数据安全可靠。此外，PostgreSQL 实现了 **多版本并发控制（MVCC）**，这是一种在事务之间保持高并发的方式。

本章可分为两部分：第一部分更实用，提供事务是什么、如何使用以及如何理解 MVCC 的具体示例。第二部分更理论化，解释 WAL 的工作原理，以及它们如何使 PostgreSQL 即使在崩溃后也能恢复。

在本章中，你将学习以下主题：

- 事务属性
- 事务隔离级别
- MVCC 是什么以及它如何工作
- 保存点

