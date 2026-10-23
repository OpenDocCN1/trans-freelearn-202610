# 10

# Users, Roles, and Database Security

PostgreSQL 是一个坚如磐石的数据库，对安全性给予了极大的关注，提供了非常丰富的用于处理权限、特权和安全策略的基础设施。本章基于第 3 章《管理用户和连接》中介绍的基本概念，重新审视角色概念，并扩展知识，特别关注授予角色的安全性和特权（角色可以是用户，也可以是用户组）。你将学习如何配置角色的各个方面，以仔细管理安全性，从连接到数据库内数据的访问。

PostgreSQL 还提供了一种强大的机制，即行级安全性（RLS），它允许对策略进行精细定义，以屏蔽某些用户的数据部分。

在本章中，你还将了解访问控制列表（ACL）以及 PostgreSQL 在内部处理权限的方式，这是授予或撤销特权的结果。最后，你将简要了解 PostgreSQL 提供的用于安全存储角色密码的密码加密算法。

本章涵盖以下主题：

+   - 理解角色
+   - ACLs
+   - 授予和撤销权限
+   - RLS
+   - 角色密码加密
+   - SSL 连接





在本章中，你将看到普通用户输入的带有 `forumdb=>` 提示符的命令，以及数据库管理员输入的带有 `forumdb=#` 提示符的命令，两者交错出现。除非另有明确说明，所有与授予和撤销权限相关的示例都将以 forum 数据库用户的身份运行。

## 与超级用户相关的属性

借助 SUPERUSER 属性，角色将被创建为集群管理员，即对集群内所有对象拥有所有权限的角色，最显著的是能够添加、删除和修改用户；修改 PostgreSQL 配置；终止用户连接；以及停止集群。

一个集群中可以拥有任意数量的超级用户。然而，超级用户是一类不受任何限制的用户，因此除非绝对必要，否则最好不要将全部权限授予不受信任的用户。尽可能避免使用超级用户角色是另一个好习惯，有助于防止对集群及其数据造成意外损害。

## 与复制相关的属性

REPLICATION 属性用于指定新角色能够使用复制协议，即 PostgreSQL 用于在集群之间复制数据的一种特定网络协议。

REPLICATION 选项允许角色不受任何特定限制地访问集群内的所有数据。因此，通常只授予那些用于复制的角色。

出于安全考虑，除非另有指定，否则默认设置 NOREPLICATION 选项。

## 与 RLS 相关的属性

RLS 是一种策略执行机制，用于阻止某些角色访问特定表中的特定元组。换句话说，它在表行级别应用安全约束，因此得名**行级安全**。

驱动 RLS 的选项只有一个：BYPASSRLS。如果角色拥有该选项，则该角色**绕过**（即不受）集群内每一行的所有安全约束。可想而知，该选项的默认值是将其否定（即 NOBYPASSRLS），以便尽可能让角色受到安全策略的约束。

需要注意的是，集群超级用户始终能够绕过 RLS 策略。

本章的 RLS 部分将进一步介绍 RLS。

## 修改现有角色的属性：ALTER ROLE 语句

可以想象，一旦创建，角色并非不可变：你可以通过 ALTER ROLE 语句为角色添加或移除属性。该语句的语法与创建角色时使用的语法非常相似，如下所示：

```
ALTER ROLE name [ [ WITH ] option [ ... ] ]
```

其中，name 是唯一的角色名，选项的指定方式与 CREATE ROLE 语句完全相同。

例如，假设你想让 luca 角色具备创建数据库和新角色的能力。你可以执行两条 ALTER ROLE 语句，或将选项合并如下：

```
forumdb=# ALTER ROLE luca WITH CREATEDB;
ALTER ROLE

forumdb=# ALTER ROLE luca WITH CREATEROLE;
ALTER ROLE

-- same as the above two statements
forumdb=# ALTER ROLE luca CREATEROLE CREATEDB;
ALTER ROLE
```

如果之后你改变主意，可以通过指定否定形式来移除其中一个或两个选项：

```
forumdb=# ALTER ROLE luca NOCREATEROLE NOCREATEDB;
ALTER ROLE
```

ALTER ROLE 语句始终可由集群超级用户执行，但也可以由具有 CREATEROLE 选项的非超级用户角色执行（即可以创建并因此管理其他角色的角色），只要该语句应用于非超级用户角色即可。

## 重命名现有角色

ALTER ROLE 语句还允许修改角色名称：RENAME 子句允许将角色替换为另一个唯一的角色名。例如，让我们将某个角色的较短用户名改为较长的用户名：

假设 luca 角色是一个名为 forum_stats 的组的成员，因此可以对该角色执行显式转换：

```
forumdb=> SET ROLE forum_stats;
SET
forumdb=> SELECT current_user, session_user;
current_user | session_user
----------------+------------
forum_stats     | luca
(1 row)
```

如你所见，在 SET ROLE 语句之后，CURRENT_USER 的值发生了变化，以反映用户当前实际扮演的角色，而 SESSION_USER 则保存了用户连接数据库时使用的原始值。

总而言之，CURRENT_USER(CURRENT_ROLE) 跟踪当前正在运行的角色，而 SESSION_USER 保存打开数据库连接时使用的角色。

## 角色级配置参数

除了角色属性和授予的权限之外，角色还可以附加一些配置参数，以记录其用法。本质上，可以将一组 SET 命令附加到角色上，这样每次该角色连接数据库时，这些命令都会被隐式执行。

假设用户 luca 每次连接到数据库时都需要为 client_min_messages 值执行一次 SET 命令：

