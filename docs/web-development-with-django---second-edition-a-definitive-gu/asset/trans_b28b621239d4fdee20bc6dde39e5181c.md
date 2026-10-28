

# SQL 删除操作

下面是一个使用 `delete` 命令从数据库中删除记录的示例：

```sql
delete from book where title='The Sparrow Warrior';
```

`delete` 是 SQL 中用于删除操作的关键字。在这里，只有当标题为 The Sparrow Warrior 时，才会执行此操作。删除操作后，book 表将如下所示：

|   | title | publisher | author |
|---|---|---|---|
| 1 | Ninja Warrior | East Hill Publications | Edward Smith |
| 2 | The European History | Northside Publications | Eric Robbins |

![](img/4840a43efd335abd10b93708225692d6_97_0.png)

这些是 SQL 的基本操作。我们不会进一步深入所有 SQL 命令和语法，但欢迎你探索更多关于使用 SQL 进行数据库基础操作的内容。

> **注意**
> 
> 如需进一步阅读，你可以先探索使用 `join` 语句的某些高级 SQL `select` 操作，它们用于跨多个表查询数据。有关 SQL 的详细课程，你可以参考 *The SQL Workshop* (`https://www.packtpub.com/product/the-sql-workshop/9781838642358`)。

在本节中，我们学习了如何通过使用 SQL 对数据库执行 CRUD 操作来与数据库交互。在下一节中，我们将学习 Django 的 ORM，即一个抽象层，如何与数据库交互。

## 探索 Django ORM

Web 应用程序不断与数据库交互，其中一种方式是使用 SQL。如果你决定不使用 Django 等 Web 框架而仅使用 Python 编写 Web 应用程序，可以使用 `psycopg2` 等 Python 库（用于 PostgreSQL）直接使用 SQL 命令与数据库交互。但是在开发具有多个表和字段的 Web 应用程序时，SQL 命令很容易变得过于复杂，从而难以维护。因此，像 Django 这样流行的 Web 框架提供了一层抽象，使我们能够轻松地处理数据库。Django 中帮助我们实现这一点的部分称为 ORM。

Django ORM 将面向对象的 Python 代码转换为实际的数据库结构，例如具有数据类型定义的数据库表，并通过简单的 Python 代码实现所有数据库操作。

因此，在执行数据库操作时，我们不必处理 SQL 命令。这有助于更快地进行应用程序开发并便于维护应用程序源代码。

Django 支持 SQLite、PostgreSQL、Oracle Database 和 MySQL 等关系数据库。Django 的数据库抽象层确保相同的 Python/Django 源代码可以在这些关系数据库中的任何一个中使用，只需对项目设置进行很少的修改即可。由于 SQLite 是 Python 库的一部分，并且 Django 默认配置为 SQLite，因此在本章的范围内，我们在学习 Django 模型和迁移时将使用 SQLite。接下来，我们将了解如何使用 Django 进行数据库配置。

## 数据库配置和创建 Django 应用程序

正如我们在 *Chapter 1, An Introduction to Django* 中已经看到的，当我们创建一个 Django 项目并运行 Django 服务器时，默认的数据库配置是 SQLite3。数据库配置位于项目目录中，`settings.py` 文件内。

> **注意**
> 
> 请确保查看 bookr 应用的 `settings.py` 文件。通读整个文件一次将有助于你理解后续的概念。你可以在此处找到该文件：https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/blob/main/Chapter02/final/bookr/bookr/settings.py。

因此，对于我们的示例项目，数据库配置将位于以下位置：`bookr/settings.py`。当创建 Django 项目时，此文件中存在的默认数据库配置如下所示：

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

`DATABASES` 变量被分配一个包含项目数据库详细信息的字典。在字典内部，有一个以 default 为键的嵌套字典。它保存了 Django 项目默认数据库的配置。我们使用以 `default` 为键的嵌套字典的原因是 Django 项目可能与多个数据库交互，而默认数据库是在未明确指定时 Django 用于所有操作的数据库。`ENGINE` 键表示使用哪个数据库引擎；在本例中为 `sqlite3`。

