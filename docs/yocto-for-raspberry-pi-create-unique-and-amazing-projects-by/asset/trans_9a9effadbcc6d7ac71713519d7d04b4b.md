

## 探索 Toaster

Toaster 是 BitBake 的 Web 界面。Yocto 项目网站对此的描述如下：

> "Toaster 是 OpenEmbedded 构建系统的应用程序编程接口 (API) 和基于 Web 的界面，该系统使用 BitBake。目前，Toaster 收集并展示有关您构建的信息，您可以使用 Web 浏览器进行导航和查询"

## 在主机系统上安装所需的软件包

为了在首选的 Linux 发行版上正确使用 Toaster，您需要安装 Django 和 South：

```
$ sudo pip install django==1.6
$ sudo pip install South==0.8.4
```

### 注意

pip 命令使安装 Python 模块变得容易，就像在 Linux 发行版中安装软件包一样。

## 运行 Toaster

既然我们的系统已准备好使用 Toaster，我们只需使用以下命令来启动服务：

```
$ source toaster start
```

这将产生以下输出：

```
The system will start. Syncing... Creating tables ... Installing custom SQL ... Installing indexes ... Installed 0 object(s) from 0 fixture(s) Synced: > django.contrib.staticfiles > django.contrib.humanize > south Not synced (use migrations): - orm - bldcontrol (use ./manage.py migrate to migrate these) Running migrations for orm: - Nothing to migrate. - Loading initial data for orm. Installed 0 object(s) from 0 fixture(s) Starting webserver Webserver address: http://0.0.0.0:8000/ Bitbake server address: 0.0.0.0, server port: 54693 Successful start.
```

我们现在可以通过 http://0.0.0.0:8000/ 访问我们的 Web 界面。

## 运行 BitBake

目前，Toaster 不支持通过界面进行配置（未来版本的 Toaster 将添加构建运行和构建自定义功能）。Toaster 收集并展示有关您构建的信息，您可以使用 Web 浏览器进行导航和查询。因此，我们必须手动启动 BitBake 来监控构建进度。如 [第 2 章，为树莓派构建我们的第一个 Poky 镜像] 中所示，我们可以使用以下命令启动构建过程：

```
$ bitbake rpi-basic-image
```

构建过程将按以下方式进行：

```
Loading cache: 100%
|###############################################################################
###############################################################################
###############################################################################
| ETA:  00:00:00 Loaded 1352 entries from dependency cache. NOTE: Resolving any missing task queue dependencies Build Configuration: BB_VERSION        = "1.25.0"
BUILD_SYS           = "x86_64-linux" NATIVELSBSTRING         = "Ubuntu-14.04" TARGET_SYS = "arm-poky-linux-gnueabi" MACHINE                       = "raspberrypi" DISTRO        = "poky" DISTRO_VERSION      = "1.7" TUNE_FEATURES         = "arm armv6 vfp" TARGET_FPU        = "vfp" meta                = meta-yocto         meta-yocto-bsp        = "master:08d3f44d784e06f461b7d83ae9262566f1cf09e4" meta-raspberrypi     = "master:6c6f44136f7e1c97bc45be118a48bd9b1fef1072" NOTE: Preparing RunQueue NOTE: Executing SetScene Tasks NOTE: Executing RunQueue Tasks
```

## 运行 Web 界面

既然我们已启动 BitBake，可以监控该过程（构建过程）的进度，如以下屏幕截图所示。请注意，打开 Toaster 界面的最佳方法是导航至该界面（使用超链接）。

![](img/76681dcfb8cd2bd107d0c06ab088f916_76_0.png)

Toaster 仍处于开发阶段；它很可能成为 Hob 的一个可靠替代品。的确，开发团队希望通过 Web 界面使 BitBake 完全可配置。

## 摘要

在本章中，我们了解了如何使用 Hob 和 Toaster 等用户友好的界面。我们了解了这些工具的不同功能（配置和功能）。值得注意的是，这些工具为开发团队提供了更好的灵活性。