```
$ psql -U luca forumdb

forumdb=> SET client_min_messages TO 'DEBUG';
SET
```

这可能很烦人，而且最重要的是，还有风险。用户可能会忘记执行 SET 命令，而该命令正是连接按预期工作所必需的。可以通过修改角色，使连接一建立就自动执行 SET 命令：

```
forumdb=# ALTER ROLE luca
    IN DATABASE forumdb
    SET client_min_messages TO 'DEBUG';
```

## ALTER ROLE

现在，每次 luca 角色连接到 forumdb 数据库时，SET 命令都会自动执行：

```
$ psql -U luca forumdb

forumdb=> SHOW client_min_messages;

client_min_messages
----------------------
debug
(1 row)
```

修改角色运行时参数的一般语法如下：

```
ALTER ROLE name IN DATABASE dbname SET parameter_name TO parameter_value
```

在此，你需要指定角色名（或使用特殊关键字 ALL 表示所有现有角色）、数据库名称，以及要修改的参数名称和值。

也可以使用 RESET ALL 子句丢弃任何角色级配置，如下例所示：

```
forumdb=# ALTER ROLE luca
    IN DATABASE forumdb
    RESET ALL;
ALTER ROLE
```

## 检查角色

有多种方法可以检查现有角色并获取其属性信息。正如第 3 章“管理用户和连接”中所见，一种快速方法是使用 psql 中的 `\du` 命令：

```
forumdb=> \du

                         List of roles
 Role name |          Attributes
-----------+----------------------------
--
book_authors | Cannot login
enrico     |
```

```
forum |
```

forum_admins | Cannot login
forum_emails | No inheritance, Cannot login
forum_stats | No inheritance, Cannot login
luca | 1 connection
postgres | Superuser, Create role, Create DB, Replication, Bypass RLS
```

Attributes 列提供了角色属性的助记说明：例如，luca 角色被限制为只能有一个连接。

除了 psql 的特殊命令外，超级用户还可以随时查询系统目录以获取现有角色的信息。主要入口是 pg_authid 表，该表为每个现有角色包含一行，其中的列反映了角色的每个属性（即通过 CREATE ROLE 或 ALTER ROLE 语句定义的内容），例如：

```
forumdb=# \x
Expanded display is on.
forumdb=# SELECT * FROM pg_authid WHERE rolname = 'luca';
-[ RECORD 1 ]---------------------------------------------
oid | 16384
rolname | luca
rolsuper | f
rolinherit | t
rolcreaterole | f
rolcreatedb | f
rolcanlogin | t
rolreplication | f
rolbypassrls | f
rolconnlimit | 1
rolpassword | SCRAM-SHA-256$4096:...f2QU/7KAVM=
rolvaliduntil |
```

每个角色都有一个唯一的名称和一个 OID 值，OID 以数值形式表示角色。这与 Unix 系统（以及许多其他系统）中用户的表示方式类似，其中角色的数值仅用于内部使用。

许多角色属性都是布尔值，其中 f 表示 false（即没有该选项），t 表示 true（即具有该选项）。





例如，在前面的示例中，你可以看到 `rolcreatedb` 为 false，这意味着该角色是使用 NOCREATEDB 选项创建（或更改）的。

角色密码（`rolpassword` 字段）以哈希形式表示，其中包含所使用算法的标识符（在上面为 SCRAM-SHA-256）。

还有另一个可能的目录，名为 `pg_roles`，它显示了关于 `pg_authid` 的相同信息：

```
```text
forumdb=> SELECT * FROM pg_roles WHERE rolname = 'luca';
-[ RECORD 1 ]+----------
rolname       | luca
rolsuper      | f
rolinherit    | t
rolcreaterole | f
rolcreatedb   | f
rolcanlogin   | t
rolreplication | f
rolconnlimit  | 1
rolpassword   | ********
rolvaliduntil |
rolbypassrls  | f
rolconfig     |
oid           | 16384
```
```

为什么我们需要对同一数据有两个相似的视图？只有集群超级用户可以查询 pg_authid，而每个用户都可以查询 pg_roles，因为不会泄露角色密码的风险（正如你可以看到密码字段已被屏蔽）。

关于组成员关系呢？你可以查询特殊的 `pg_auth_members` 目录来获取有关哪些角色是哪些其他角色成员的信息。作为示例，以下查询提供了组列表：

```
```sql
forumdb=> SELECT r.rolname, g.rolname AS group,
                 m.admin_option AS is_admin
             FROM pg_auth_members m
             JOIN pg_roles r ON r.oid = m.member
             JOIN pg_roles g ON g.oid = m.roleid
             ORDER BY r.rolname;
```
```

```
rolname | group | is_admin
----------+---------------+----------
enrico | book_authors | f
enrico | forum_admins | f
luca | forum_stats | f
luca | book_authors | f
pg_monitor | pg_read_all_settings | f
pg_monitor | pg_read_all_stats | f
pg_monitor | pg_stat_scan_tables | f
test | forum_stats | f
(8 rows)
```

## 从其他角色继承的角色

我们在 *第 3 章，管理用户和连接* 中已经看到，一个角色可以包含其他角色，因此充当组。

当一个角色成为另一个角色的成员时，它会获得包含（组）角色的所有权限。然而，有时此类特权是动态授予的，即成员角色将透明地拥有特权，有时特权是静态授予的，即成员角色需要显式成为组角色才能使用其特权。角色的 INHERIT 属性区分了角色默认如何使用权限。GRANT 语句有一个可选的 WITH INHERIT 子句，可以在将角色添加到组时指定：如果子句的值为 true，则添加的角色将立即动态获得组的所有权限，否则不会。

如果未指定 WITH INHERIT 选项，则角色的 INHERIT 属性将被隐式使用。为了理解差异和影响，让我们看看 forum_admins 和 forum_stats 角色是如何构建的：

```
forumdb=# CREATE ROLE forum_admins WITH NOLOGIN;
CREATE ROLE

