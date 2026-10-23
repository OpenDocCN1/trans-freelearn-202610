

虽然你在互联网上到处都能找到 PostgreSQL 扩展，但几乎每次你需要新扩展时，你都会与 PGXN 交互。PGXN 不是一个简单的网站或代码仓库，而是一个由四个部分组成的详细平台：搜索引擎、扩展管理器、**应用程序编程接口 (API)**，以及客户端。

**搜索引擎**允许用户搜索 PGXN 内容以查找特定扩展。**管理器**负责接收新扩展（或新扩展版本）并让用户获取它们（即分发扩展）。**API** 定义了应用程序如何与管理器和搜索引擎交互，因此也定义了客户端如何构建。目前有两个主要客户端可用——PGXN 网站和 pgxnclient 命令行应用程序。pgxnclient 应用程序可能是获取和安装扩展的最有效方式，这将在以下小节中详细介绍；然而，你也可以直接与 PGXN 网站交互来搜索和下载扩展。你将在本章后面看到使用 PGXN 网站的示例。

扩展是基于 **PostgreSQL 扩展系统 (PGXS)** 构建的，PGXS 是一组基本规则，扩展必须遵守这些规则才能提供一个通用的可管理接口。特别是，PostgreSQL 提供了一个统一的 Makefile，每个扩展都应使用它来提供安装、升级和移除扩展的一组通用功能。你可以通过 `pg_config` 检查 PGXS 基础 Makefile，找到其位置：

```
```sh
$ pg_config --pgxs
/usr/lib/postgresql/16/lib/pgxs/src/makefiles/pgxs.mk
```
```

pgxs.mk 是为每个扩展提供通用功能的基 Makefile，其用法在我们展示如何从头创建扩展时将变得更加清晰。

## 扩展组件

一个扩展由两个主要组件组成——**控制文件**和**脚本文件**：

- **控制文件**提供有关扩展及其管理方式的信息，例如在哪里以及如何安装它，如何升级它等等。控制文件在某种程度上是扩展的元数据。
- **脚本文件**是一个 SQL 文件，其中包含创建扩展一部分的数据库对象的语句。在某种程度上，这就是扩展的内容。脚本文件可以反过来加载完成扩展的其他文件，如共享库等。

当你要求 PostgreSQL 安装扩展时，系统会检查控制文件以获取有关扩展的信息，确保扩展尚未安装，然后在事务中执行脚本文件。结果，你可以在数据库中使用该扩展。

每个扩展都有一个版本，以便你可以精确决定要安装哪个版本。如果未指定版本，PostgreSQL 将假定你想要最新可用版本。

扩展安装在集群的 share 目录中，通常可以通过执行带 `--sharedir` 选项的 pg_config 命令找到。以下是一个示例：

```
$ pg_config --sharedir
/usr/share/postgresql/16
```

组成扩展的所有文件都将放置在共享目录中，集群期望这些文件对运行集群的用户可用（通常是操作系统用户 postgres）。一旦文件对集群可用，扩展必须在需要它的每个数据库中选择性安装；请记住，PostgreSQL 在数据库之间提供非常强的隔离，因此加载到一个数据库中的扩展不会自动在另一个数据库中可用。但是，请记住模板数据库（参见第 2 章）可作为新创建数据库的骨架，因此一旦你在模板数据库中安装了扩展，你将在所有其他创建的数据库中找到该扩展已可用。

## 控制文件

扩展控制文件必须具有与扩展相关且后缀为 .control 的名称。例如，一个有效名称可以是 learnpg.control。

控制文件是一个文本文件，你可以在其中指定指令，这些指令是安装说明和元数据，让 PostgreSQL 处理扩展安装。每个指令都有一个名称和一个值。最常见的指令如下：

- directory 指定扩展脚本路径。
- default_version 指定当用户未指定任何版本时要安装的扩展版本。
- comment 是扩展及其目的的描述。
- requires 是安装和使用本扩展所需其他扩展的可选列表，因此代表依赖列表。

## 管理扩展

每个扩展都在数据库级别管理，这意味着每个需要扩展的数据库都必须管理该扩展生命周期。换句话说，没有按集群管理扩展并将其应用于集群中每个数据库的方法。

扩展主要由三个 SQL 语句管理：CREATE EXTENSION、DROP EXTENSION 和 ALTER EXTENSION，分别用于在数据库中安装扩展、从数据库中移除扩展以及修改扩展属性或升级它们。

每个扩展由一个助记符和一个版本指定；如果未指定版本，PostgreSQL 假定你想要处理最新可用版本或已安装的版本。

在以下小节中，将解释这三个管理语句中的每一个。

### 创建扩展

CREATE EXTENSION 语句允许你在当前数据库中安装现有扩展。

该语句的概要如下：

```
CREATE EXTENSION [ IF NOT EXISTS ] extension_name
    [ WITH ] [ SCHEMA schema_name ]
                [ VERSION version ]
                [ CASCADE ]
```

扩展名称是扩展的助记符，如你所见，你可以指定要安装的扩展版本号。如果扩展依赖于任何其他扩展，CASCADE 选项允许系统自动递归执行依赖项的 CREATE EXTENSION。你可以决定扩展对象必须放置到哪个模式中，当然，这仅对可以重新定位的扩展才有意义。

正如你所能想象的，IF NOT EXISTS 允许命令在扩展已被安装时优雅地失败。更精确地说，如果扩展已在数据库中安装，则不执行任何操作。

为了更好地了解 CREATE EXTENSION 的工作原理，假设我们想在 forumdb 数据库中安装 PL/Perl 过程语言；由于 PL/Perl 扩展作为 PostgreSQL contrib 模块提供，你应该已经在集群中拥有该扩展。

因此，为了安装它，你需要执行以下操作：

```
forumdb=# CREATE EXTENSION plperl;
CREATE EXTENSION
```

请注意，PL/Perl 扩展（助记符 plperl）需要使用数据库管理员进行安装。如果你尝试再次安装同一扩展，命令将失败，除非你使用 IF NOT EXISTS 子句：

```
forumdb=# CREATE EXTENSION plperl;
ERROR:  extension "plperl" already exists

forumdb=# CREATE EXTENSION IF NOT EXISTS plperl;
NOTICE:  extension "plperl" already exists, skipping
CREATE EXTENSION
```

作为另一个简单的示例，我们可以安装 pg_stat_statements 扩展的特定版本：

```
forumdb=# CREATE EXTENSION pg_stat_statements VERSION '1.10';
CREATE EXTENSION
```

请注意，pg_stat_statements 需要更改 shared_preload_libraries 配置参数，这反过来需要重启集群。在本章的 Docker 镜像中，shared_preload_libraries 设置已为你配置好，以便使用 pg_stat_statements 扩展。如果你想手动更改 PostgreSQL 配置以使用 pg_stat_statements，你需要编辑 postgresql.conf 配置文件或执行：

ALTER SYSTEM SET shared_preload_libraries TO 'pg_stat_statements';

然后重启集群。

## 查看已安装的扩展

在 psql 终端中，可以使用 \dx 特殊命令获取已安装扩展的列表：

```
forumdb=# \dx

                         List of installed extensions

    Name    | Version | Schema | Description
```
