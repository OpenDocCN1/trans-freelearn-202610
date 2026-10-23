

`get_max()` 函数是一个简单的 PL/PgSQL 函数，已安装在本章的 Docker 容器中，仅用于演示 PostgreSQL 在删除扩展时如何足够智能地移除所有依赖对象。

总之，你已经学会了如何手动管理一个扩展，从安装到升级或移除；在下一节中，你将学习如何以更自动化的方式执行相同的步骤。

## 探索 PGXN 客户端

PGXN 客户端是一个外部应用程序，用 Python 编写，作为 PGXN 的命令行界面。该应用程序名为 pgxnclient，通过命令运行，这些命令是 install、download、uninstall 等操作，允许数据库管理员与 PGXN 和扩展进行交互。

在某种程度上，pgxnclient 的工作方式与 Perl 的 cpan（或 cpanm）命令、Raku 的 zef、Python 的 pip 等相同。

作为一个外部应用程序，pgxnclient 不随 PostgreSQL 分发，因此在使用它之前你需要在机器上安装它。安装 pgxnclient 并不是使用 PostgreSQL 扩展的强制要求，但它可以让你的生活变得轻松许多。

在下面的小节中，你将看到如何在主要的 Unix 和类 Unix 操作系统上安装 pgxnclient，但在那之前，重要的是让你知道，一旦安装完成，你的系统上会出现两个可执行文件：pgxn 和 pgxnclient。你可以将这两个可执行文件视为彼此的别名，即使这并不完全正确（一个包装了另一个）；然而，你可以使用其中任何一个，得到完全相同的结果。在本章中，我们将使用 pgxn 作为主要的可执行文件。

在本书仓库中的 Docker 镜像里，pgxnclient 程序已经安装好了，所以你不需要安装它。

有关如何安装 pgxnclient 的详细说明超出了本书的范围，因此如果你无法让应用程序正常工作，请仔细检查项目文档和安装说明。

## 在 Debian GNU/Linux 及衍生版上安装 pgxnclient

pgxnclient 已为 Debian GNU/Linux 及衍生版打包，这意味着你可以简单地让 apt 安装它：

```
$ sudo apt install pgxnclient
```

程序安装完成后，你可以使用 --version 选项简单地测试它，它会打印你安装的版本号：

```
$ pgxn --version
pgxnclient 1.3.2
```

## 在 Fedora Linux 和基于 Red Hat 的发行版上安装 pgxnclient

pgxnclient 也已为 Fedora 打包，因此你可以使用操作系统的包管理器安装它：

```
$ sudo dnf install -y pgxnclient
```

进程完成后，你可以查询应用程序以验证它是否正常工作：

```
$ pgxn --version
pgxnclient 1.3.2
```

## 在 FreeBSD 上安装 pgxnclient

pgxnclient 已为 FreeBSD 打包，因此你可以通过 pkg 工具或通过软件 ports 安装它。最快的方式是使用 pkg，你只需要请求安装该程序：

```
$ sudo pkg install --yes pgxnclient
```

## 从源码安装 pgxnclient

你始终可以从源码安装 pgxnclient，即使这仅建议在你的操作系统未提供打包版本，或版本相对于你的需求已过时的情况下使用。你可以从官方项目 GitHub 仓库下载最新发布的压缩版本，例如：

```
$ wget https://github.com/pgxn/pgxnclient/archive/refs/tags/v1.3.2.zip
```

一旦你拥有压缩归档，你需要解压缩它并进入随之创建的目录——以你下载的 PGXN 版本命名——在我们的例子中是 pgxnclient-1.3.2。一旦进入该目录，执行 setup.py Python 脚本将允许你安装应用程序：

```
$ unzip v1.3.2.zip
$ cd pgxnclient-1.3.2
$ sudo python setup.py install
...
Finished processing dependencies for pgxnclient==1.3.2
```

安装完成后，你可以查询应用程序以验证它是否正常工作：

```
$ pgxn --version
pgxnclient 1.3.2
```

## pgxnclient 命令行界面

PGXN 客户端应用程序提供了一个类似于其他基于命令的应用程序（如 cpanm 和 git）的命令行界面。你可以通过请求帮助来获取主要命令列表：

```
$ pgxn help
usage: pgxn [--version] [--help] COMMAND ...

Interact with the PostgreSQL Extension Network (PGXN).

optional arguments:
  --version  print the version number and exit
  --help     show this help message and exit

available commands:
  COMMAND    the command to execute. The complete list is available using 'pgxn help --all'. Builtin commands are:
  check      run a distribution's test
  download   download a distribution from the network
  help       display help and other program information
  info       print information about a distribution
  install    download, build and install a distribution
```

```
load      load a distribution's extensions into a database
mirror    return information about the available mirrors
search    search in the available extensions
uninstall
          remove a distribution from the system
unload    unload a distribution's extensions from a database
```

通常，你会使用以下子集命令：

- search 用于使用关键词搜索发行版
- info 用于更仔细地查看一个扩展
- download 用于下载（但不安装）一个扩展
- install 用于下载并在集群中安装一个扩展
- load 用于针对特定数据库执行 CREATE EXTENSION
- unload 用于针对特定数据库执行 DROP EXTENSION
- uninstall 用于从集群中移除一个扩展

你可能使用的最小命令集是 search、install 和 uninstall。

对于每个命令，如果你将命令指定为 help 命令的参数，你可以获得更详细的帮助。例如，要获取有关 search 命令的更多信息，你可以执行以下操作：

```
$ pgxn help search
usage: pgxn search [--help] [--mirror URL] [--verbose] [--yes]
                    [--docs | --dist | --ext]
                    TERM [TERM ...]

search in the available extensions

positional arguments:
  TERM        a string to search

optional arguments:
  --help      show this help message and exit
  --docs      search in documentation [default]
  --dist      search in distributions
  --ext       search in extensions
```

```
global options:
  --mirror URL  the mirror to interact with [default: https://api.pgxn.org/]
  --verbose     print more information
  --yes         assume affirmative answer to all questions
```

在下面的章节中，你将看到如何有效地使用 PGXN 来安装一个扩展。

## 安装扩展

通常，使扩展运行起来的工作流程涉及几个步骤。首先，你需要找出使用哪个扩展、哪个版本，以及与你的集群的兼容性。一旦你找到了需要的扩展，你必须在集群中安装它。

在集群中安装它实际上意味着将其部署到 PostgreSQL 目录中，即将所有扩展相关文件和库移动到集群的共享目录中，以便 PostgreSQL 可以查找运行扩展所需的代码。

最后，你需要在需要它的每个数据库中创建扩展。创建扩展就像在特定数据库内启用扩展的使用一样。

为了演示扩展的使用，我们将安装 orafce，即 Oracle 兼容函数扩展。描述整个扩展并不是本节的目的，所以让我们简单地说，这个扩展提供了一组函数、数据类型和其他内容，使 PostgreSQL 看起来像 Oracle 数据库，从而使基于 Oracle 的应用程序迁移变得更容易。

以下小节描述了使扩展运行所需的每个步骤。

### 通过 pgxnclient 安装扩展

通常，安装扩展的第一步是获取有关它的详细信息——这意味着搜索一个扩展。在这种特殊情况下，我们已经知道要查找的扩展是什么，但让我们通过 pgxn 搜索它：

> 注意，orafce 版本会定期更改。4.5.0 在撰写时是最新的，但你可能会看到更新的版本号。

