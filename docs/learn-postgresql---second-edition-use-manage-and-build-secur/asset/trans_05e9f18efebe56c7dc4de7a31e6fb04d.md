

为了允许用户 luca 在 schema 中创建新对象，必须授予 CREATE 权限。然而，没有 USAGE 权限，角色将无法访问 schema 中的任何内容，因此你需要同时提供这两种权限：

```
-- as user 'forum'
forumdb=> GRANT CREATE ON SCHEMA configuration TO luca;
GRANT

forumdb=> GRANT USAGE ON SCHEMA configuration TO luca;
GRANT
```

因此，luca 角色现在可以在 schema 中创建新对象：

```
-- as user 'luca'
forumdb=> CREATE TABLE configuration.conf( param text,
    value text,
    UNIQUE (param) );
CREATE TABLE

forumdb=> INSERT INTO configuration.conf
    VALUES( 'posts_per_page', '10' );
INSERT 0 1
```

没有 USAGE 权限，角色将无法再访问 schema 中的任何对象，即使它是该对象的拥有者：

```
-- as role 'forum'
forumdb=> REVOKE USAGE ON SCHEMA configuration FROM luca;
REVOKE
```

事实上，用户将无法再读取自己的数据：

```
-- as role 'luca'
forumdb=> SELECT * FROM configuration.conf;
ERROR: permission denied for schema configuration
LINE 1: SELECT * FROM configuration.conf;
```

另一方面，通常会允许角色操作特定 schema 中包含的数据，而不授予其创建新数据库对象如表的权限。

这是一个常见的场景，数据库管理员设置 schema 及其对象，让最终用户处理 schema 中包含的数据，但不允许他们修改其结构本身。这可以通过细粒度的权限设置来实现，例如：

```
-- as user 'forum'
forumdb=> GRANT USAGE ON SCHEMA configuration TO luca;
GRANT

forumdb=> REVOKE CREATE ON SCHEMA configuration FROM luca;
REVOKE
```

你可以将 schema 视为其他数据库对象的容器。为了访问容器，你必须拥有 USAGE 权限，而为了创建新对象，你必须拥有 CREATE 权限。尽管如此，USAGE 并不会提供对 schema 内任何对象的无限制访问。相反，它根据你对这些对象的权限提供对对象的访问。

## Schema 中的所有对象

由于 schema 是数据库对象的命名容器，因此它们可以用作通过 ALL <objects> IN SCHEMA 子句对 schema 中包含的每个对象应用不同特权的快捷方式。

举例来说，为了对 schema 中包含的所有表应用一组相同的权限，你可以执行以下操作：

```
-- as user 'forum'
forumdb=> REVOKE ALL
    ON ALL TABLES IN SCHEMA configuration
    FROM luca;
REVOKE

forumdb=> GRANT SELECT, INSERT, UPDATE
    ON ALL TABLES IN SCHEMA configuration
    TO luca;
GRANT
```

这可以大大简化大型 schema 的管理。

为了演示权限，让我们创建一个非常简单的例程 `get_max`，它返回两个整数之间的最大值：

```
```sql
forumdb=> CREATE FUNCTION get_max( a int, b int )
RETURNS int AS $$
BEGIN
  IF a > b THEN
    RETURN a;
  ELSE
    RETURN b;
  END IF;
END $$ LANGUAGE plpgsql;
```
```

现在，让我们阻止除 luca 之外的任何角色执行该例程：

```
```sql
forumdb=> REVOKE EXECUTE ON ROUTINE get_max FROM PUBLIC;
REVOKE
forumdb=> GRANT EXECUTE ON ROUTINE get_max TO luca;
GRANT
```
```

除 luca 之外的任何角色在调用该函数时都会收到 `permission denied` 错误：

```
```sql
-- executing as enrico
forumdb=> SELECT forum.get_max( 10, 20 );
ERROR: permission denied for function get_max
```
```

由于 `get_max` 是一个函数，我们本可以使用 FUNCTION 关键字而不是包罗万象的 ROUTINE 来编写 GRANT 和 REVOKE 权限。这只是个人偏好问题。

特别是，当你想同时并用单个语句对 schema 内的所有函数和过程应用权限时，ROUTINE 关键字就非常方便，例如以下内容：

```
```sql
-- as user forum
forumdb=> GRANT EXECUTE ON ALL ROUTINES IN SCHEMA forum;
```
```

## 与数据库相关的权限

与数据库相关的权限有很多：CONNECT 允许或拒绝传入连接，而不考虑基于主机的访问控制；TEMP 允许在数据库中创建临时对象（例如表）；CREATE 允许在数据库中创建新对象。