forumdb=# CREATE ROLE forum_stats WITH NOLOGIN;
CREATE ROLE

forumdb=# REVOKE ALL ON forum.users FROM forum_stats;
REVOKE
```

```
forumdb=# REVOKE ALL ON forum.users FROM forum_admins;
REVOKE

forumdb=# GRANT ALL ON SCHEMA forum TO forum_admins;
GRANT

forumdb=# GRANT USAGE ON SCHEMA forum TO forum_stats;
GRANT

forumdb=# GRANT ALL ON forum.users TO forum_admins;
GRANT

forumdb=# GRANT SELECT (username, gecos) ON forum.users TO forum_stats;
GRANT

forumdb=# GRANT forum_admins TO enrico;
GRANT ROLE

forumdb=# GRANT forum_stats TO luca;
GRANT ROLE
```

首先，这两个角色是在没有直接登录能力的情况下创建的；这是因为我们不希望组本身被用作用户。相反，我们希望属于该组的<|message|>

```
GRANT

-- assign permissions
forumdb=# GRANT SELECT (email)
    ON forum.users TO forum_emails;
GRANT

-- assign the role to the group
-- implicitly uses WITH INHERIT false
forumdb=# GRANT forum_emails TO forum_stats;
GRANT ROLE
```

现在，luca 是 forum_stats 和 forum_emails 的成员，但由于后者不会动态地将其权限传播给其成员，luca 无法获得读取 email 列的权限：

```
% psql -U luca forumdb

forumdb=> SELECT username, gecos, email FROM forum.users;
ERROR:  permission denied for table forum.users
```

然而，成为另一个角色的成员意味着一个角色始终可以显式地成为它自己所属的组，冒充后者，从而获得授予该组的全部权限。这就像前一个角色代表组角色行事一样。为了能够代表包含的组行事，角色必须发出显式的 SET ROLE 语句。显然，一个角色不能成为任意其他角色：它只能代表显式授予的角色行事，并且只能代表包含的组行事。因此，如果用户 luca 执行显式的 SET ROLE 成为 forum_emails 角色，它将能够查询 users 表中的 email 列：

```
forumdb=> SELECT current_role;
 current_role
---------------
 luca
(1 row)

forumdb=> SET ROLE TO forum_emails;
SET

forumdb=> SELECT current_role;
 current_role
---------------
 forum_emails
(1 row)

forumdb=> SELECT email FROM forum.users;
       email
-------------------
fluca1978@gmail.com
sscotty71@gmail.com
(2 rows)

forumdb=> SELECT gecos FROM forum.users;
ERROR:  permission denied for table users
```

现在让我们更改 GRANT 的 INHERIT 属性，使权限能够动态传播：

```
forumdb=# GRANT forum_emails TO forum_stats
             WITH INHERIT true;
GRANT ROLE
```

现在让我们看看 luca 角色是否可以同时使用 forum_stats 和 forum_emails 组的两个特权：

```
$ psql -U luca forumdb

forumdb=> SELECT gecos, username, email FROM forum.users;
       gecos       | username  |          email
-------------------+-----------+-------------------------
LUCA FERRARI       | fluca1978 | fluca1978@gmail.com
ENRICO PIROZZI      | sscotty71 | sscotty71@gmail.com
(2 rows)
```

太好了！现在角色可以在同一时间使用两个组的特权，而无需显式更改其当前角色。

## Role inheritance overview

当一个角色是属于一个或多个其他角色时，权限解析过程如下：

- 如果角色拥有请求的权限，则不再进行其他检查，操作被允许（例如，已显式授予该角色）。
- 如果角色没有请求的权限，则会在包含的组（如果有）中搜索该权限。如果在其中一个组中找到该权限，并且该组是以 INHERIT 属性授予的（隐式或显式），则权限会动态应用。否则，如果在任何父组中找到权限，但未使用 INHERIT 选项授予，则权限不会动态传播（即必须使用 SET ROLE）。

无论如何，角色始终可以通过显式的 SET ROLE 语句利用其所属组的权限，这意味着 INHERIT 属性仅用于动态传播权限，从而防止角色需要更改为另一个角色。

值得注意的是，通过显式 SET ROLE 更改角色，声明用户将执行需要特定权限的特定任务。

## ACLs

PostgreSQL 将分配给角色和对象的权限存储为 ACL，并在需要时检查特定角色和数据库对象的 ACL，以了解是否可以执行命令或查询。在本节中，你将了解 ACL 是什么、如何存储，以及如何解释它们以了解 ACL 提供的权限。

重要的是要注意，ACL 以及因此的权限严格绑定到角色和数据库对象，这意味着授予对象特定权限并不意味着被授予角色将在另一个数据库中拥有相同的权限，即使该数据库中存在同名且性质相同的对象。例如，允许角色在一个数据库中运行 PL/Perl 代码并不自动赋予其在其他数据库中运行 PL/Perl 代码的权限。

ACL 是权限组的表示，其结构如下：

```
grantee=flags/grantor
```

其中：

- `grantee` 是应用权限的角色名称。
- `flags` 是表示权限的字符串。
- `grantor` 是授予权限的用户。

每当 grantor 和 grantee 名称相同时，该角色就是数据库对象的拥有者。

ACL 中可使用的标志如下表所示。可以看出，并非所有标志都适用于所有对象：例如，对函数设置“删除”权限没有意义，对表设置“执行”权限也没有意义：

| Flag | Description | Statements | Applies to |
|---|---|---|---|
| a | Append or insert new data | `INSERT` | Tables and columns |
| r | Read or get data | `SELECT` | Tables, columns, and sequences |
| w | Write or update data | `UPDATE` | Tables |
| d | Delete data | `DELETE` | Tables |
| D | Delete all data | `TRUNCATE` | Tables |
| C | Create a new object | `CREATE` | Databases, schemas, and table spaces |
| c | Connect to a database |  | Databases |
| t | Trigger or react to data changes | `CREATE TRIGGER` | Tables |
| T | Create temporary objects | `CREATE TEMP` | Tables |
| x | Cross-reference between data | `FOREIGN KEY` | Tables |
| X | Execute runnable code | `CALL` and `SELECT` | Functions, routines, and procedures |
| U | Use various objects |  | Sequences, schemas, foreign objects, types, and languages |

*Table 10.1: ACL flags*

有了可能的标志列表，就很容易解码以下与表对象相关的 ACL：

```
luca=arw/enrico
```

首先，识别涉及的角色：luca 和 enrico。luca 在等号之前；因此它是 ACL 所指的角色，这意味着此 ACL 描述了 luca 角色拥有的权限。另一个角色 enrico 在斜杠之后，因此是授予 luca 角色权限的角色。现在，关于标志，ACL 提供了追加（a）、读取（r）和写入（w）权限。以上可读作“enrico 授予 luca 对表执行 INSERT、UPDATE 和 SELECT 的权限。”

让我们现在看一个来自数据库中表的 ACL 示例：你可以使用特殊的 \dp psql 命令获取有关表的信息：

```
forumdb=> \dp categories

                         Access privileges
 Schema |   Name   | Type  | Access privileges | Column privileges | Policies
