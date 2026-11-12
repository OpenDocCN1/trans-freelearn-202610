

## Preferences and providers

## 首选项和提供者

Once BitBake has realized the "parsing" step (analyzing all the recipes), it must know how to build the target. It starts by looking through the PROVIDES variable set in the recipe files. The default PROVIDES value for a recipe is its name (PN).

一旦 BitBake 完成“解析”步骤（分析所有配方），就必须知道如何构建目标。它首先会查看配方文件中设置的 PROVIDES 变量。配方的默认 PROVIDES 值为其名称（PN）。

### Note

### 注意

PN represents the name of the recipe; PR, the revision of the recipe; and PV, the version of the recipe. For example, when using the recipe rpi-gpio_0.5.9.bb, here is what the values will be:

PN 表示配方名称；PR 表示配方的修订版本；PV 表示配方的版本。例如，使用配方 rpi-gpio_0.5.9.bb 时，值如下：

```
${PN} = rpi-gpio
${PV} = 0.5.9
```

Sometimes, a target might have multiple providers. A common example is virtual/kernel, which is provided by each kernel recipe (check out meta-raspberrypi/tree/master/recipes-kernel/linux for further information). Each machine often selects the best kernel provider by using a line similar to the following in the machine configuration file. If we look into this following file (meta-raspberrypi/conf/machine/include/rpi-default-providers.inc), we can see some variables:

有时，目标可能有多个提供者。常见示例是 virtual/kernel，由每个内核配方 提供（有关更多信息，请查看 meta-raspberrypi/tree/master/recipes-kernel/linux）。每台机器通常在机器配置文件中使用类似于以下的一行来选择最佳内核提供者。如果我们查看以下文件（meta-raspberrypi/conf/machine/include/rpi-default-providers.inc），可以看到一些变量：

```
# RaspberryPi BSP default providers
PREFERRED_PROVIDER_virtual/kernel = "linux-raspberrypi"
PREFERRED_PROVIDER_u-boot = "u-boot-rpi"
```

In this case, we have selected the required U-Boot version and kernel for our Raspberry Pi.

在此示例中，我们为 Raspberry Pi 选择了所需的 U-Boot 版本和内核。

## Dependencies

## 依赖项

In order to satisfy dependencies, the recipes must declare what they need to have available during the build process.

为满足依赖项，配方必须声明在构建过程中需要可用的内容。

