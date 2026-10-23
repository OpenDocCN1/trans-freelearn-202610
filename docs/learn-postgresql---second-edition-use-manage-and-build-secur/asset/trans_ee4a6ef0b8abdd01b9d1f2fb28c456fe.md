

-   `CONNECTION LIMIT <n>` 允许用户同时打开不超过 `<n>` 个到集群的连接，而不限定于某个特定数据库。这通常有助于防止用户浪费集群上的资源。
-   `VALID UNTIL` 允许你指定一个（未来的）时刻，届时该角色将过期。

为某个角色设置密码并不意味着该角色就能连接到集群：为了允许交互式登录，该角色还必须具有 LOGIN 选项。换句话说，下面的语句不会允许该用户登录：

```
postgres=# CREATE ROLE luca
    WITH PASSWORD 'xxx';
```

默认选项是 NOLOGIN（它阻止交互式登录）。因此，为了定义交互式用户，记得在创建角色时添加 LOGIN 选项：

```
template1=# CREATE ROLE luca
    WITH LOGIN PASSWORD 'xxx';
```

多个选项可以按任意顺序书写，因此上面的代码表示相同的语句，但可读性较差：

```
postgres=# CREATE ROLE luca
    WITH PASSWORD 'xxx' LOGIN;
```

VALID UNTIL 选项允许你指定一个未来的日期甚至时间戳（即一个时刻），届时角色密码将过期，并且不再允许登录集群。这对于将一组用户标记为未来可解雇很有用。

当然，这个选项只对交互式角色有意义，也就是那些具有 LOGIN 能力的角色。例如，下面的角色将在 2030 年圣诞节之后被禁止登录：

```
postgres=# CREATE ROLE luca
    WITH LOGIN PASSWORD 'xxx'
    VALID UNTIL '2030-12-25 23:59:59';
```

## 将角色用作组

组是包含其他角色的角色。就这么简单！

通常，当你想创建一个组时，你只需要创建一个没有 LOGIN 选项的角色，然后将所有成员逐个添加到包含角色中。将角色添加到包含角色会使后者成为一个组。

为了将角色创建为特定组的成员，可以使用 `IN ROLE` 选项。该选项接受组名（该组本身也是另一个角色），新创建的角色将成为该组的成员。例如，在下面的代码块中，你可以看到 `book_authors` 组的创建以及角色成员 `luca` 和 `enrico` 的添加：

```
postgres=# CREATE ROLE book_authors
    WITH NOLOGIN;
CREATE ROLE
postgres=# CREATE ROLE luca
    WITH LOGIN PASSWORD 'xxx'
    IN ROLE book_authors;
CREATE ROLE
postgres=# CREATE ROLE enrico
    WITH LOGIN PASSWORD 'xxx'
    IN ROLE book_authors;
CREATE ROLE
```

> CREATE ROLE 的 IN GROUP 子句是 IN ROLE 子句的过时同义词。

也可以使用特殊的 GRANT 语句向组中添加成员。GRANT 语句是允许精细权限调整的通用 SQL 语句（更多内容见第 10 章“用户、角色和数据库安全”）；PostgreSQL 扩展了 SQL 语法，允许将角色授予另一个角色。当你将一个角色授予另一个角色时，后者将成为前者的成员。换句话说，假设所有角色都已存在且没有任何特定关联，下面的语句将角色 `enrico` 添加到 `book_authors` 组中：

```
postgres=# GRANT book_authors TO enrico;
```

每个组可以有一个或多个管理员成员，他们被允许向组中添加新成员。ADMIN 选项允许用户指定将作为新创建组的管理员的成员。例如，在下面的代码块中，你可以看到名为 `book_reviewers` 的新组的创建，并将 `luca` 指定为管理员；这意味着用户 `luca`，即使不是集群超级用户，也能够向 `book_reviewers` 组中添加新成员：

```
postgres=# CREATE ROLE book_reviewers
    WITH NOLOGIN
    ADMIN luca;
CREATE ROLE
```