--------+----------+-------+-------------------+-------------------+----------
 forum  | categories | table | forum=arwdDxt/forum+ |  | 
        |          |       | luca=arw/enrico    + |  | 
        |          |       | =d/forum          |  | 
(1 row)
```

ACL 清楚地显示在命令输出的 Access privileges 列中。ACL 的第一行关于 categories 表的所有者：由于被授予者和授予者相同（forum），这是表所有者。表所有者 forum 拥有所有权限：追加（a）、读取（r）、写入（w）、删除（d）、截断（D）、触发（t）和交叉引用（x）。因此，可以这样读：“表所有者可以对该表执行任何操作。”

ACL 的第二行就是上面解码的那一行，可读作“luca 可以 INSERT、UPDATE 和 SELECT 数据。”ACL 的第三行有点晦涩：授予者仍然是 forum 角色，但在等号之前没有被授予者。这意味着 ACL 指的是每个角色。由于 ACL 仅包含删除（d）权限，这意味着数据库中的每个角色都可以删除表中的行，正如用户 forum 所希望的那样。

ACL 会被处理以查找匹配项。假设 luca 角色想要从表中删除一行，因此发出 DELETE 语句。该语句是允许还是被拒绝？





阅读与 luca 角色相关的 ACL（luca=arw/forum），可以清楚地看到该角色无法从表中删除任何内容。然而，存在一个“兜底”ACL，允许每个角色执行 DELETE 操作（=d/forum）；因此，即使是 luca 角色也被允许删除元组。

另一方面，不同的角色（例如，forum_stats）不被允许对表执行任何 INSERT，因为既没有为该角色，也没有为任何其他未明确指出的角色授予特定权限。

但这些 ACL 是如何产生的呢？首先，它们都是由用户 enrico 创建的，因此假设他是连接到数据库的用户，GRANT 语句的序列应该如下所示：

```
-- generates ACL: luca=arw/forum
forumdb=> GRANT SELECT, UPDATE, INSERT
             ON forum_categories
             TO luca;
GRANT

-- generates ACL: =d/forum
forumdb=> GRANT DELETE ON forum_categories
             TO PUBLIC;
GRANT
```

现在你已经了解了 PostgreSQL 如何管理 ACL 以及如何将 GRANT 和 REVOKE 命令转换为 ACL，是时候看看授予角色的默认权限是什么了。

## 默认 ACL

如果创建了一个对象并且既未对其应用 GRANT 也未应用 REVOKE，会发生什么？系统不会为此类对象存储 ACL，正如你通过创建一张简单的空表并检查其权限所看到的那样：

```
forumdb=> CREATE TABLE perm_test( t text );
CREATE TABLE

forumdb=> \dp perm_test
                         Access privileges
Schema |  Name   | Type  | Access privileges | Column privileges |
Policies
----------+----------------+--------+--------------------------------+--------------------------------
-----
forum | perm_test | table |                                |                                |
(1 row)
```

由于表中没有关联的 ACL，PostgreSQL 如何知道哪些角色被允许对该对象执行什么操作？答案在于默认权限：PostgreSQL 对对象应用一组默认权限并对照其默认列表进行检查。

最值得注意的是，如果角色是对象的拥有者，则它拥有该对象可用的所有权限。如果角色不是拥有者，则会检查 PUBLIC 权限，即使用为该类型对象分配给特殊 PUBLIC 角色的所有权限。

出于安全原因，PUBLIC 相关权限列表相当简短，可以总结如下：

- 对例程的执行权限（X）
- 连接数据库并在数据库上创建临时对象（cT）
- 使用语言、类型和域（U）

正如你所见，默认情况下，PUBLIC 权限集不允许角色执行任何真正危险的操作，因此授权角色对对象执行操作唯一的方法是仔细地 GRANT 和 REVOKE 权限。

第一次对对象执行 GRANT 时，PostgreSQL 还会为该对象的拥有者引入默认 ACL。在前述表的情况下，拥有者将具有类似 luca=arwdDxt/luca 的 ACL（假设 luca 角色是拥有者）。因此，假设我们授予 enrico 操作数据的权限：

```
forumdb=> \dp perm_test

                    Access privileges
