

## 使用 Yocto Project 为 Raspberry Pi 配置 LCD 显示器

Raspberry Pi 有多种不同的 LCD 显示屏可供选择。其中最流行的包括官方的 Raspberry Pi 7 触摸屏和 PiTFT 显示屏（通常称为 PiTFT HAT，因为它们适合安装在 Raspberry Pi 上方）。目前，我们将重点介绍 Raspberry Pi 7 触摸屏和 PiTFT 型号。

在本节中，我们将介绍如何启动标准的 Yocto Project 镜像，该镜像包含窗口系统和可用的触摸屏。

### Raspberry Pi 7 触摸屏

使用基于 Yocto Project 的镜像入门 Raspberry Pi 7 触摸屏需要进行多项配置。首先，我们需要确保使用的是 4.1+ 内核。从 meta-raspberrypi 的 **jethro** 版本开始，4.1 是默认版本。目前，meta-raspberrypi 对 Raspberry Pi 7 触摸屏的支持尚不完整，因此在启动镜像之前，我们需要进行一些小的修改。

### 注意

Raspberry Pi 7 触摸屏的构建说明可在 https://www.adafruit.com/images/product-files/2718/2718build.jpg 上找到。该显示屏适用于 Raspberry Pi A/B 板；但是，显示屏背面的安装孔仅与 A+、B+ 以及 Raspberry Pi 2/3 对齐。

```
$ source /path/to/poky/oe-init-build-env my_project
```

如果使用 meta-raspberrypi 的默认设置，我们将拥有一个受支持的内核，其中所有必需的配置选项默认处于启用状态。为保险起见，您可以浏览本章的前面部分，以确保内核已正确配置以适用于您的显示屏。考虑到这一点，我们可以从创建新项目开始：

```
$ cat local/bblayers.conf
# LAYER_CONF_VERSION is increased each time build/conf/bblayers.conf
# changes incompatibly
LCONF_VERSION = "6"
BBPATH = "${TOPDIR}"
BBFILES ?= ""
BBLAYERS ?= " \
/path/to/poky/meta \
/path/to/poky/meta-yocto \
/path/to/poky/meta-yocto-bsp \
/path/to/meta-raspberrypi \
/path/to/meta-openembedded/meta-oe \
/path/to/meta-openembedded/meta-multimedia \
"
BBLAYERS_NON_REMOVABLE ?= " \
/path/to/poky/meta \
/path/to/poky/meta-yocto \
"
```

### 注意

如果您尚未为本书中的早期示例克隆 OpenEmbedded 层，可以使用以下命令完成：$ git clone git://git.openembedded.org/meta-openembedded

我们还需要对 local.conf 进行一些修改：

```
$ echo "MACHINE = \"raspberrypi\"" >> conf/local.conf (Or MACHINE=raspberrypi2)
$ echo "LICENSE_FLAGS_WHITELIST = \"commercial\"" >> conf/local.conf
```

完成此操作后，我们即可开始新构建。默认情况下，meta-raspberrypi 将确保例如为触摸屏设置输入设备以及相应的 udev 规则。这些设置将在构建期间无缝应用：

```
$ bitbake core-image-sato
```

将新镜像写入 SD 卡后，我们还需要确保已加载触摸屏所需的所有驱动程序。这可以通过将此行添加到 SD 卡启动分区中 config.txt 文件的末尾来完成：

```
$ echo "dtoverlay=rpi-ft5406" >> /path/to/sdcard/boot/config.txt
```

现在，您已经准备就绪，可以使用 Raspberry Pi 7 触摸屏启动带有触摸支持的窗口系统了。

![](img/76681dcfb8cd2bd107d0c06ab088f916_196_0.png)

*运行 core-image-sato 的 Raspberry Pi 7 触摸屏*

### PiTFT 2.8 电阻式触摸屏

meta-raspberrypi 中对 PiTFT 显示屏的原生支持是较新的功能。请查看 meta-raspberrypi 的 readme 文件中的“启用 PiTFT 支持”部分，以确认您的 PiTFT 型号是否受支持。

使用 Yocto Project，可以为任何现有（或自定义）Raspberry Pi 镜像配置 PiTFT 支持。该支持已作为 `MACHINE_FEATURES` 添加到 meta-raspberrypi 中。`MACHINE_FEATURES` 变量是 Yocto Project 的标准功能，可用于为您要构建的目标添加硬件功能。Yocto Project 中存在一组预定义的 `MACHINE_FEATURES` 功能，其中包括 Wi-Fi、键盘、Alsa 和 Bluetooth 等。在 Raspberry Pi BSP 层中，其中一些功能默认处于启用状态：

```
$ grep "MACHINE_FEATURES " conf/machine/include/rpi-base.inc
MACHINE_FEATURES += "kernel26 apm usbhost keyboard vfat ext2 screen touchscreen
alsa bluetooth wifi sdio"
```

PiTFT 支持需要两个新的 `MACHINE_FEATURES` 功能：一个名为 `pitft` 的通用功能和一个特定于型号的版本。在本示例中，我们将使用 `pitft28r`，它对应于 PiTFT 2.8 电阻式触摸屏。添加新 `MACHINE_FEATURES` 功能的最简单方法是将其添加到项目中的本地配置文件：

