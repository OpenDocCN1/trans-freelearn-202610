

pjtexier@amplitude:~ $ ls -l classes/
total 24
-rw-rw-r-- 1 pjtexier pjtexier 1721 avril 22 19:20 cmake_qt5.bbclass
-rw-rw-r-- 1 pjtexier pjtexier 2001 avril 22 19:20 populate_sdk_qt5.bbclass
-rw-rw-r-- 1 pjtexier pjtexier 7633 avril 22 19:20 qmake5_base.bbclass
-rw-rw-r-- 1 pjtexier pjtexier 219 avril 22 19:20 qmake5.bbclass
-rw-rw-r-- 1 pjtexier pjtexier 1935 avril 22 19:20 qmake5_paths.bbclass
```

## conf 文件夹

conf 文件夹应提供配置（.conf）文件。此文件夹中的 layer.conf 文件包含一些重要变量，例如，关于优先级（BBFILE_PRIORITY）的变量：

```
# Define the priority for recipes (.bb files) from this layer,
# choosing carefully how this layer interacts with all of the
# other layers.
BBFILE_PRIORITY_qt5-layer = "7"
```

要验证优先级值，我们可以使用以下命令：

```
$: bitbake-layers show-layers
```

## recipes-* 目录

recipes-* 目录包含一些配方。例如，在 meta-qt5 层中，我们有 recipes-qt，其中包含一些元数据（例如 Qt5）。

## 机器（BSP）层

在本演示中，我们将以特定于我们硬件的 BSP 层为例：meta-raspberrypi BSP 层。此目录（文件夹）应包含类似于以下的文件列表：

```
pjtexier@amplitude:~ $ ls -l meta-raspberrypi/
classes
conf
custom-licenses
recipes-bsp
recipes-core
recipes-devtools
recipes-graphics
recipes-kernel
recipes-multimedia
COPYING.MIT
README
```

使用 BSP 层时，有一些区别。首先是在 conf 文件夹中。现在，此目录中有一个新文件夹，名为 machine。

此文件夹包含一些用于处理我们特定硬件（例如树莓派和树莓派 2）的配置文件。例如，有一个重要文件（rpi-base.inc）位于：

- machine
  - * raspberry.conf (树莓派的配置文件)
  - * raspberry2.conf (树莓派 2 的配置文件)
- Include
  - * rpi-base.inc

rpi-base.inc 文件如下所示：

```
include conf/machine/include/rpi-default-settings.inc
include conf/machine/include/rpi-default-versions.inc
include conf/machine/include/rpi-default-providers.inc

SOC_FAMILY = "rpi"
include conf/machine/include/soc-family.inc

IMAGE_FSTYPES ?= "tar.bz2 ext3 rpi-sdimg"

SERIAL_CONSOLE = "115200 ttyAMA0"

XSERVER = " \\
xserver-xorg \\
xf86-input-evdev \\
xf86-input-mouse \\
xf86-input-keyboard \\
xf86-video-fbdev \\
"
# Really supported starting from linux-raspberrypi 3.18.y only
KERNEL_DEVICETREE ?= " \\
    bcm2708-rpi-b.dtb \\
    bcm2708-rpi-b-plus.dtb \\
    bcm2709-rpi-2-b.dtb \\
    \\
    ds1307-rtc-overlay.dtb \\
    hifiberry-amp-overlay.dtb \\
    hifiberry-dac-overlay.dtb \\
    hifiberry-dacplus-overlay.dtb \\
    hifiberry-digi-overlay.dtb \\
    iqaudio-dac-overlay.dtb \\
    iqaudio-dacplus-overlay.dtb \\
    lirc-rpi-overlay.dtb \\
    pcf8523-rtc-overlay.dtb \\
    pps-gpio-overlay.dtb \\
    w1-gpio-overlay.dtb \\
    w1-gpio-pullup-overlay.dtb \\
    "
