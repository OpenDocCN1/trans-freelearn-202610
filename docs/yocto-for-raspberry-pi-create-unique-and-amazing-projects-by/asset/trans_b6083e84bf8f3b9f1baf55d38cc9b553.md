

cp -a ${S}/server ${D}${datadir}/server-packet
}

# 包文件
FILES_${PN} += "${datadir}/server-packet"

## 说明

让我们来看看这些变量的作用：

- RDEPENDS_{PN} 变量列出了构建包正常运行所需的运行时依赖项，这些依赖项必须安装。在我们的例子中，bash 对 nodejs 是必需的。
- FILES_{PN} 变量包含放置到包中的目录或文件列表。在我们的例子中，server-packet 是必需的。

我们现在已准备好可用的环境。如果我们要将应用程序部署到 Raspberry Pi 上，可以使用以下命令。

主机命令如下：

```
$ bitbake webserver-packet
$ scp tmp/deploy/rpm/arm1176jzfshf_vfp/webserver-packet-0.1-r0.arm1176jzfshf_vfp.rpm root@ip_address_of_rpi:/home/
```

以下是 Raspberry Pi 命令：

```
$ rpm -ivh webserver-packet-0.1-r0.arm1176jzfshf_vfp.rpm
$ rm webserver-packet-0.1-r0.arm1176jzfshf_vfp.rpm
$ node /usr/share/server-packet/server/server.js
```

## 自动启动 - init 文件

如果我们想要一个独立的应用程序，那么在 Raspberry Pi 启动序列期间自动启动我们的应用程序将很有趣。为此，我们将使用直接继承自 System V 的 init 脚本。

第一步是在我们的结构中创建一个 .init 文件。我们将此文件命名为 server.init。目录结构如下：

```
$ tree webserver-packt/
webserver-packt/
├── webserver-packt
│   ├── server.init
│   └── server.tar.gz
└── webserver-packt_0.1.bb
```

因此，该脚本将在安装后以及每次 Raspberry Pi 启动时自动启动应用程序。以下是来自 System V 的 init 脚本：

```bash
#!/bin/bash
### BEGIN INIT INFO
# Provides:          server.init
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start daemon at boot time
# Description:       Enable service provided by daemon.
### END INIT INFO
DAEMON=node
NAME=server.init
DESC="Nodejs app"
ARGS="/usr/share/server-packt/server/server.js"
set -e
usage()
{
    echo "---------------------------------------"
    echo "Usage: $0 (stop|start|restart)"
    echo "---------------------------------------"
}
service_start()
{
    echo -n "starting $DESC: $NAME... "
    start-stop-daemon -S -x $DAEMON -- $ARGS &
    echo "done."
}
echo ""

service_stop()
{
    echo -n "stopping $DESC: $NAME... "
    start-stop-daemon -K -x $DAEMON
    echo "done."
}

case $1 in
    stop)
        service_stop
        echo ""
        ;;
    start)
        service_start
        echo ""
        ;;
    restart)
        service_stop
        service_start
        echo ""
        ;;
    *)
        usage
        ;;
esac
exit 0
```

## 说明

让我们来看看代码的工作原理：

- DAEMON 变量指定要启动的二进制文件 (/usr/bin/node)
- NAME 变量指定脚本名称
- DESC 变量包含关于 DAEMON (node) 的一些信息
- ARGS 变量包含要传递给 DAEMON (node) 的参数

如果要在 Raspberry Pi 上测试此脚本，请使用以下命令：

```
$ scp server.init root@ip_address_of_rpi:/etc/init.d
$ cd /etc/init.d
$ update-rc.d server.init defaults
```

此命令停止应用程序：

```
/etc/init.d/server.init stop
```

此命令重启应用程序：

```
/etc/init.d/server.init restart
```

## 自动启动 - 配方文件

将我们的 init 文件添加到 webserver-packt.bb 配方中包括：

- 将 server.init 文件添加到 SRC_URI。
- 为我们的 init 脚本添加 do_install() 步骤。
- 包含 update-rc.d。在 Yocto 中添加此功能，需要在配方中添加 inherit update-rc.d。

以下是更新后的 webserver-packt.bb：