```
$ cd /path/to/my/rpi/project
$ echo "MACHINE_FEATURES += "pitft pitft28r"" >> conf/local.conf
```

这将添加对运行控制台镜像或窗口系统镜像的支持。pitft 功能将在内部确保启用 SPI 和 i2c。它还将配置帧缓冲区以处理 PiTFT 显示屏（从 /dev/fb0 更改为 /dev/fb1）。特定于型号的计算机功能（本示例中的 pitft28r）会将特定于型号的设备树叠加层添加到 config.txt，为屏幕设置适当的配置（例如方向和速度），并确保加载触摸功能所需的 stmpe 模块。它还会为触摸屏添加一些默认校准数据。在设置好 `MACHINE_FEATURES` 功能后，我们可以构建镜像，将其加载到 SD 卡，然后启动：

```
$ bitbake core-image-sato
```

![](img/76681dcfb8cd2bd107d0c06ab088f916_197_0.png)

*运行 core-image-sato 的 PiTFT 2.8 电阻式触摸屏*

启动 Raspberry Pi 时，LCD 最初会变为白色。过一段时间，它应该会再次变为黑色，这意味着内核已识别屏幕。不久之后，您应该可以使用窗口系统了。“2.8”屏幕相当小，但可以使用虚拟键盘在终端中输入例如简单命令。触摸屏已为您使用默认值进行了校准，这些值适合您的屏幕和旋转方向。如果您认为默认校准不足，例如，如果您想更改显示方向（默认 90 度），可以使用例如 ts_calibrate 重新校准它。

### 注意

```
ts_calibrate is part of the tslib recipe in Poky. It's included by default when building core-image-sato, and it can easily be added to a custom image by specifying MACHINE_FEATURES += "touchscreen"
```

### 在 LCD 显示器上开发应用程序并使用它们

此时，我们已在 Raspberry Pi 上运行基于 Yocto Project 且支持 PiTFT 显示屏的镜像。在本节中，我们将开发一个可用于我们花哨触摸屏的图形应用程序。在开发图形应用程序时，存在几种不同的框架。Qt 是嵌入式设备最著名的图形框架之一，并提供免费软件版本。在本节中，我们将介绍如何使用 Qt 和 EGLFS 为嵌入式 Linux 环境设置和开发图形应用程序。Qt 是一种跨平台应用程序框架，因此除了可以在各种类型的硬件上运行外，还可以在许多不同的软件平台（例如 X11、OS X、Windows 和 EGLFS）上使用。我们在示例中将使用的 EGLFS，基本上是一种平台插件，用于在 EGL 和 OpenGL ES 之上运行 Qt 应用程序。事实上，它甚至不需要窗口系统（例如 X11）即可工作，这使其非常适合嵌入式 Linux。

### 使用 Qt 开发自定义应用程序

使用 Yocto Project 开发基于 Qt 的应用程序时，我们需要在自己的层中设置 Qt。开发图形应用程序有两种标准方法：针对导出的 SDK 构建应用程序，或为应用程序创建新的配方。在本示例中，我们将针对导出的 SDK 开发我们的应用程序。为了成功让新应用程序在目标上构建和运行，我们需要执行以下步骤。

- 添加 Qt 所需的外部层
- 导出包含 Qt 支持的 SDK
- 创建 Qt 应用程序并针对导出的 SDK 构建它
- 为目标设置带有 Qt 支持的镜像
- 在官方 Raspberry Pi 7" 触摸屏上运行 Qt 应用程序

存在多种不同的 Qt 版本，其中 Qt5 是最新版本。在开发我们的新应用程序时，我们将使用 meta-qt5 外部层。首先，我们需要下载 meta-qt5 并在项目中启用其使用：

```
$ git clone https://github.com/meta-qt5/meta-qt5.git
```

将 meta-qt5 添加到 local/bblayers.conf：

```
$ cat local/bblayers.conf
# LAYER_CONF_VERSION is increased each time build/conf/bblayers.conf
# changes incompatibly
LCONF_VERSION = "6"

BBPATH = "${TOPDIR}"
BBFILES ?= ""

BBLAYERS ?= " \
/path/to/poky/meta \
/path/to/poky/meta-yocto \
/path/to/poky/meta-yocto-bsp \
/path/to/meta-raspberrypi \
/path/to/meta-openembedded/meta-multimedia \
/path/to/meta-qt5 \
"
BBLAYERS_NON_REMOVABLE ?= " \
/path/to/poky/meta \
/path/to/poky/meta-yocto \
"
```

接下来，我们需要导出 SDK 并将其解压到首选位置：

```
$ bitbake meta-toolchain-qt5
$ .tmp/deploy/sdk/poky-glibc-x86_64-meta-toolchain-qt5-cortexa7hf-vfp-vfpv4-neon-toolchain-2.0.1.sh
```

路径可能会有所不同，具体取决于您使用的 Raspberry Pi 型号和 Yocto Project 版本。

### 注意

