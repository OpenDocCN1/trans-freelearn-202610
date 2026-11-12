

## 编辑 local.conf 文件

rpi-build/conf/local.conf 文件是一个可以配置构建过程各个方面的文件。正是通过此文件，我们可以选择目标机器（MACHINE 变量）、发行版（DISTRO 变量）、包类型（PACKAGE_CLASSES 变量）以及主机配置（例如 PARALLEL_MAKE）。我们需要从默认值中更改的最小变量集如下：

```
BB_NUMBER_THREADS ?= "${@oe.utils.cpu_count()}"
PARALLEL_MAKE ?= "-j ${@oe.utils.cpu_count()}"
MACHINE ?= "raspberrypi"
```

BB_NUMBER_THREADS 变量决定 BitBake 将并行执行的任务数量（这些是在 Yocto 下的任务；我们不一定在谈论编译）。默认情况下，在 build/conf/local.conf 中，该变量初始化为 ${@oe.utils.cpu_count()}，对应于主机系统上检测到的核心数量（/proc/cpuinfo）。

PARALLEL_MAKE 变量对应于 make 的 -j 选项，用于指定 GNU Make 在编译任务中可以并行运行的进程数量。同样，可用核心数量定义了所使用的默认值。

MACHINE 变量是我们确定要为 Raspberry Pi 构建的目标机器的地方（在 .conf 文件中定义；在我们的案例中，它是 raspberrypi.conf）。

## 编辑 bblayers.conf 文件

现在我们仍然需要将特定层添加到我们的目标中。这将使来自该层的配方可用于我们的构建。因此，我们应该编辑 build/conf/bblayers.conf 文件：

```
# LAYER_CONF_VERSION is increased each time build/conf
#bblayers.conf
# changes incompatibly
LCONF_VERSION = "6" BBPATH = "${TOPDIR}" BBFILES ?= ""
BBLAYERS ?= " \
/home/packt/RASPBERRYPI/poky/meta \
/home/packt/RASPBERRYPI/poky/meta-yocto \
/home/packt/RASPBERRYPI/poky/meta-yocto-bsp \
"BBLAYERS_NON_REMOVABLE ?= " \
/home/packt/RASPBERRYPI/poky/meta \
/home/packt/RASPBERRYPI/poky/meta-yocto \
"
```

添加以下行：

```
# LAYER_CONF_VERSION is increased each time build/conf
#bblayers.conf
# changes incompatibly
LCONF_VERSION = "6" BBPATH = "${TOPDIR}" BBFILES ?= ""
BBLAYERS ?= " \
/home/packt/RASPBERRYPI/poky/meta \
/home/packt/RASPBERRYPI/poky/meta-yocto \
/home/packt/RASPBERRYPI/poky/meta-yocto-bsp \
/home/packt/RASPBERRYPI/poky/meta-raspberrypi \
"BBLAYERS_NON_REMOVABLE ?= " \
/home/packt/RASPBERRYPI/poky/meta \
/home/packt/RASPBERRYPI/poky/meta-yocto \
"
```

当然，您需要根据自己的安装情况调整绝对路径（此处为 /home/packt/RASPBERRYPI）。

## 构建 Poky 镜像

在开发的这个阶段，让我们来看看可用且经过认证适用于我们平台的镜像（.bb 文件）。

## 镜像选择

Poky 提供了几种预先设计的镜像配方，我们可以使用它们来构建自己的二进制镜像。我们可以通过从 poky 目录运行以下命令来查看可用镜像的列表：

```
$ ls meta*/recipes*/images/*.bb
```

