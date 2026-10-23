

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_467_0.png)

Figure 12.3: The extension download page

要继续进行，你首先需要解压下载的归档文件：

```
$ unzip orafce-4.5.0.zip
```

现在你可以进入为此扩展创建的目录并编译它（系统上需要安装编译器和所有源代码构建工具）：

```
$ cd orafce-4.5.0
$ make
```

你需要 PGXS Makefiles 才能从头编译扩展。通常，此类 Makefiles 会随 PostgreSQL 的开发工具一起安装。例如，在基于 Red Hat 的 Linux 发行版上，你需要安装 postgresql16-dev 包。在本章的 Docker 镜像中，所有编译和安装扩展所需的工具都已准备就绪。

如果编译成功，扩展将被创建并准备好添加到 PostgreSQL 共享库目录。为了将扩展文件移入集群，你需要访问数据库目录，例如使用 sudo：

```
$ sudo make install
```

从这里开始，你可以在需要该扩展的每个数据库中使用 CREATE EXTENSION 语句。

## 使用已安装的扩展

一旦扩展被安装——也就是说，无论是手动还是通过 pgxn *部署*到 PostgreSQL 集群——你就可以在需要它的每个数据库中创建该扩展。

orafce 扩展必须由超级用户创建，因此你需要以管理员身份连接到数据库才能执行 CREATE EXTENSION 语句：

```
$ psql -U postgres forumdb
psql (16.0)
Type "help" for help.

forumdb=# CREATE EXTENSION orafce;
CREATE EXTENSION
```

如果现在检查数据库中已安装的扩展，你会看到刚刚创建的 orafce，版本为 4.5.0——与我们使用 pgxn 或在网站上搜索扩展时找到的版本相同：

```
forumdb=# \x \dx
Expanded display is on.
List of installed extensions
```

| Name | Version | Schema | Description |
|---|---|---|---|
| orafce | 4.5 | public | Functions and operators that emulate a subset of functions and packages from the Oracle RDBMS |
| pg_stat_statements | 1.10 | public | track planning and execution statistics of all SQL statements executed |
| plpgsql | 1.0 | pg_catalog | PL/pgSQL procedural language |

一旦扩展已安装到数据库中，每个用户都可以使用它。作为一个简单的测试，你可以查询 Oracle 拥有的 DUAL 表，而 orafce 为你的遗留查询创建了该表以继续运行：

```
$ psql -U luca forumdb
psql (16.0)
Type "help" for help.

forumdb=> SELECT * FROM oracle.dual;
```

| dummy |
|-------|
| X     |

(1 row)

请注意，扩展创建的表位于 oracle 命名空间中。通过使用 search_path，很容易避免在任何对象名称前输入 oracle：

```
forumdb=> SET search_path TO "$user", public, oracle;
SET
forumdb=> SELECT * FROM dual;
```

| dummy |
|-------|
| X     |

(1 row)

## 移除已安装的扩展

可能会出现你不再需要某个扩展，因此想要将其从集群中移除的情况。移除未使用的扩展是一个好习惯，因为它能保持集群整洁，不依赖于你真正不需要的对象。

如果某个数据库不再需要扩展及其相关内容，你可以发出 DROP EXTENSION 语句，扩展就会从你的数据库中消失。当然，如果扩展是由数据库超级用户安装的，你也需要以超级用户身份发出该语句。关于 orafce 的例子，作为超级用户，你可以执行以下操作：

```
$ psql -U postgres forumdb
psql (16.0)
Type "help" for help.

forumdb=# DROP EXTENSION orafce;
DROP EXTENSION
```

正如你所想象的，检查扩展列表时将不再显示 orafce 条目，并且所有功能，包括 DUAL 表，都已消失：

```
forumdb=# \dx
List of installed extensions
```

| Name | Version | Schema | Description |
|---|---|---|---|
| pg_stat_statements | 1.10 | public | track planning and execution statistics of all SQL statements executed |
| plpgsql | 1.0 | pg_catalog | PL/pgSQL procedural language |

(2 rows)

```
forumdb=# SELECT * FROM oracle.dual;
ERROR: relation "oracle.dual" does not exist
LINE 1: SELECT * FROM oracle.dual;
```

从单个数据库中移除扩展并不会将其从你已执行过显式 CREATE EXTENSION 函数的其他数据库中移除。它也不会从集群共享目录中移除扩展文件和库。

从集群中移除（取消部署）扩展的确切方式取决于你最初安装它的方式。

### 通过 pgxnclient 移除扩展

pgxn 的 uninstall 命令执行与 install 命令完全相反的操作：它会移除与扩展相关的所有文件。命令行选项相同，这使我们能够执行如下简单的命令：

```
$ pgxn uninstall orafce --sudo --verbose
```

所有与扩展相关的文件都将被从集群的共享库目录中移除。扩展因此永远消失，如果你需要再次安装它，将需要从第一步重新开始。

### 移除手动编译的扩展

要移除你手动从源代码编译的扩展，你需要再次使用 make，这次使用 uninstall 命令，在你解压下载的压缩归档的目录中：

```
$ cd orafce-4.5.0
$ sudo make uninstall
```

> 避免使用不同工具混合管理扩展。如果你通过 pgxnclient 安装了扩展，最好用同一客户端将其移除；反之，如果你是从源代码安装的，请使用相同的方法将其移除。

总之，你已经了解了如何使用 PGXN 客户端或手动通过 PGXN 基础设施处理扩展。在下一节中，你将学习如何构建自己的扩展。

## 创建你自己的扩展

在本节中，我们将从头构建一个扩展，以便你更好地理解它们是如何构成的。其目的是让你了解如何将你自己的 SQL 脚本甚至转换为扩展，并利用扩展在可管理性方面带来的所有优势。

### 定义示例扩展

为了演示如何构建自己的扩展，我们将创建一个适用于论坛数据库的简单功能集，提供一些额外功能。特别是，我们将定义一个名为 tagext 的扩展，它将提供一个实用函数，给定 tag 表中的特定标签，将返回该标签的完整路径，包括所有祖先。

例如，Linux 标签是 Operating Systems 标签的子标签，因此 Linux 标签的路径为 Operating System > Linux。

特别是，我们希望我们的扩展为我们提供一个名为 tag_path 的函数，给定一个标签，提供如下示例中的标签路径：

```
forumdb=> SELECT tag_path( 'Kubuntu' );
```

| tag_path |
|----------|
| Operating Systems > Linux > Ubuntu > Kubuntu |

(1 row)

在以下各节中，你将看到如何通过实现示例扩展来实现上述结果。所有必需的文件都可在本书源代码仓库中找到。

### 创建扩展文件

让我们先从控制文件开始，在其中插入有关扩展的一些基本信息。创建一个名为 tagext.control 的文件并将其放入文件夹中，例如 /src/tagext。该文件的内容如下：

```
comment = 'Tag Programming Example Extension'
default_version = '1.0'
superuser      = false
relocatable    = true
```

在本章示例中使用的 Docker 镜像中，你将在 /src/tagext 文件夹中找到扩展文件，并能够进入该文件夹安装扩展。

上述控制文件包含一个描述扩展给其他管理员的注释，指定 default_version，即如果用户未指定版本时要安装的版本，并规定此扩展可以由任何用户安装（superuser = false）并移动到用户希望的任何模式（relocatable = true）。

然后是 Makefile，即构建和安装扩展的文件：

```
EXTENSION = tagext
DATA = tagext--1.0.sql

PG_CONFIG = pg_config
```