```
DESCRIPTION = "Panel to monitor rpi temperature"
LICENSE = "GPLv2"
LIC_FILES_CHKSUM =
"file://${COREBASE}/meta/COPYING.GPLv2;md5=751419260aa954499f7abaabaa882bbe"

# Package Release
PR = "r0"

# Use local tarball
SRC_URI = " \\
 file://server.tar.gz \\
 file://server.init \\
 "

RDEPENDS_${PN} += "bash"

# Make sure our source directory (for the build) matches the directory structure in
the tarball
S = "${WORKDIR}"

do_install() {
   install -d ${D}${datadir}/server-packt
   cp -a ${S}/server ${D}${datadir}/server-packt

   install -d ${D}${sysconfdir}/init.d/
   install -m 0755 ${WORKDIR}/server.init ${D}${sysconfdir}/init.d/server-packt-init
}

# Package files
FILES_${PN} += "${datadir}/server-packt"

inherit update-rc.d

INITSCRIPT_NAME = "server-packt-init"
INITSCRIPT_PARAMS = "start 99 5 2 . stop 19 0 1 6 ."
```

## 说明

以下是对更新后的 webserver-packet.bb 文件的说明：

- `INITSCRIPT_NAME` 变量表示初始化脚本的文件名，安装后位于 `/etc/init.d`。
- `INITSCRIPT_PARAMS` 变量指定传递给 `update-rc.d` 的选项。在我们的例子中，脚本的运行级别为 99，在 init 级别 2 和 5 启动，在级别 0、1 和 6 停止。

## 注意

如果要了解 update-rc.d 的更多信息，可以访问 Yocto Project 的官方文档：http://www.yoctoproject.org/docs/1.8/mega-manual/mega-manual.html#ref-classes-update-rc.d

## 部署 raspberry-pckt-image

现在我们有了一个功能应用程序 (webserver-pckt)，可以将其添加到自定义镜像 raspberry-pack-image 中：

```
# Base this image on core-image-minimal
include recipes-core/images/core-image-minimal.bb
DESCRIPTION = "Image for raspberry-pi"
IMAGE_FEATURES += "ssh-server-dropbear splash"
# Include modules in rootfs
IMAGE_INSTALL += " \\
 kernel-modules \\
 gpio-pckt \\
 i2c-tools \\
 spitools \\
 nunchuck \\
 v4l-utils \\
 nodejs \\
 webserver-pckt \\
 "
```

做完之后，我们可以开始创建镜像，然后将其挂载到 SD 卡上：

```
$ bitbake raspberry-pckt-image
```

## 测试应用程序

要在 Raspberry Pi 上测试应用程序，只需在您喜欢的浏览器中访问以下地址：

http://raspberry-pi-ip-address:3344

如果整个环境已正确配置，您应该看到以下页面：

![](img/76681dcfb8cd2bd107d0c06ab088f916_181_0.png)

## 本项目的未来

我们探讨了服务器的基本思路，您应该能够对其进行改进，使其尽可能具备工业级水平。可以轻松想象以下应用：

- 监控主板上存在的 ADC
- 通过 HTML 页面向设备发送串行命令 (node-serial)
- 监控 CAN-bus 设备 ( https://github.com/sebi2k1/node-can )

最终项目可能类似于这样：

![](img/76681dcfb8cd2bd107d0c06ab088f916_182_0.png)

## 摘要

在本章中，我们学习了如何创建 Web 界面、如何将其集成到现有环境中，以及如何将其部署到 Raspberry Pi 上。

在下一章中，我们将学习 Linux 内核的一些基础知识，以及如何配置内核以支持各种 LCD 触摸屏。本章还将介绍如何设置 Yocto 项目，以在使用 LCD 触摸屏的 Raspberry Pi 上运行图形应用程序或窗口系统。

## 第 10 章。使用 LCD 触摸屏和 Linux 内核

在使用嵌入式 Linux 系统设置 LCD 触摸屏时，建议具备 Linux 内核及其配置的基础知识。在本章中，我们将学习 Linux 内核的一些基础知识，以及如何使用交叉编译器下载、配置和编译它。在 Raspberry Pi 上运行基于 Linux 的系统时，目前的建议是使用 Raspberry Pi Linux 内核而非主线 Linux 内核；我们将在本章中了解原因。

在本章后面，我们将了解如何使用 Raspberry Pi 7 触摸屏和 PiTFT 2.8 触摸屏，启动基于 Yocto Project 并带有窗口系统的镜像。本章将以使用 Qt 应用程序框架为 Raspberry Pi 开发可通过触摸屏操作的图形应用程序的演示结束。