所有配方提供的镜像本质上是已解包和配置的包集合，生成可用于实际硬件的文件系统（有关不同镜像的更多信息，您可以访问 ( http://www.yoctoproject.org/docs/latest/mega-manual/mega-manual.html#ref-images ) ）。

接下来，这是可用镜像的简要表示：

![](img/76681dcfb8cd2bd107d0c06ab088f916_52_0.png)

我们可以将 meta-raspberrypi 提议的层添加到所有这些层中：

```
$ ls meta-raspberrypi/recipes-core/images/*.bb
rpi-basic-image.bb
rpi-hwup-image.bb
rpi-test-image.bb
```

- rpi-hwup-image.bb：这是一个基于 core-image-minimal 的镜像。
- rpi-basic-image.bb：这是一个基于 rpi-hwup-image.bb 并添加了一些功能（启动画面）的镜像。
- rpi-test-image.bb：这是一个基于 rpi-basic-image.bb 并包含 meta-raspberrypi 中的一些包的镜像。

在本章的其余部分，我们将从这三个配方中选择一个。请注意，这些文件（.bb）像所有其他文件一样描述配方。它们逻辑地组织，在这里，我们拥有用于为 Raspberry Pi 创建镜像的配方。

## 运行 BitBake

此时，我们需要启动 **BitBake**，即构建引擎，它将解析包含您作为参数传递的镜像的所有配方（在我们的第一个示例中，我们可以使用 `rpi-basic-image`）：

```
$ bitbake rpi-basic-image
```

```
Loading cache: 100%
|###############################################################################|
#################################################################################
###| ETA:  00:00:00
########################################
Loaded 1352 entries from dependency cache.
NOTE: Resolving any missing task queue dependencies
Build Configuration:
BB_VERSION = "1.25.0"
BUILD_SYS = "x86_64-linux"
NATIVELSBSTRING = "Ubuntu-14.04"
TARGET_SYS = "arm-poky-linux-gnueabi"
MACHINE = "raspberrypi"
DISTRO = "poky"
DISTRO_VERSION = "1.7"
TUNE_FEATURES = "arm armv6 vfp"
TARGET_FPU = "vfp"
meta
meta-yocto
meta-yocto-bsp =
"master:08d3f44d784e06f461b7d83ae9262566f1cf09e4"
meta-raspberrypi =
"master:6c6f44136f7e1c97bc45be118a48bd9b1fef1072"
NOTE: Preparing RunQueue
NOTE: Executing SetScene Tasks
NOTE: Executing RunQueue Tasks
```

启动后，Bitbake 首先浏览环境提供的所有文件（`.bb`, `.bbclass`）并将信息存储在缓存中。

虽然 Bitbake 的解析器是并行化的，但第一次执行总是会更长一些（只多几秒钟），因为它需要构建缓存。然而，后续执行将几乎是瞬间完成的，因为 Bitbake 加载缓存。从前面的命令可以看到，在执行任务列表之前，Bitbake 显示了详细的版本跟踪（目标、版本、操作系统等等）。最后，Bitbake 开始执行任务并显示进度。

根据您的设置，您可以去喝杯咖啡，甚至吃些披萨。通常，在此之后，如果一切顺利，您会很高兴地发现 `tmp/` 子目录的目录结构（`rpi-build`）通常已被填充。构建目录（`rpi-build`）在镜像创建后包含大约 20 GB 的数据。

经过几个小时的构建后，我们可以为结果和我们目标的系统镜像的创建而欢欣鼓舞：

```
$ ls rpi-build/tmp/deploy/images/raspberrypi/*sdimg
rpi-basic-image-raspberrypi.rpi-sdimg
```

正是这个文件将用于创建我们的可启动 SD 卡。

## 创建 SD 卡

现在您的环境已准备就绪，您可以使用以下命令创建 SD 卡（请记住将 /dev/sdX 改为正确的设备名称，并小心不要选择错误的设备名称而损坏硬盘）：

```
$ sudo dd if=rpi-basic-image-raspberrypi.rpi-sdimg of=/dev/sdX
bs=1M
```

复制完成后，您可以使用以下命令检查操作是否成功（参见 mmcblk0）：

```
$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
mmcblk0     179:0    0   3,7G  0 disk
├─mmcblk0p1 179:1    0    20M  0 part
/media/packt/raspberrypi
└─mmcblk0p2 179:2    0   108M  0 part /media/packt/f075d6df-d8b8-4e85-a2e4-36f3d40035c3c
```

您还可以查看界面左侧：

![](img/76681dcfb8cd2bd107d0c06ab088f916_56_0.png)

## 在 Raspberry Pi 上启动镜像

我们现在来到了本章无疑最令人期待的时刻：使用全新的 Poky 镜像启动我们的 Raspberry Pi 的时刻。

您只需将 SD 卡插入插槽，将 HDMI 电缆连接到显示器，并连接电源（还建议使用鼠标和键盘来关闭设备，除非您计划直接拔掉电源并可能损坏引导分区）。

连接电源后，您应该会看到 Raspberry Pi 启动画面：

![](img/76681dcfb8cd2bd107d0c06ab088f916_57_0.png)

### 注意

Yocto/Poky 发行版的登录用户是 root。