显然，只有在管理员角色已经存在的情况下，ADMIN 选项才能在 CREATE ROLE 中使用；在示例中，`luca` 角色必须在组之前创建，因为他将成为管理员。

GRANT 语句可以解决这个问题——WITH ADMIN OPTION 子句允许角色成员具有管理权限。

例如，下面的代码展示了如何使 `enrico` 用户也成为 `book_reviewers` 组的管理员。请注意，你必须完整地拼写 WITH ADMIN OPTION，如下所示：

```
postgres=# GRANT book_reviewers
    TO enrico
    WITH ADMIN OPTION;
GRANT ROLE
```

如果组角色具有 LOGIN 选项会怎样？该组仍然是一个角色容器，但它也可以充当一个能够登录的单一用户帐户。虽然这是可能的，但更常见的做法是拒绝组角色登录，以避免混淆。

## 删除现有角色

为了删除现有角色，你需要使用 DROP ROLE 语句。该语句的语法非常简单：

```
DROP ROLE [ IF EXISTS ] name [, ...]
```

你只需要指定要删除的角色名称，或者，如果需要删除多个角色，你可以将它们指定为逗号分隔的列表。

要被删除，角色必须存在；因此，如果你尝试删除一个不存在的角色，你将收到错误：

```
postgres=# DROP ROLE this_role_does_not_exist;
ERROR:  role "this_role_does_not_exist" does not exist
```

如你所见，PostgreSQL 会警告你，如果角色不存在，它无法删除该角色。

你无法破坏 PostgreSQL！PostgreSQL 会保护自身免受你的错误影响，并且在保护你的数据安全方面做得非常出色！前面关于删除不存在角色的示例，就是 PostgreSQL 保护自身免受错误影响以确保服务始终稳定的一个例子。

DROP ROLE 语句支持 IF EXISTS 子句，该子句阻止 PostgreSQL 抱怨删除缺失的角色：

```
postgres=# DROP ROLE IF EXISTS this_role_does_not_exist;
NOTICE:  role "this_role_does_not_exist" does not exist, skipping
DROP ROLE
```

如你所见，这次 PostgreSQL 没有引发错误；相反，它显示一条关于角色不存在的通知。但是，它执行了语句，什么也没做，但报告成功而不是失败。为什么这可能有用？想象你有一个负责删除多个角色的自动化任务：如果 DROP ROLE 报告失败，你的任务可能会中断，而使用 IF EXISTS，你可以放心，PostgreSQL 不会因为缺少角色而导致中止。

有几个语句支持 IF EXISTS 子句，你将在后续章节中看到。其想法是避免在你不感兴趣捕获错误时报告错误，在自动化程序中，你应该尽可能使用此子句。

如果你删除一个组会发生什么？成员角色将保持不变，但当然，与组的关联将丢失（因为组已被删除）。换句话说，删除组不会级联到其成员。

## 检查现有角色

既然你知道了如何创建和删除角色，那么如何检查现有角色，包括你自己的角色？有不同方法可以获取有关现有角色的信息，所有这些方法都依赖于 PostgreSQL 目录，这是集群内省的唯一来源。

为了获取有关你正在运行的角色信息，请使用特殊关键字 CURRENT_ROLE：你可以通过 SELECT 语句查询它（此类语句将在后续章节中介绍，所以现在只需照搬使用，如下所示）：

```
postgres=# SELECT current_role;
current_role
-------------
postgres
(1 row)
```

如果你使用另一个用户连接到数据库，你将看到不同的结果：

```
$ psql -U luca postgres
psql (16.0)
Type "help" for help.

postgres=> SELECT current_role;
current_role
-------------
luca
(1 row)
```

了解自己的角色很重要，但获取现有角色及其属性的信息可能更具启发性。psql 提供了特殊的 `\du` （描述用户）命令，用于列出系统中所有可用的角色：

```
$ psql -U postgres
psql (16.0)
Type "help" for help.

postgres=# \du

               List of roles
 Role name | Attributes
```