NAME 键定义数据库的名称，可以是任意值。但对于 SQLite3，由于数据库以文件形式创建，NAME 可以具有文件需要创建的目录的完整路径。db 文件的完整路径是通过将 BASE_DIR 中之前定义的路径与 db.sqlite3 拼接（或连接）来处理的。请注意，BASE_DIR 是 settings.py 文件中已定义的项目目录。

如果你使用其他数据库，如 PostgreSQL、MySQL 等，则必须在前述数据库设置中进行更改，如下所示：

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'bookr',
        'USER': <username>,
        'PASSWORD': <password>,
        'HOST': <host-IP-address>,
        'PORT': '5432',}}
```

这里，ENGINE 已更改为使用 PostgreSQL。需要为 HOST 和 PORT 分别提供服务器的主机 IP 地址和端口号。如名称所示，USER 是数据库用户名，PASSWORD 是数据库密码。除了配置更改外，我们还必须安装数据库驱动程序或绑定以及数据库主机和凭据。这将在后面的章节中详细介绍，但现在由于我们使用的是 SQLite3，默认配置已足够。请注意，前面的代码只是示例，展示了使用 PostgreSQL 等不同数据库时需要进行的更改，但由于我们使用的是 SQLite，我们将使用已存在的数据库配置，无需对数据库设置进行任何修改。接下来，我们将简要介绍 Django 应用以及 Django 中存在的一些默认应用。

## Django 应用

Django 项目可以有多个应用，它们通常作为独立实体存在。因此，每当需要时，一个应用也可以插入到不同的 Django 项目中。例如，如果我们开发一个电子商务 Web 应用程序，该 Web 应用程序可以有多个应用，例如用于客户支持的聊天机器人或用于在用户从应用程序购买商品时接受付款的支付网关。如果需要，这些应用也可以插入到或在不同的项目中重用。

Django 默认启用了以下应用。以下是一个项目 settings.py 文件的片段：

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

这些是用于管理站点、身份验证、内容类型、会话、消息以及用于收集和管理静态文件的一个应用程序的一组已安装或默认应用。在接下来的章节中，我们将深入研究这一点。对于本章的范围而言，我们将在下一节中了解为什么这些已安装应用需要 Django 迁移。

## Django 迁移

正如我们之前所学，Django 的 ORM 有助于简化数据库操作。操作的一个主要部分是将 Python 代码转换为数据库结构，例如具有指定数据类型的数据库字段和表。换句话说，将 Python 代码转换为数据库结构的过程称为 **migration**。例如，而不是通过运行 SQL 命令创建数据库表，你会在 Python 中为它们编写模型；这将在即将到来的 *Creating models and migrations* 部分中学习。这些模型将具有 **fields**，它们构成了数据库表的蓝图。字段反过来将具有不同的字段类型，为我们提供有关存储在其中的数据类型的信息（回想一下我们在 Exercise 2.01 – 创建书籍数据库的第 4 步中如何将字段的数据类型指定为 TEXT）。

由于我们已经设置了 Django 项目，让我们执行第一次迁移。虽然我们尚未向项目添加任何代码，但我们可以迁移 INSTALLED_APPS 中列出的应用程序。这是因为 Django 的已安装应用需要在数据库中存储相关数据以供其操作，而迁移将创建所需的数据库表来在数据库中存储数据。应在此终端或 shell 中输入以下命令来执行此操作：

```
python manage.py migrate
```

> **注意**
> 对于 macOS，你可以使用 python3 代替前述命令中的 python。

这里，manage.py 是一个在项目创建时自动创建的脚本。它用于执行管理或管理任务。通过执行此命令，我们创建了已安装应用所需的所有数据库结构。

由于我们使用 DB Browser for SQLite 来浏览数据库，让我们查看执行 migrate 命令后所做的更改的数据库。

