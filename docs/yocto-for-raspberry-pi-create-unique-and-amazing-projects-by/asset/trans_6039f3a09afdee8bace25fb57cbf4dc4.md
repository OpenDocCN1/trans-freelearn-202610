
## I. 将 packt_rpi 层添加到构建中

如果我们想安装该层，只需将绝对路径集成到 bblayers.conf 文件中，就像 README 文件中所说的：

要使用此层，需要让构建系统知道它。

假设 packt_rpi 层位于 yocto 构建树的顶级目录，可以通过将 packt_rpi 层的位置添加到 bblayers.conf 中（以及其他所需的层）来将其添加到构建系统，例如：

```
BBLAYERS ?= " \
    /path/to/yocto/meta \
    /path/to/yocto/meta-yocto \
    /path/to/yocto/meta-yocto-bsp \
    /path/to/yocto/meta-packt_rpi \
"
```

我们可以将层添加到 bblayers.conf 文件中：

```
# LAYER_CONF_VERSION is increased each time  build/conf/bblayers.conf
# changes incompatibly
LCONF_VERSION = "6"

BBPATH = "${TOPDIR}"
BBFILES ?= ""

BBLAYERS ?= " \
/home/packet/RASPBERRYPI/poky/meta \
/home/packet/RASPBERRYPI/poky/meta-yocto \
/home/packet/RASPBERRYPI/poky/meta-yocto-bsp \
/home/packet/RASPBERRYPI/poky/meta-raspberrypi \
/home/packet/RASPBERRYPI/poky/meta-openembedded/meta-webserver \
/home/packet/RASPBERRYPI/poky/meta-packt_rpi "
BBLAYERS_NON_REMOVABLE ?= " \
/home/packet/RASPBERRYPI/poky/meta \
/home/packet/RASPBERRYPI/poky/meta-yocto \
```

在此之后，我们的层已完全集成到 Yocto Project 中，因此可以被 BitBake（元调度程序）解析。

## 注

如需更多信息，可阅读官方文档 http://www.yoctoproject.org/docs/1.8/dev-manual/dev-manual.html#yocto-project-layers 。

## 将 gpio-packt 添加到 meta-packt_rpi

