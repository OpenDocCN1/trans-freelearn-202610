

## 本书内容概要

第 1 章，认识 Yocto Project，介绍了 Yocto Project 的基本概念。将讨论 Yocto Project、OpenEmbedded Core、Poky 和 BitBake 的历史。

第 2 章，为 Raspberry Pi 构建第一个 Poky 镜像，教你如何使用 Yocto Project 为 Raspberry Pi 创建第一个镜像以及如何运行它。

第 3 章，使用 Hob 和 Toaster 进行构建，教你如何使用用户友好的界面 Hob 和 Toaster。

第 4 章，理解 BitBake，为你提供对 BitBake 的更深入理解。

第 5 章，在 Raspberry Pi 上创建、开发和部署，教你如何将自定义应用程序与 Raspberry Pi 集成。这将包括学习如何生成用于交叉编译应用程序的 SDK。

第 6 章，使用外部层，带你了解层的工作原理以及如何将外部层与 Raspberry Pi 项目集成。

第 7 章，在 Raspberry Pi 上部署自定义层，探讨如何使用 Yocto Project 提供的不同工具生成自定义层。

第 8 章，深入探讨 Raspberry Pi 的外设和 Yocto 配方，教你如何通过 Yocto Project 处理 Raspberry Pi 的 SPI 和 I2C 总线。你还将学习如何为自定义应用程序创建自己的配方。

第 9 章，在 Raspberry Pi 上制作媒体中心，介绍如何部署自定义应用程序以构建嵌入式媒体中心，例如可用于监控 CPU 温度。该解决方案将需要 HTML5 和 Node.js 来设置远程监控温度所需的 Web 界面。

第 10 章，玩转 LCD 触摸屏和 Linux 内核，介绍了 Linux 内核的一些基础知识以及如何配置它以支持各种 LCD 触摸屏。随后，本章解释了如何设置 Yocto Project，在 Raspberry Pi 上使用 LCD 触摸屏运行图形应用程序或窗口系统。

第 11 章，为 Raspberry Pi BSP 层做出贡献，教你如何为 meta-raspberrypi 层做出贡献。

第 12 章，家庭自动化项目 – 启动自定义镜像，是本书的最后一章，我们将在其中总结本书中所学的所有内容。这是通过创建一个家庭自动化项目来完成的。

## 本书所需内容

- 一台配备受支持主机系统的 Linux 工作站（请参阅 http://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html#detailed-supported-distros）
- 主机系统所需的软件包（请参阅 http://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html#required-packages-for-the-host-development-system）
- Git、tar 和 Python 的必需版本（请参阅 http://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html#required-git-tar-and-python-versions）

## 本书适用人群

本书面向希望了解 Yocto Project 的嵌入式软件学生、嵌入式 Linux 工程师以及精通 Raspberry Pi（或其他 ARM 平台）的嵌入式系统爱好者。

本书是熟练掌握并拓宽知识以便将其应用于嵌入式开发的理想方式。

如果你在寻找一本能帮助你在 Raspberry Pi 和 Yocto Project 上进行开发的书籍，那么这本书就是你需要的那一本。

## 约定

在本书中，你会发现多种文本样式，用于区分不同种类的信息。以下是这些样式的一些示例及其含义解释。

文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号将显示为：“要运行任务，BitBake 将首先查找名为 do_ <task name> 的环境变量，该变量将包含要执行的任务代码（使用 Python 或 shell）。”

代码块设置如下：

```
# LAYER_CONF_VERSION is increased each time build/conf/bblayers.conf
# changes incompatibly
LCONF_VERSION = "6" BBPATH = "${TOPDIR}" BBFILES ?= "" BBLayers ?= " \
/home/packet/RASPBERRYPI/poky/meta \
/home/packet/RASPBERRYPI/poky/meta-yocto \
/home/packet/RASPBERRYPI/poky/meta-yocto-bsp \
/BBLAYERS_NON_REMOVABLE ?= " \
/home/packet/RASPBERRYPI/poky/meta \
/home/packet/RASPBERRYPI/poky/meta-yocto \
"
```

任何命令行输入或输出都写为：

```
$ modprobe spidev
```

**新术语**和**重要单词**以粗体显示。你看到屏幕上的单词，例如，在菜单或对话框中，会出现在文本中，如：“添加一个层非常简单。我们只需单击 **图层** 按钮。”

### 注意

警告或重要说明会以类似这样的框出现。

### 技巧

技巧和窍门会以这样的形式出现。

## 读者反馈

我们始终欢迎来自读者的反馈。请告诉我们你对本书的看法——你喜欢或不喜欢什么。读者反馈对我们很重要，因为这有助于我们开发你能真正受益的书籍。如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在消息主题中注明书名。如果你在某个主题方面有专长，并且有兴趣编写或参与编写书籍，请参阅我们的作者指南 www.packtpub.com/authors。

## 客户支持

既然你是 Packt 图书的自豪拥有者，我们有许多资源可以帮助你充分利用你的购买。

## 下载示例代码

你可以从你在 http://www.packtpub.com 上的账户下载本书的示例代码文件。如果你在其他地方购买了此书，可以访问 http://www.packtpub.com/support 并注册，以便文件直接发送到你的邮箱。

你可以按照以下步骤下载代码文件：

1. 使用你的电子邮件地址和密码登录或注册我们的网站。
2. 将鼠标指针悬停在顶部的 SUPPORT 选项卡上。
3. 单击代码下载与勘误。
4. 在搜索框中输入书名。
5. 选择你要下载代码文件的书籍。
6. 从下拉菜单中选择你购买此书的地方。
7. 单击代码下载。

文件下载完成后，请确保使用最新版本的以下工具解压或提取文件夹：

- WinRAR / 7-Zip for Windows
- Zipeg / iZip / UnRarX for Mac
- 7-Zip / PeaZip for Linux

本书的代码包也托管在 GitHub 上，地址为 https://github.com/PacktPublishing/Yocto-for-Raspberry-Pi。我们还在丰富的图书和视频目录中提供了其他代码包，地址为 https://github.com/PacktPublishing/。去看看吧！

## 下载本书的彩色图片

我们还为你提供了一个包含本书中使用的屏幕截图/图表彩色图片的 PDF 文件。彩色图片将帮助你更好地理解输出中的变化。你可以从 https://www.packtpub.com/sites/default/files/downloads/YoctoforRaspberryPi_ColorImages.pdf 下载此文件。

## 勘误

虽然我们已尽一切努力确保内容的准确性，但错误仍难免发生。如果你在我们的某本书中发现错误——无论是文本中的错误还是代码中的错误——我们将不胜感激，如果你能将其报告给我们。这样做，你可以避免其他读者感到沮丧，并帮助我们改进本书的后续版本。如果你发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择你的书籍，单击 **勘误提交表单** 链接，并输入你的勘误详细信息。一旦你的勘误得到验证，你的提交将被接受，勘误将上传到我们的网站，或添加到该书名下勘误部分的任何现有勘误列表中。

要查看之前提交的勘误，请访问 https://www.packtpub.com/books/content/support 并在搜索字段中输入书名。所需信息将出现在 **勘误** 部分下。