Schema | Name      | Type  | Access privileges | Column privileges |
Policies
----------+-----------+-------+-------------------+-------------------
-----
forum | perm_test | table |                   |                   |
(1 row)

forumdb=> GRANT SELECT, INSERT,
          UPDATE, DELETE
ON perm_test TO enrico;
GRANT
forumdb=> \dp perm_test

Access privileges
Schema | Name | Type | Access privileges | Column privileges |
Policies
-----------------+--------+------+----------------------------+---------------
------
forum | perm_test | table | forum=arwdDxt/forum+|
| | | enrico=arwd/forum |
(1 row)
```

正如你所见，GRANT 后，ACL 由两个条目组成，一个是我们刚刚授予用户 enrico 的，另一个是隐式应用于表拥有者 luca 的。

还需要注意的是，ACL 存储的是角色可以做什么，而不是不能做什么。ACL 中未列出的所有内容都将被拒绝。为了更好地理解这一点，请考虑撤销 enrico 角色的权限：

```
forumdb=> REVOKE TRUNCATE ON perm_test FROM enrico;
REVOKE

forumdb=> \dp perm_test

Access privileges
Schema | Name | Type | Access privileges | Column privileges |
Policies
-----------------+--------+------+----------------------------+---------------
------
forum | perm_test | table | forum=arwdDxt/forum+|
| | | enrico=arwd/forum |
(1 row)
```

正如你所见，此次撤销并未更改 enrico 角色的 ACL 行。该角色本来就没有此权限；因此撤销它对 ACL 没有任何影响。

同样，撤销 PUBLIC 的权限也不会影响已存在的 ACL。如果我们从每个用户中移除 INSERT 权限，enrico 仍将保留自己的权限，因为 ACL 是累加存储的：

```
forumdb=> REVOKE INSERT ON perm_test FROM PUBLIC;
```

```
(1 row)
```

这里没有什么新内容，但函数的创建（类型 f）呢？现在很容易看出以下内容：

```
forumdb=> SELECT acldefault( 'f', r.oid )
           FROM pg_roles r
           WHERE r.rolname = CURRENT_ROLE;
       acldefault
------------------------
{=X/forum,forum=X/forum}
(1 row)
```

这次产生了两个 ACL：第一个授予所有用户可执行权限，而后者则指定拥有者为 forum 角色，也具有可执行权限。

你可以通过其 OID 和对象类型来检查特定用户的全部默认 ACL，其中主要类型有 r 表示表，c 表示列，l 表示语言，f 表示例程和过程。还有其他类型可用。请参阅官方文档。现在是时候看看如何在实践中操作 ACL 和权限了。在下一节中，你将学习如何处理权限管理。

## 授予和撤销权限

正如你在第 3 章《管理用户和连接》中所见，一个角色与一组权限相关联，这些权限是通过 GRANT 语句提供，并通过 REVOKE 语句移除。权限在内部以 ACL 的形式存储，正如你在上一节中所见。

本节重新审视 GRANT 和 REVOKE 语句，以帮助你更好地理解如何针对不同的数据库对象使用它们。

GRANT 语句的语法如下：

```
GRANT <permission, permission, ...> ON <database-object> TO <role>;
```

在这里，你列出要与目标角色关联的所有权限，用于指定的数据库对象。还可以通过 WITH GRANT OPTION 子句扩展 GRANT 语句，这将使目标角色能够将其收到的相同权限授予另一个角色。

REVOKE 语句的语法类似：

```
REVOKE <permission, permission, ...> ON <database-object> FROM <role>;
```

有一个特殊的角色，名为 PUBLIC，可以在处理权限管理时使用。它不是一个具体的角色，而是一个标记，表示“所有可用角色”。换句话说，如果你将权限授予 PUBLIC，就相当于隐式地将此权限授予所有可用角色。

但“所有可用角色”是什么意思？它指的是所有现有和未来的角色。PUBLIC 角色代表系统中在权限管理时以及将来任何时候存在的任何角色。

根据以上所述，为了防止任何用户访问你的对象，你应始终从特殊 PUBLIC 角色中移除所有权限，然后有选择地为特定角色提供所需的权限。

在以下各节中，我们将详细介绍分配和移除权限的不同权限，并根据数据库对象对其进行分类。作为一般经验法则，权限列表取决于你可以在数据库对象上运行的操作。

在许多情况下，特殊关键字 ALL 是与数据库对象相关的所有权限的替代。

### 与表相关的权限

我们已经看到了与数据库表相关的主要权限。它们指的是可以在表对象上运行的主要语句，例如 SELECT、INSERT、UPDATE、DELETE 和 TRUNCATE。此外，还可以使用特殊关键字 TRIGGER 和 REFERENCES 在表中创建触发器和外键。

当然，特殊关键字 ALL 包含上述所有权限。

例如，为了让 forum_stats 角色具有读取、更新和向 categories 表插入数据的权限，而不授予执行其他操作的权限，你可以在以 forum 用户连接后执行以下操作：

```
forumdb=> REVOKE ALL
    ON forum.categories FROM forum_stats;