BitBake uses a special mechanism that allows us to list the build-time dependencies and then checks whether all of the rules are satisfied before the build step. For example, if you work with canutils (http://pengutronix.de/software/socket-can/download/canutils/), you have to set this following variable:

BitBake 使用一种特殊机制，允许我们列出构建时依赖项，然后在构建步骤前检查所有规则是否满足。例如，如果您使用 canutils（http://pengutronix.de/software/socket-can/download/canutils/），则必须设置以下变量：

```
DEPENDS = "libsocketcan"
```

In this example, **CANUTILS** needs libsocketcan; therefore, BitBake will start by building the libsocketcan package (and installing the headers into rootfs) before building canutils and linking.

在此示例中，**CANUTILS** 需要 libsocketcan；因此，BitBake 将在构建 canutils 并进行链接之前，先构建 libsocketcan 包（并将头文件安装到 rootfs）。

When an application depends on something to run, it is called a runtime dependency (these are packages necessary on the target in order to guarantee proper functioning). In this case, we don't need to set the DEPENDS variable but the RDEPENDS variable in a recipe in order to inform BitBake.

当应用程序运行时依赖于某物时，这称为运行时依赖项（这些是目标上保证正常运行所必需的包）。在这种情况下，我们不需要在配方中设置 DEPENDS 变量，而是需要设置 RDEPENDS 变量，以告知 BitBake。

## Fetching

## 获取

The mechanism used by BitBake to fetch source code is internally called the **fetcher backend**. There are several fetcher backends supported, which can be configured to align user requirements and optimize source code fetching.

BitBake 用于获取源代码的机制在内部称为 **fetcher 后端**。支持多个 fetcher 后端，可配置以满足用户要求并优化源代码获取。

![](img/76681dcfb8cd2bd107d0c06ab088f916_87_0.png)

BitBake supports several protocols for remote file downloads. The most commonly used are http://, https://, and git://. When BitBake executes the do_fetch task in a recipe, it checks the contents of SRC_URI. We will discover, through the various fetchers, how to proceed based on our need.

BitBake 支持多种用于远程文件下载的协议。最常用的是 http://、https:// 和 git://。当 BitBake 在配方中执行 do_fetch 任务时，它会检查 SRC_URI 的内容。我们将通过各种 fetcher 了解如何根据需求进行操作。

### The local file fetcher

### 本地文件 fetcher

The local file fetcher submodule handles URLs that begin with `file://`. The filename you specify within the URL can either be an absolute or relative path to a file. For example, with a file called `my_source_file.c`, we must write the `SRC_URI` attribute's content like this:

本地文件 fetcher 子模块处理以 `file://` 开头的 URL。URL 中指定的文件名可以是文件的绝对路径或相对路径。例如，对于名为 `my_source_file.c` 的文件，必须将 `SRC_URI` 属性的内容写成如下形式：

```
SRC_URI = "file://my_source_file.c"
```

### The HTTP fetcher

### HTTP fetcher

The HTTP fetcher obtains files from web servers. Internally, the fetcher uses the wget utility.

HTTP fetcher 从 Web 服务器获取文件。在内部，fetcher 使用 wget 实用程序。

In this example, we will use the bcm2835 library. With this submodule, we will retrieve the tar.gz file of this library:

在此示例中，我们将使用 bcm2835 库。使用此子模块，我们将检索该库的 tar.gz 文件：

```
SRC_URI = "http://www.open.com.au/mikem/bcm2835/bcm2835-${PV}.tar.gz"
```

### The Git fetcher

### Git fetcher

One of the most commonly used source control management systems in use is Git. BitBake has solid support for it, and the Git backend is used when the do_fetch task is run and finds a git:// URL at the SRC_URI variable. Here is an example featuring a utility developed by Christophe Blaess. It is a simple command-line tool to help with the use of Linux spidev devices.

Git 是当前最常用的源代码管理系统之一。BitBake 对其提供强大支持，当运行 do_fetch 任务并在 SRC_URI 变量中找到 git:// URL 时，将使用 Git 后端。以下示例展示了 Christophe Blaess 开发的一个实用程序。它是一个简单的命令行工具，可帮助使用 Linux spidev 设备。

```
SRCREV = "cc6a41fdcec60610703ba6db488c621c64952898"
```

This variable contains the reference to the commit that the version control system will use as a basis. In the case of Git, it is the commit hash.

该变量包含版本控制系统将用作基础的提交引用。对于 Git，它是提交哈希。

```
SRC_URI = "git://github.com/cpb-/spi-tools.git;protocol=git"
```

### Note

### 注意

When the SRCREV variable points to a hash not available in the master branch, we need to use the branch=<branch name> parameter, as follows: SRC_URI = "git://myserver/myrepo.git;branch=mybranch". In the cases when the hash used points to a tag that is not available in a branch, we need to use the nobranch=1 option, as follows: SRC_URI = "git://myserver/myrepo.git;nobranch=1".

当 SRCREV 变量指向 master 分支上不可用的哈希时，需要使用 branch=<branch name> 参数，如下所示：SRC_URI = "git://myserver/myrepo.git;branch=mybranch"。当所使用的哈希指向分支上不可用的标记时，需要使用 nobranch=1 选项，如下所示：SRC_URI = "git://myserver/myrepo.git;nobranch=1"。

For further information about the SRC_URI parameter's values, go to http://www.yoctoproject.org/docs/1.6/bitbake-user-manual/bitbake-user-manual.html#var-SRC_URI for some examples.

有关 SRC_URI 参数值的更多信息，请访问 http://www.yoctoproject.org/docs/1.6/bitbake-user-manual/bitbake-user-manual.html#var-SRC_URI 查看示例。

## Understanding BitBake's tasks

## 了解 BitBake 任务

The bitbake command is the primary interface to the BitBake tool. This chapter presents the bitbake command syntax and provides several execution examples.

bitbake 命令是 BitBake 工具的主要接口。本章介绍了 bitbake 命令语法，并提供了几个执行示例。

For example, if you want to build a specific recipe, run the following command:

例如，如果要构建特定的配方，请运行以下命令：

```
$ bitbake <recipe>
```

BitBake runs a set of scheduled tasks. When we wish to run a specific task, we can use the following command:

BitBake 运行一组计划任务。当我们希望运行特定任务时，可以使用以下命令：

```
$ bitbake <recipe> -c <task>
```

If you want to clean a specific package (spi-tools, for example), you just have to run this:

如果要清理特定包（例如 spi-tools），只需运行：

```
$ bitbake spi-tools -c clean
```

To list the tasks defined for a recipe, we can use the following command:

要列出配方定义的任务，可以使用以下命令：

```
$ bitbake <recipe> -c listtasks
```

Here is a description of BitBake tasks:

以下是 BitBake 任务的描述：

- **do_fetch**: The first step when building a recipe is fetching the required source. This is done using the fetching backend feature we discussed previously. It is important to point out that fetching source or a file does not mean it is a remote source. In fact, every file required during the recipe build must be fetched so that it is made available in the WORKDIR directory.

- **do_fetch**: 构建配方的第一步是获取所需的源代码。这是使用我们之前讨论过的获取后端功能完成的。需要指出的是，获取源代码或文件并不意味着它是远程源。事实上，配方构建期间所需的每个文件都必须被获取，以便在 WORKDIR 目录中可用。

    All downloaded content is stored in the download folder (the DL_DIR variable), so all external source code is cached to avoid redownloading it every time we need the same source.

    所有下载的内容都存储在下载文件夹（DL_DIR 变量）中，因此会缓存所有外部源代码，以避免每次需要相同源代码时重新下载。

- **do_unpack**: The natural subsequent task after the do_fetch task is do_unpack. It is responsible for unpacking source code or checking the requested revision or branch in case the referenced source uses an SCM system.

- **do_unpack**: do_fetch 任务后的自然后续任务是 do_unpack。它负责解包源代码，或者在引用的源代码使用 SCM 系统时检查请求的修订版本或分支。

- **do_patch**: Once the source code has been properly unpacked, BitBake initiates the process of adapting it. This is done by the do_patch task. Every file fetched by do_fetch that has the .patch extension is assumed to be a patch to be applied. This task applies the list of required patches.

- **do_patch**: 正确解包源代码后，BitBake 会启动适配过程。这是由 do_patch 任务完成的。do_fetch 获取的每个具有 .patch 扩展名的文件都被假定为要应用的补丁。此任务应用所需补丁列表。

    The process of applying a patch uses the S variable, which points to the source code. The default value used for S is ${WORKDIR}/${PN}-${PV}, and it is used for the do_patch, do_configure, do_compile, and do_install tasks.

    应用补丁的过程使用 S 变量，该变量指向源代码。S 的默认值为 ${WORKDIR}/${PN}-${PV}，并用于 do_patch、do_configure、do_compile 和 do_install 任务。

- **do_configure, do_compile, and do_install**: The do_configure, do_compile, and do_install tasks are performed in this order. Some recipes may omit one task or another. It is important to note that the environment variables defined in the tasks are different from one task to another.

- **do_configure、do_compile 和 do_install**: 按此顺序执行 do_configure、do_compile 和 do_install 任务。某些配方可能会省略某个任务。需要注意的是，任务中定义的环境变量在不同任务之间有所不同。

Tasks vary a lot from one recipe to another. Poky provides a rich collection of predefined tasks in the classes, which ought to be used when possible. For example, when the Autotools class is inherited by a recipe, it provides a known implementation for the do_configure, do_compile, and do_install tasks.

不同配方之间的任务差异很大。Poky 在类中提供了丰富的预定义任务集合，应尽可能使用。例如，当配方继承 Autotools 类时，它为 do_configure、do_compile 和 do_install 任务提供已知实现。

- do_package: The do_package task splits the files installed by the recipe into logical components, such as debugging symbols, documentation, and libraries. The do_package task ensures that files are split up and packaged correctly.

- do_package: do_package 任务将配方安装的文件拆分为逻辑组件，例如调试符号、文档和库。do_package 任务确保文件被正确拆分和打包。

One of the most common uses of Poky is the generation of the root filesystem. The rootfs image should be seen as a ready-to-use root filesystem for a target. The rootfs is basically a directory with the desired packages installed. The list of packages to be installed into rootfs is defined by a union of packages listed by IMAGE_INSTALL and the packages included by IMAGE_FEATURES.

Poky 最常见的用途之一是生成根文件系统。rootfs 映像应被视为目标的可直接使用的根文件系统。rootfs 基本上是一个安装了所需包的目录。要安装到 rootfs 的包列表由 IMAGE_INSTALL 列出的包与 IMAGE_FEATURES 包含的包的并集定义。

### Note

### 注意

After do_rootfs has finished, the generated image file is placed in <build-dir>/tmp/deploy/image/raspberrypi/.

do_rootfs 完成后，生成的映像文件将放置在 <build-dir>/tmp/deploy/image/raspberrypi/ 中。

We will learn more about the process and its contents in Chapter 5, Creating, Developing, and Deploying on the Raspberry Pi.

我们将在第 5 章“在 Raspberry Pi 上创建、开发和部署”中详细了解此过程及其内容。