KERNEL_IMAGETYPE ?= "Image"
MACHINE_FEATURES = "kernel26 apm usbhost keyboard vfat ext2 screen touchscreen alsa bluetooth wifi sdio"
# Raspberry Pi has no hardware clock
MACHINE_FEATURES_BACKFILL_CONSIDERED = "rtc"
MACHINE_EXTRA_RRECOMMENDS += " kernel-modules"
# Set Raspberrypi splash image
SPLASH = "psplash-raspberrypi"
IMAGE_BOOT_FILES ?= "bcm2835-bootfiles/* ${KERNEL_IMAGETYPE};${SDIMG_KERNELIMAGE}"
```

正是在此文件中，我们设置了一些基本变量，包括以下内容：

- SERIAL_CONSOLE 指定串行控制台要连接的速度和设备。它作为控制台参数传递给内核，例如 115200 ttyAMA0。
- IMAGE_FSTYPES 指定要创建的根文件系统镜像的格式。
- KERNEL_IMAGETYPE 指定要构建的内核镜像类型（uImage 或 zImage）。

### 注意

如果要了解有关 BSP 层的更多信息，可以访问 [http://www.yoctoproject.org/docs/1.8/bsp-guide/bsp-guide.html](http://www.yoctoproject.org/docs/1.8/bsp-guide/bsp-guide.html)。

## 将外部层添加到树莓派

现在我们知道层的工作原理，可以将现有的外部层添加到我们的环境中。为此，我们将使用 meta-webserver (http://layers.openembedded.org/layerindex/branch/master/layer/meta-webserver/) 来包含著名的 Web 服务器 Monkey (http://monkey-project.com/)。

众所周知，第一步是修改我们的 conf/bblayers.conf 文件以添加层的路径，如下所示：

```
# LAYER_CONF_VERSION is increased each time
build/conf/bblayers.conf
# changes incompatibly
LCONF_VERSION = "6"
BBPATH = "${TOPDIR}"
BBFILES ?= ""
BBLAYERS ?= " \
/home/packt/RASPBERRYPI/poky/meta \
/home/packt/RASPBERRYPI/poky/meta-yocto \
/home/packt/RASPBERRYPI/poky/meta-yocto-bsp \
/home/packt/RASPBERRYPI/poky/meta-raspberrypi \
/home/packt/RASPBERRYPI/poky/meta-openembedded/meta-webserver \
"
BBLAYERS_NON_REMOVABLE ?= " \
/home/packt/RASPBERRYPI/poky/meta \
/home/packt/RASPBERRYPI/poky/meta-yocto \
"
```

现在我们已包含该层以便 BitBake 可以解析它，可以使用以下命令创建镜像：

```
$ bitbake rpi-basic-image
```

编译/生成过程结束后，我们可以启动树莓派并验证其正常运行。这非常简单，因为我们只需启动喜爱的浏览器，并在地址栏中输入以下内容：

http://ip_address_of_rpi/2001

现在，我们在树莓派上有一个嵌入式 Web 服务器。可以使用简单的 Web 服务器（通过升级固件来提高速度）创建许多项目，包括 CGI、HTML5/JavaScript 应用程序、带有 mpeg-streamer 的应用程序等等）。

## 总结

在本章中，我们了解了 Poky 发行版中层的主要原理。我们学习了层的工作原理，如何将外部层集成到我们的树莓派项目中，以及如何生成自定义镜像。

在下一章中，我们还将讨论层，但我们将创建自定义层以将其集成到树莓派的镜像中。

## 第 7 章。在树莓派上部署自定义层

在本章中，你将学习如何使用 Yocto Project 提供的不同工具生成自定义层。首先，你将了解如何生成层；然后，你将向层集成配方。此外，为完成本章，我们将生成自定义镜像。阅读本章将使你能够更好地在 Yocto Project 中组织源代码。

### 使用 yocto-layer 脚本创建 meta-packt_rpi 层

要创建我们的自定义层，可以使用两种不同的方法：

- 手动：创建目录 (meta-*) 并创建层配置文件 (conf/layer.conf)
- * 使用 Poky 环境提供的 yocto-layer 脚本

为了获得灵活性并避免操作失误，我们将使用第二种方法。要使用它，我们最初必须 source 所有变量，以便通过我们的 shell 在 yocto-layer 脚本中获得访问权限，如以下命令所示：

```
$ source oe-init-build-env rpi-build
```

现在我们的环境已设置好，可以访问 yocto-layer 脚本，因此可以开始创建层的过程。

请注意，此脚本 (yocto-layer) 默认在当前目录中创建层。这就是为什么我们必须将其放在环境的根目录下：

```
$ cd /where/you/want/to/stored/your/layer
```

现在可以使用以下命令启动脚本：

```
$ yocto-layer create <layer_name> -o <dest_dir>
```

### 注意

**meta- 字符串**会自动添加到层名称前面。

在我们的示例中，我们将把层命名为 **meta-packt_rpi**。以下是命令：

```
$ yocto-layer create packt_rpi
Please enter the layer priority you'd like to use for the layer: [default: 6]
Would you like to have an example recipe created? (y/n) [default: n] y
Please enter the name you'd like to use for your example recipe: [default: example]
example-packt
Would you like to have an example bbappend file created? (y/n) [default: n] y
Please enter the name you'd like to use for your bbappend file: [default: example]
example-packt
Please enter the version number you'd like to use for your bbappend file (this should match the recipe you're appending to): [default: 0.1]
New layer created in meta-packt_rpi.
Don't forget to add it to your BBLAYERS (for details see meta-packt_rpi/README).
```

通过此脚本，我们生成了自己的层 meta-packt_rpi，该层中包含一个示例配方、一个 bbappend 文件示例等等。我们生成的层示例如下命令所示：

```
$ tree meta-packt_rpi/
```