REVOKE

forumdb=> GRANT SELECT, INSERT, UPDATE
```



ON forum.categories TO forum_stats;
GRANT

forumdb=> \dp categories

Access privileges
Schema | Name | Type | Access privileges | Column privileges | Policies
----------------+---------+------+--------------------+---------------+----------
forum | categories | table | forum=arwdDxt/forum | | 
| | | forum_stats=arw/forum | |
(1 row)

第一个 REVOKE 语句并非强制，但这是一个良好的实践。因为我们希望确保该角色仅具有我们即将授予的权限，而没有更多，移除角色的所有权限可确保任何之前的 GRANT 语句不会保留。

正如你所见，forum_stats 用户的 ACL 反映了我们授予的权限。

## 基于列的权限

由于与表对象相关的某些语句可以直接针对列，例如 SELECT 和 UPDATE，因此也可以授予或撤销列权限。概要相同，但你可以列出权限所指的列。

列权限只能应用于 SELECT、UPDATE、INSERT 和 REFERENCES 权限，因为只有这些权限可以明确引用列；特殊关键字 ALL 封装了整个权限列表。

例如，假设 forum_stats 用户只能通过 gecos 和 username 列与表 users 交互，能够读取两者但仅更新第一个。权限可以由用户 forum 按如下方式分配：

```
forumdb=> REVOKE ALL ON forum.users
    FROM forum_stats;
REVOKE

forumdb=> GRANT SELECT (username, gecos),
    UPDATE (gecos)
    ON forum.users TO forum_stats;
GRANT
```

正如前面所强调的那样，包含第一个 REVOKE 语句是一个良好的实践，以确保在分配我们想要的权限之前重置角色的权限。然后，我们授予 SELECT 和 UPDATE 权限，指定每个语句能够交互的列。

前述 GRANT 语句的副作用是 forum_stats 角色不再能够发出列列表比 GRANT 中指定的更宽的 SELECT 或 UPDATE：

```
forumdb=> SELECT current_role;
current_role
-------------
luca
(1 row)

-- denied, not all the columns can be read!
forumdb=> SELECT * FROM forum.users;
ERROR:  permission denied for table users

-- allowed
forumdb=> SELECT gecos, username FROM forum.users;
   gecos   | username
-----------+----------
LUCA FERRARI | fluca1978
ENRICO PIROZZI | sscotty71
(2 rows)

-- denied, the 'username' column cannot be updated!
forumdb=> UPDATE users SET username = upper( username );
ERROR:  permission denied for table users

-- allowed
forumdb=> UPDATE forum.users SET gecos = lower( gecos );
UPDATE 2
```

让我们现在检查 users 表的权限：

```
forumdb=> \dp forum.users

Access privileges
```

```
Schema | Name | Type | Access privileges | Column privileges | Policies
------------+-------+------+-------------------+-------------------+--------
forum | users | table | forum=arwdDxt/forum | username:          |
          |       |      |                   |   forum_stats=r/forum |
          |       |      |                   | gecos:             |
          |       |      |                   |   forum_stats=rw/forum |
          |       |      |                   |   email:            |
          |       |      |                   |   forum_emails=r/forum
|
(1 row)
```

这里有两个重要之处与之前所有的示例不同。首先，Access privileges 列不包含与 forum_stats 角色相关的任何条目，即使我们明确授予了权限。其次，Column privileges 列现在已填满与 forum_stats 角色相关的行。

Column privileges 中的每一行都指表的某一列，并包含每个允许角色的 ACL。例如，username 列具有 ACL forum_stats=r/forum，这意味着 forum_stats 角色对该列具有读取权限（即 SELECT）。gecos 列具有 ACL forum_stats=rw/forum，这表示 forum_stats 角色能够对该列进行读取和写入（即 SELECT 和 UPDATE）。

总之，如果角色已授予一个或多个针对所有列的权限，则 ACL 放置在 Access privileges 列下，如果权限与特定列相关，则 ACL 显示在 Column privileges 列下。

你必须小心不要使权限相互冲突。例如，假设我们错误地向 forum_stats 角色授予了 SELECT 权限：

```
forumdb=> GRANT SELECT
    ON forum.users TO forum_stats;
GRANT
```

如果我们在执行该语句后检查权限，可以看到 ACL 已作为访问权限插入：

```
forumdb=> \dp users

Access privileges
Schema | Name | Type | Access privileges | Column privileges | Policies
--------+-------+------+--------------------+--------------------+--------
forum | users | table | forum=arwdDxt/forum | username:          |
| | | forum_stats=r/forum | forum_stats=r/forum |
| | | | gecos:              |
| | | | forum_stats=rw/forum |
(1 row)
```

在 SELECT 语句的情况下会考虑哪个权限？

很容易测试并发现 PostgreSQL 认为最后授予的权限比授予列的权限更宽松。因此，该角色已被授予选择表上每一列的能力：

```
forumdb=> SELECT * FROM users;
pk | username | gecos | email
----+----------+---------------+-------------------
1 | fluca1978 | luca ferrari | fluca1978@gmail.com
2 | sscotty71 | enrico pirozzi | sscotty71@gmail.com
(2 rows)
```

修复问题可能并不像你想象的那么简单。撤销你不希望角色访问的列的读取权限可能不会产生你期望的效果，即使由表所有者（forum 用户）执行：

```
forumdb=> REVOKE SELECT (pk, email)
    ON users FROM forum_stats;