在下一章中，我们将了解 BitBake 在 Yocto 项目中发挥的主要作用。我们将发现使我们能够为镜像生成软件包的不同任务。

## 第 4 章。了解 BitBake

在本章中，我们将首先探索 Poky 使用的元数据（基本概念）和配方（它们之间的依赖关系）。然后，我们将了解 BitBake 下载源代码的不同方式。我们将通过介绍 BitBake 用于创建根文件系统镜像的任务来结束本章。

## BitBake

如前所述，在第 1 章 *了解 Yocto 项目* 中，**BitBake** 是一个任务调度程序（类似于 GNU Make），用于解析 shell 和 Python 脚本。解析的代码生成并运行任务（配置、编译等），这些任务本质上是根据代码依赖关系排序的步骤集合。

以下是直接摘自 BitBake 用户手册的要点：

- BitBake 根据提供的元数据执行任务，这些元数据构建了任务。元数据存储在配方（.bb）、配置（.conf）和类（.bbclass）文件中，并为 BitBake 提供关于要运行哪些任务以及这些任务之间依赖关系的说明。
- BitBake 包含一个获取库，用于从各种地方获取源代码，例如源代码控制系统或网站。
- 要构建的每个单元（例如一段软件）的说明称为配方文件，并包含有关该单元的所有信息（依赖关系、源文件位置、校验和、描述等）。
- BitBake 包含一个客户端/服务器抽象，可以从命令行使用或作为 XMLRPC 服务使用，并且具有多种不同的用户界面。

### 元数据

BitBake 使用的元数据可以有几种不同的形式，如下所示：

- 配置（.conf）文件
- 配方（.bb 和 .bbappend 文件）
- 类（.bbclass 文件）
- 包含（.inc）文件

### 配置

以 .conf 扩展名表示的配置文件定义了管理项目构建过程的各种配置变量。这些文件分为几个区域，分别定义机器配置选项、发行版配置选项、编译器调优选项、常规通用配置选项和用户配置选项。

### 类

以 .bbclass 扩展名表示的类文件包含在元数据文件之间共享很有用的信息。BitBake 源代码树目前附带一个名为 base.bbclass 的类元数据文件。您可以在 classes 目录中找到此文件。base.bbclass 文件很特殊，因为它始终自动包含于所有配方和类中。此类包含标准基本任务的定义，例如获取、解包、配置（默认为空）、编译（运行任何现有 makefile）、安装（默认为空）和打包（默认为空）。这些任务在项目开发过程中通常由其他类重写或扩展。

### 配方

BitBake 配方（以 .bb 文件扩展名表示，例如 bcm2835_1.38.bb）是最基本的元数据文件。这些配方文件为 BitBake 提供以下信息：

- 关于软件包的描述性信息
- 配方的版本
- 现有依赖项
- 源代码所在位置
- 源代码是否需要任何补丁
- 如何编译源代码
- 在目标机器上安装正在编译的软件包的位置

## 解析元数据

BitBake 做的第一件事是解析基础配置元数据（.conf 文件）。基础配置元数据包括 `bblayers.conf` 文件（用于确定 BitBake 需要识别哪些层）、所有必要的 `layer.conf` 文件（每个层一个）以及 `bitbake.conf`。数据本身具有多种类型：

- 配方：包含有关特定软件片段的详细信息。
- 类数据：提供常见构建信息的抽象（例如，如何构建 Linux 内核）。
- 配置数据：提供特定于机器的设置、策略决策等。配置数据充当将一切联系在一起的粘合剂。

`layer.conf` 文件用于构建 BBPATH 和 BBFILES 等关键变量。BBPATH 用于分别在 `conf/` 和 `class/` 目录下搜索配置文件和类文件。BBFILES 用于查找配方文件（.bb 和 .bbappend）。如果没有 `bblayers.conf` 文件，则假设用户已直接在环境中设置 BBPATH 和 BBFILES 变量。

接下来，将使用刚刚构建的 BBPATH 变量搜索 `bitbake.conf` 文件。`bitbake.conf` 文件还可以使用 `include` 或 `require` 指令包含其他配置文件。