一般语法如下：

```
GRANT <permission> ON DATABASE <database> TO <role>;
REVOKE <permission> ON DATABASE <database> FROM <role>;
```

例如，如果你需要将所有用户锁定在数据库之外，例如因为你必须进行维护工作，你可以发出以下 REVOKE 命令：

```
forumdb=# REVOKE CONNECT ON DATABASE forumdb FROM PUBLIC;
REVOKE
```

新的传入连接将被拒绝，并出现权限被拒绝错误：

```
$ psql -U luca forumdb
psql: error: could not connect to server: FATAL:  permission denied for database "forumdb"
DETAIL:  User does not have CONNECT privilege.
```

现在，如果你希望 luca 角色是唯一能够连接到数据库并创建对象但不能创建临时对象的角色，你需要发出以下命令：

```
forumdb=# REVOKE ALL ON DATABASE forumdb FROM public;
REVOKE

forumdb=# GRANT CONNECT, CREATE ON DATABASE forumdb TO luca;
GRANT
```

## 其他 GRANT 和 REVOKE 语句

还有其他 GRANT 和 REVOKE 组控制表空间、类型和外部数据包装器的权限。这里不会讨论它们，但可以在官方 PostgreSQL 文档中找到它们，现在我们对如何将权限应用于不同对象有相当清晰的工作流程，它们应该很容易理解。

## 分配对象所有者

你已经看到对象的拥有者对该对象拥有所有可用权限。有时，你可能希望将对象的拥有权更改为另一个角色，后者随之获得所有权限。通常，更改所有权是使用特殊的 ALTER 语句完成的，例如以下内容：

```
ALTER <object> OWNER TO <role>;
```

例如，要更改表的拥有权，你可以发出以下命令：

```
forumdb=# ALTER TABLE forum.categories OWNER TO luca;
ALTER TABLE
```

而要更改函数的拥有权，你可以发出以下命令：

```
-- equivalent to: ALTER FUNCTION get_max OWNER TO luca;
forumdb=# ALTER ROUTINE forum.get_max OWNER TO luca;
ALTER ROUTINE
```

对于所有其他类型的对象也存在类似的语句。

超级用户可以通过将其设置为任何现有用户来更改每个数据库对象的拥有者，而普通用户只能更改为他们所属的角色的所有权。

## 检查 ACL

为了查看已授予角色和对象的权限，你可以使用前面提到的 psql 特殊命令 \dp（describe permissions），它报告特定对象（例如表）的 ACL 配置。该命令对特殊目录 pg_class 执行查询，其中包含名为 relacl 的特定字段——一个 ACL 数组。你可以按如下方式查看：

```
forumdb=> \dp users

                         Access privileges

 Schema |   Name   | Type  |    Access privileges    | Column privileges | Policies
--------+----------+-------+-------------------------+-------------------+----------
 forum  | users    | table | forum=arwdDxt/forum     | gecos:            +
        |          |       |                         | forum_stats=w/forum |
(1 row)

forumdb=> SELECT relname, relacl
         FROM pg_class WHERE relname = 'users';
 relname |      relacl
---------+---------------
 users   | {forum=arwdDxt/forum}
```

正如你所见，\dp 命令的输出和查询的输出是相同的，只是输出格式不同。

你也可以使用特殊的函数 aclexplode 来获取有关 ACL 的更详细的描述信息。该函数返回一组记录，每条记录都包含授予者和被授予者的 OID 以及授予权限的文本描述。因此，可以构建如下查询：

```
forumdb=> WITH acl AS (
    SELECT relname,
        (aclexplode(relacl)).grantor,
        (aclexplode(relacl)).grantee,
        (aclexplode(relacl)).privilege_type
    FROM pg_class )
SELECT g.rolname AS grantee,
    acl.privilege_type AS permission,
    gg.rolname AS grantor
FROM acl
JOIN pg_roles g ON g.oid = acl.grantee
JOIN pg_roles gg ON gg.oid = acl.grantor
WHERE acl.relname = 'users';
```

这将返回分配给表 categories 的所有单个权限，如下所示：

```
grantee | permission | grantor
---------+------------+--------
forum    | INSERT     | forum
forum    | SELECT     | forum
forum    | UPDATE     | forum
forum    | DELETE     | forum
forum    | TRUNCATE   | forum
forum    | REFERENCES | forum
forum    | TRIGGER    | forum
```

## RLS

在本书的上一部分，你看到了 PostgreSQL 通过其权限机制允许角色（用户和组）访问数据库中的不同对象以及这些对象中包含的数据。