REVOKE
```

如果你记得，REVOKE 不存储 ACL 而是修改现有的 ACL。在这种特定情况下，由于与之前的 pk 和 email 列无关，REVOKE 语句不会改变任何内容：

```
forumdb=> \dp users
```

后两个权限仅用于允许你对序列进行更细粒度的权限配置。

GRANT 和 REVOKE 命令的一般概要如下：

```
GRANT <permission> ON SEQUENCE <sequence> TO <role>;
REVOKE <permission> ON SEQUENCE <sequence> FROM <role>;
```

特殊关键字 ALL 封装了适用于序列的所有权限。

为了理解序列的权限如何工作，让我们考虑用于生成 categories 表主键的序列：categories_pk_seq。

首先，移除 luca 角色的所有权限，以便他无法再与序列交互：

```
forumdb=> REVOKE ALL
    ON SEQUENCE categories_pk_seq
    FROM luca;
REVOKE
```

现在，如果 luca 角色尝试从序列获取新值，他会收到权限被拒绝的错误：

```
forumdb=> SELECT nextval( 'categories_pk_seq' );
ERROR:  permission denied for sequence categories_pk_seq
```

授予序列 USAGE 权限允许 luca 角色再次查询序列：

```
forumdb=> GRANT USAGE ON SEQUENCE categories_pk_seq TO luca;
GRANT
```

现在，该角色可以成功应用 setval 函数：

```
forumdb=> SELECT setval( 'categories_pk_seq', 10 );
 setval
--------
     10
(1 row)

forumdb=> SELECT nextval( 'categories_pk_seq' );
 nextval
--------
11
(1 row)
```

请记住，USAGE 权限封装了 SELECT 和 UPDATE 权限，因此一旦你向角色授予了 USAGE，序列就可以被查询并设置为特定值。

## 与模式相关的权限

模式是各种对象的命名空间，主要是表和视图，但也包括函数、例程和其他数据库对象。可以应用于模式的主要有两个权限：CREATE，允许在模式内创建对象；以及 USAGE，允许角色“使用”模式中的对象（假设它对对象具有适当的权限）。

乍一看这可能有点令人困惑，因为如果角色没有 USAGE 权限，即使它是所有者，也无法访问对象。

此处使用 GRANT 和 REVOKE 的一般概要涉及显式的 ON SCHEMA 子句（以区别于针对表的权限）：

```
GRANT <permission> ON SCHEMA <schema> TO <role>;
REVOKE <permission> ON SCHEMA <schema> FROM <role>;
```

与其他类似语句一样，关键字 ALL 封装了所有权限。为了更好地理解这两种不同的权限，让我们创建一个配置模式并查看如何启用对其的访问：

```
-- as user forum
forumdb=> CREATE SCHEMA configuration;
CREATE SCHEMA
```

该模式由 forum 用户创建，因此用户 luca 在其中没有任何权限，所以他无法创建表：

```
-- as user 'luca'
forumdb=> CREATE TABLE configuration.conf( param text,
    value text,
    UNIQUE (param) );

