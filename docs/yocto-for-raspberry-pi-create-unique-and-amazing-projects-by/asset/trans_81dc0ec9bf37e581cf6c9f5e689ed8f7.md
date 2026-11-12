

## Yocto Project - 工作流

以下图表从高层次展示了 Yocto Project 开发环境，以呈现交叉编译框架：

![](img/76681dcfb8cd2bd107d0c06ab088f916_38_0.png)

让我们来看看图中各组件代表的含义：

- 用户配置：这是你可以用来控制构建过程的元数据。
- 元数据层：这些是提供软件、机器和发行版元数据的各种层。
- 源文件：这些包含上游版本、本地项目以及源代码管理（Git、SVN 等）。
- 构建系统：这些是在 BitBake 控制下的进程。此模块详细说明了 BitBake 如何获取源文件、应用补丁、完成编译、分析输出以生成软件包、创建和测试软件包、生成镜像以及生成交叉开发工具。
- 软件包馈送：这些是包含输出软件包（RPM、DEB 或 IPK）的目录，随后用于构建系统生成的镜像或 SDK。这些馈送还可以使用 Web 服务器或其他方式复制和共享，以便在运行时扩展或更新设备上的现有镜像（前提是启用了运行时软件包管理）。
- 镜像：这些是开发过程生成的镜像（构成操作系统的各个部分，例如内核镜像、引导加载程序和根文件系统）。
- 应用程序开发 SDK：这些是与镜像一起生成或单独由 BitBake 生成的交叉开发工具。

## 摘要

本章介绍了 Yocto Project 的工作原理、构成其核心的组件（如 Poky、OpenEmbedded-Core 和 BitBake）以及它们在 Yocto Project 中的协作方式。

在下一章中，我们将通过不同的步骤来练习 Yocto Project 的工作流程，以下载、配置和准备 Poky 构建环境，以便为树莓派生成我们的第一个 Poky 镜像。

## 第 2 章 为树莓派构建我们的第一个 Poky 镜像

在本章中，我们将尝试理解 Poky 工作流程的基本概念。使用 Linux 命令行，我们将执行下载、配置和准备 Poky 树莓派环境以及生成可由目标设备使用的镜像所需的不同步骤。

### 为主机系统安装所需软件包

主机系统配置所需的步骤取决于所使用的 Linux 发行版。事实上，建议使用由 Poky 维护和支持的 Linux 发行版之一。这样可以避免在设置主机系统时浪费时间和精力。目前，Yocto Project 支持以下发行版：