为测试此层，我们将集成在 [第 5 章：在树莓派上创建、开发和部署](https://www.packtpub.com/) 中开发的应用程序（gpio-packt）。这样可以让我们逐步实现层，使其完全可重复用于各种项目。包含此配方后，您的层结构应如下所示：

```
$ tree meta-packt_rpi/
meta-packt_rpi/
├── conf
│   └── layer.conf
├── COPYING.MIT
├── README
├── recipes-custom
│   └── gpio-packt
│       ├── gpio-packt
│       │   ├── gpio-example.c
│       │   └── gpio-packt.bb
├── recipes-example
│   └── example
│       ├── example-packt-0.1
│       │   ├── example.patch
│       │   └── helloworld.c
│       └── example-packt_0.1.bb
├── recipes-example-bbappend
│   ├── example-bbappend
│   ├── example-packt-0.1
│   │   └── example.patch
│   └── example-packt_0.1.bbappend
```

与任何其他额外软件包一样，构建系统需要知道您希望包含 gpio-packt。为此，请编辑 /conf/local.conf 配置文件并在底部添加以下行：

```
IMAGE_INSTALL_append = " gpio-packt"
```

此行告诉构建系统在创建最终镜像时包含 gpio-packt 软件包。如果出于某种原因该变量已存在，只需在末尾追加 gpio-packt 软件包名称，如下所示：

```
IMAGE_INSTALL_append = " app1 app2 gpio-packt"
```

现在，如果我们想测试它，可以使用以下命令启动 BitBake，然后在树莓派上测试我们的程序：

```
$ bitbake rpi-basic-image
```

## 修补 gpio-packt

现在我们已经了解如何将配方（gpio-packt）集成到先前创建的层（meta-packt_rpi）中，将了解如何为 gpio-packt 配方创建补丁。

## 生成补丁

第一步是创建 .patch 文件。使用以下命令创建原始文件的备份：

```
$ cd meta-packt_rpi/recipes-custom/gpio-packt
$ cp gpio-packt/gpio-example.c gpio-packt/gpio-example.orig
```

现在，可以修改我们的主源代码（gpio-example.c）：

```
$ sed -i 's/Button Mode/Button Mode!/' gpio-packt/gpio-example.c
```

我们已按需更改源文件；下一步是使用 diff 命令创建补丁文件：

```
$ diff -u gpio-packt/gpio-example.orig gpio-packt/gpio-example.c > gpio-packt/fix.patch
```

现在我们有了 .patch 文件，需要更新主源文件：

```
$ mv gpio-packt/gpio-example.orig gpio-packt/gpio-example.c
```

## 将补丁添加到配方文件

请注意，即使我们已生成 .patch 文件，目前它仍然不可见，因为它已输入到 SRC_URI 变量中。以下是考虑补丁的完整配方，已更新：

```
DESCRIPTION = "gpio example"
LICENSE="GPLv2"
LIC_FILES_CHKSUM = "file://${COMMON_LICENSE_DIR}/GPL-2.0;md5=801f80980d171dd6425610833a22dbe6"
PR = "r0"
SRC_URI = "file://gpio_example.c \
           file://fix.patch"
do_compile() {
	${CC} ${CFLAGS} ${LDFLAGS} ${WORKDIR}/gpio_example.c -o gpio_example
}
do_install() {
	install -m 0755 -d ${D}${bindir}
	install -m 0755 ${S}/gpio_example ${D}${bindir}
}
```

如果我们查看层的结构，将看到以下内容：

```
$ tree meta-packt_rpi/
meta-packt_rpi/
├── conf
│   └── layer.conf
├── COPYING.MIT
├── README
├── recipes-custom
│   └── gpio-packt
│       ├── gpio-packt
│       │   ├── fix.patch
│       │   ├── gpio-example.c
│       └── gpio-packt.bb
├── recipes-example
│   └── example
│       └── example-packt-0.1
│           ├── example.patch
│           ├── helloworld.c
│           └── example-packt_0.1.bb
├── recipes-example-bbappend
│   └── example-bbappend
│       ├── example-packt-0.1
│       │   └── example.patch
│       └── example-packt_0.1.bbappend
```

现在，可以使用以下命令启动 BitBake 来测试我们的新配方：

```
$ bitbake rpi-basic-image
```

## 创建 raspberry-packt-image.bb 镜像

在本章中，我们的目标是创建一个可在任何环境中重复使用的层。我们将了解如何创建自己的镜像，以更多地依赖 meta-raspberrypi BSP 层所提供的镜像。无需担心，因为要创建镜像，只需制作一个配方文件。

## 创建环境

为分离我们层的配方，我们将镜像配方放置在名为 recipe-core 的层中，在那里将创建 raspberry-packt-image.bb 配方文件。创建后，我们的层结构如下：

```
$ tree meta-packt_rpi/
meta-packt_rpi/
├── conf
│   └── layer.conf
├── COPYING.MIT
├── README
├── recipes-core
│   └── images
│       └── raspberry-packt-image.bb
├── recipes-custom
│   └── gpio-packt
│       ├── gpio-packt
│       │   ├── fix.patch
│       │   ├── gpio-example.c
│       │   └── gpio-packt.bb
├── recipes-example
│   └── example
│       └── example-packt-0.1
│           ├── example.patch
│           ├── helloworld.c
│           └── example-packt_0.1.bb
├── recipes-example-bbappend
│   ├── example-bbappend
│   │   └── example-packt-0.1
│   │       └── example.patch
│   └── example-packt_0.1.bbappend
```

## 修改配方文件

为更好地管理我们的镜像，最好的方法就是从 meta-raspberrypi 层中包含的基本镜像学习。

## 注

您可以访问主 GitHub 仓库 https://github.com/agherzan/meta-raspberrypi/blob/master/recipes-core/images/rpi-hwup-image.bb 。

以下是一个基本示例——我们自己的镜像，符合树莓派标准，并包含我们自己的配方：

```
# Base this image on core-image-minimal
include recipes-core/images/core-image-minimal.bb
DESCRIPTION = "Image for raspberry-pi"
IMAGE_FEATURES += "ssh-server-dropbear splash"
# Include modules in rootfs
IMAGE_INSTALL += " \\
 kernel-modules \\
 gpio-packt
"
```

IMAGE_INSTALL 变量将生成根文件系统的相关软件包分组。如需更多信息，可参阅官方文档 http://www.yoctoproject.org/docs/1.8/ref-manual/ref-manual.html#var-IMAGE_INSTALL。

- 另一个可能有趣的变量是 IMAGE_FEATURES，它允许您处理预定义软件包（dev、debug 等）。您可以通过阅读官方文档了解更多信息：

- `allow-empty-password`：允许 Dropbear 和 OpenSSH 接受 root 登录以及密码字符串为空的账户的登录。

- `dbg-pkgs`：为给定镜像中安装的所有软件包安装调试符号软件包。

- `debug-tweaks`：使镜像适合开发（例如，允许无需密码的 root 登录并启用安装后日志记录）。有关更多信息，请参见本列表中的 'allow-empty-password'、'empty-root-password' 和 'post-install-logging' 功能。

- `dev-pkgs`：为给定镜像中安装的所有软件包安装开发软件包（头文件和额外的库链接）。

- `doc-pkgs`：为给定镜像中安装的所有软件包安装文档软件包。

- `empty-root-password`：将 root 密码设置为空字符串，从而允许使用空密码登录。

- `package-management`：安装包管理工具并保留包管理器数据库。

- `post-install-logging`：在目标系统上首次启动镜像时，将安装后脚本运行记录到 `/var/log/postinstall.log` 文件中。

- `ptest-pkgs`：为所有启用 ptest 的配方安装 ptest 软件包。

- `read-only-rootfs`：创建根文件系统为只读的镜像。有关更多信息，请参阅 Yocto Project 开发手册中的“创建只读根文件系统”部分。

- `splash`：在启动期间启用显示启动画面。默认情况下，此画面由 `psplash` 提供，允许自定义。如果您希望使用替代启动画面软件包，可以在镜像配方或分发配置级别中将 `SPLASH` 变量设置为不同的软件包名称（或名称）。

- `staticdev-pkgs`：为给定镜像中安装的所有软件包安装静态开发软件包，即静态库（即 `*.a` 文件）。