ERROR:  permission denied for schema configuration
LINE 1: CREATE TABLE configuration.conf( param text, value text, UNI...




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





特别是关于表， 你已经学会了如何将访问限制到表格数据中仅特定的列列表。

PostgreSQL 提供了另一种限制表格数据访问的有趣机制：RLS。其思想是 RLS 决定角色可以访问哪些元组，无论是读模式还是写模式。因此，如果基于列的权限提供了一种限制表格数据垂直形状的方法，那么 RLS 则提供了一种限制数据本身水平形状的方法。

何时适合使用 RLS？想象一下你有一个包含与用户相关的数据的表，并且你不希望你的用户能够篡改其他用户的数据。在这种情况下，将每个用户的访问限制在其自己的元组上提供了良好的隔离，从而防止数据被篡改。另一个相当常见的场景是多租户系统，在该系统中，你将相同的数据存储在完全相同的表中，但针对不同的公司。你不希望一家公司能够窥探或检查另一家公司的数据，因此再次 RLS 可以证明是有用的。

当然，RLS 并不是万能药，你可能想出的许多涉及 RLS 的解决方案本可以用其他技术实现，但了解这一重要特性可以使你的数据更能抵御滥用。

RLS 基础设施基于所谓的策略。策略是一组规则，根据这些规则，某些元组应对用户可用。根据你应用的策略，你的角色（即用户）将能够读取和/或写入某些元组。

将 RLS 应用于表通常是一个两步过程：首先，你必须定义一个策略（或多个），然后你必须在表上启用该策略。请注意，超级用户、所有者和具有特殊 BYPASSRLS 属性的角色将不受 RLS 约束。

ATTENTION：在数据库备份的情况下，例如通过 pg_dump，执行备份的用户必须能够绕过 RLS 策略；也就是说，它必须具有 BYPASSRLS 属性，否则备份将失败。显然，超级用户角色（postgres）或任何其他具有超级用户选项的角色都将成功。

策略根据逻辑条件，即过滤条件来定义元组的可用性。元组可以仅用于读取、仅用于写入或两者兼有。策略的一般语法如下：

```
CREATE POLICY <name>
ON <table>
FOR <statement>
TO <role>
USING <filtering condition>
WITH CHECK <writing condition>
```

这里，适用以下内容：

- name 是策略的名称；这用于在系统中查找它。
- table 是你想要应用策略的表。
- statement 是 SELECT、UPDATE、DELETE、INSERT 中的任意一个，或特殊关键字 ALL 表示所有可用语句。
- filtering condition 是用于限制可用元组结果集的条件，通常是角色希望从表中检索的元组。
- writing condition 是一个可选子句，提供对写入元组的限制。

策略可以用 DROP POLICY 命令删除，并可以用特定的 ALTER POLICY 命令重写。

让我们现在看几个例子，以便更好地理解策略是如何构建的。假设我们希望允许数据库用户仅查看属于他们自己的 posts 表中的元组。因此，条件是针对 SELECT 语句将用户本身与之匹配。

策略可能如下所示：

```
forumdb=> CREATE POLICY show_only_my_posts
    ON posts
    FOR SELECT
    USING ( author = ( SELECT pk FROM users
        WHERE username = CURRENT_ROLE ) );
CREATE POLICY
```

策略被命名为 show_only_my_posts 并针对 posts 表的每个 SELECT 语句起作用。只有当 USING 子句匹配时，元组才会被返回到最终结果集中，这意味着只有当 author 在 users 表中找到并且是当前数据库用户时。

创建策略并不意味着策略是活动的；你需要通过特定的 ALTER TABLE 命令在它所引用的表上启用它：

```
forumdb=> ALTER TABLE posts ENABLE ROW LEVEL SECURITY;
ALTER TABLE
```

例如，如果你观察非过滤 SELECT 命令的查询计划，你会看到 CURRENT_ROLE 过滤器被应用，就像在 USING 子句中一样：

```
forumdb=> EXPLAIN SELECT * FROM posts;

                        QUERY PLAN
---------------------------------------------------------------
  Seq Scan on posts  (cost=8.17..76.17 rows=1000 width=74)
    Filter: (author = $0)
    InitPlan 1 (returns $0)
      ->  Index Scan using users_username_key on users  (cost=0.15..8.17 rows=1 width=4)
        Index Cond: (username = (CURRENT_ROLE)::text)
```

过滤器已由 PostgreSQL 应用，即使查询中未提及它。这意味着 PostgreSQL 总是“被迫”为你执行查询并过滤结果，因此你不能期望使用 RLS 获得任何性能提升。毕竟，元组必须在某处被排除！

现在，如果你尝试以违反 CHECK 条件的方式修改元组，PostgreSQL 会抱怨并且不会允许你执行更改：

```
forumdb=> UPDATE posts
    SET last_edited_on = last_edited_on - '2 weeks'::interval;
ERROR:  new row violates row-level security policy for table "posts"
```

你始终可以通过 psql 中的特殊 \dp 命令检查 RLS（以下输出已被修剪以适应页面边界）：

```
forumdb=> \dp posts

Access privileges | Policies
------------------+----------
                  | show_only_my_posts (r):
                  |   (u): (author = ( SELECT users.pk
                  |     FROM users
                  |   WHERE (users.username = (CURRENT_ROLE)::text))
                  | manage_only_my_posts:
                  |   (u): (author = ( SELECT users.pk
                  |     FROM users
                  |   WHERE (users.username = (CURRENT_ROLE)::text))
                  |   (c): ((author = ( SELECT users.pk
                  |     FROM users
                  |   WHERE (users.username = (CURRENT_ROLE)::text))) AND ((last_edited_on + '1 day'::interval) >= CURRENT_TIMESTAMP))
(1 row)
```

最后，你可以通过发出特定的 ALTER TABLE 命令在表上禁用或重新启用策略，例如以下内容：

```
forumdb=> ALTER TABLE posts DISABLE ROW LEVEL SECURITY;
ALTER TABLE

-- to enable the RLS again
forumdb=> ALTER TABLE posts ENABLE ROW LEVEL SECURITY;
ALTER TABLE
```

策略可以根据用户发出的特定语句进行组合。默认情况下，策略被创建为许可的，这意味着它们通过逻辑“OR”组合。这意味着只要有一个策略授予操作，语句就会成功。另一方面，作为限制性创建的策略将通过逻辑“AND”合并，因此每个策略都必须成功才能执行语句。

策略的许可类型只能在创建时设置，因此例如，要使 show_only_my_posts 策略变为限制性，你需要删除它并重新创建它，明确指定其许可性：

```
forumdb=> DROP POLICY show_only_my_posts ON posts;
DROP POLICY

forumdb=> CREATE POLICY show_only_my_posts
    ON posts
    AS restrictive
    FOR SELECT
    USING ( author = ( SELECT pk FROM users
        WHERE username = CURRENT_ROLE ) );

CREATE POLICY
```

## Role password encryption

与角色关联的登录密码始终以加密形式存储，即使角色是在没有 ENCRYPTED PASSWORD 属性的情况下创建的。PostgreSQL 通过 postgresql.conf 配置文件中的 password_encryption 选项确定用于加密密码的算法。默认情况下，该选项的值设置为 scram-sha-256：

```
forumdb=> SHOW password_encryption;
password_encryption
-----------------
scram-sha-256
(1 row)
```

PostgreSQL 在版本 10 中引入了 SCRAM-SHA-256 加密算法；在那之前，加密算法设置为较不健壮的 MD5 算法，这也是唯一其他（但现在不推荐）可用的选项。

重要的是要注意，你无法在不重置活动角色的所有密码的情况下更改实时系统的密码加密算法。换句话说，如果你决定从旧的 MD5 迁移到更新的 SCRAM-SHA-256（或反之），你需要发出适当的 ALTER ROLE 语句为数据库中定义的每个角色插入新密码。

> 由于 pg_authid.rolpassword 字段以加密算法开头，即 MD5 或 SCRAM-SHA-256，因此可以简单地检查系统目录并找到尚未使用新加密算法更新的角色。





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