- Ubuntu 12.04 (LTS)
- Ubuntu 13.10
- Ubuntu 14.04 (LTS)
- Fedora release 19 (Schrödinger's Cat)
- Fedora release 21
- CentOS release 6.4
- CentOS release 7.0
- Debian GNU/Linux 7.0 (Wheezy)
- Debian GNU/Linux 7.1 (Wheezy)
- Debian GNU/Linux 7.2 (Wheezy)
- Debian GNU/Linux 7.3 (Wheezy)
- Debian GNU/Linux 7.4 (Wheezy)
- Debian GNU/Linux 7.5 (Wheezy)
- Debian GNU/Linux 7.6 (Wheezy)
- openSUSE 12.2
- openSUSE 12.3
- openSUSE 13.1

### 注意

即使你的发行版未在此列出，也不意味着 Poky 无法工作，但结果不作保证。在本书中，你将看到关于在 Ubuntu 发行版上使用 Poky 的说明。如果你想了解支持的 Linux 发行版的更多信息，可以访问此链接：http://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html。

### Poky on Ubuntu

以下列表按功能显示了在受支持的 Ubuntu 或 Debian Linux 发行版上 Poky 所需的软件包：

- 下载工具：wget 和 git-core
- 解压缩工具：unzip 和 tar
- 编译工具：gcc-multilib、build-essential 和 chrpath
- 字符串操作工具：sed 和 gawk
- 文档管理工具：texinfo、xsltproc、docbook-utils、fop、dblatex 和 xmlto
- 补丁管理工具：patch 和 diffstat

总结一下，以下是在无头系统上输入的命令：

```
$ sudo apt-get install gawk wget git-core diffstat unzip
texinfo gcc-multilib build-essential chrpath
```

### Poky on Fedora

如果你想在 Fedora 上使用 Poky，只需键入以下命令行：

```
$ sudo yum install gawk make wget tar bzip2 gzip python unzip
perl patch diffutils diffstat git cpp gcc gcc-c++ glibc-devel
texinfo chrpath ccache perl-Data-Dumper perl-Text-ParseWords
perl-Thread-Queue socat
```

### 下载 Poky 元数据

安装完所有必要软件包后，现在可以下载 Poky 的源代码了。这是通过 Git 工具完成的：

```
$ git clone git://git.yoctoproject.org/poky (branch master)
```

![](img/76681dcfb8cd2bd107d0c06ab088f916_44_0.png)

### 注意

另一种方法是直接从此仓库下载 tar.bz2 文件：https://www.yoctoproject.org/downloads

为避免所有危险和有问题的操作，强烈建议创建并切换到特定的本地分支：

```
$ cd poky
$ git checkout daisy -b work_branch
```

### 下载树莓派 BSP 元数据

此时，我们只有参考系统（Poky）的基础，而没有对 Broadcom BCM SoC 的支持。基本上，Poky 提出的 BSP 仅提供以下目标：

```
$ ls meta/conf/machine/*.conf
beaglebone.conf
edgerouter.conf
genericipx86-64.conf
genericipx86.conf
mpc8315e-rdb.conf
```

此外，还有 OE-Core 提供的：

```
$ ls meta/conf/machine/*.conf
qemuarm64.conf
qemuarm.conf
qemumips64.conf
qemumips.conf
qemuppc.conf
qemux86-64.conf
qemux86.conf
```

### 提示

下载代码包的详细步骤在本书的前言中有说明。请查看。本书的代码包也托管在 GitHub 上：https://github.com/PacktPublishing/Yocto-for-Raspberry-Pi。我们还从丰富的图书和视频目录中提供其他代码包，可在 https://github.com/PacktPublishing/ 上查看。去看看吧！

为了为我们的目标生成兼容系统，我们将在这个阶段花费更多时间，并下载特定的层（BSP Layer）到树莓派：

```
$ git clone git://git.yoctoproject.org/meta-raspberrypi
```

### 提示

如果你想了解更多关于 git scm 的信息，可以访问官方网站：http://git-scm.com/

- bitbake
- documentation
- meta
- meta-oe
- meta-raspberrypi
- meta-selftest
- meta-skeleton
- meta-yocto
- meta-yocto-bsp
- rpi-build
- scripts
- LICENSE
- oe-init-build-env
- oe-init-build-env-memres
- README
- README.hardware

现在，我们可以验证我们是否拥有平台的配置 **元数据**（raspberrypi.conf 文件）：

```
$ ls meta-raspberrypi/conf/machine/*.conf
raspberrypi.conf
```

以下截图显示了 meta-raspberrypi 文件夹：

- classes
- conf
- files
- recipes-bcm
- recipes-bsp
- recipes-core
- recipes-devtools
- recipes-graphics
- recipes-kernel
- recipes-multimedia
- COPYING.MIT
- README
- rpi.patch

本章和以下章节中展示的示例和代码使用的是 Yocto Project 版本 1.7 和 Poky 版本 12.0。作为参考，代号是 Dizzy。

既然我们已经全新下载了环境，现在可以继续初始化环境，并通过各种配置文件配置我们的镜像。

### oe-init-build-env 脚本

如倒数第二张截图所示，Poky 目录包含一个名为 oe-init-build-env 的脚本。这是一个用于构建环境配置/初始化的脚本。它不是为了直接执行，而必须通过 source 加载。它的工作（其中之一）是初始化若干环境变量，并将自身置于参数中指定的构建目录中。脚本必须按以下方式运行：

```
$ source oe-init-build-env [build-directory]
```

在这里，build-directory 是设置环境的目录名称的可选参数（例如，我们可以在单个 Poky 源代码树中使用多个构建目录）。如果未指定，则默认为 build。build-directory 文件夹是我们执行构建的地方。但是，为了标准化步骤，我们将在整本书中使用以下命令来初始化环境：

```
$ source oe-init-build-env rpi-build
### Shell environment set up for builds. ###
You can now run 'bitbake <target>'
Common targets are:
core-image-minimal
core-image-sato
meta-toolchain
adt-installer
meta-ide-support
You can also run generated qemu images with a command like
'runqemu qemux86'
```

当我们初始化构建环境时，它会在 rpi-build 内创建一个目录（conf 目录）。该文件夹包含两个重要文件：

- local.conf：这包含用于配置 Bitbake 行为的参数。
- bblayers.conf：这列出了 Bitbake 在其实现中考虑的不同层。
- 列表分配给 BBLayers 变量。

