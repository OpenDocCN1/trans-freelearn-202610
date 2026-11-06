# Raspberry Pi 2 服务器精要：将你的 Raspberry Pi 转变为支持整个多媒体世界的多用途 Web 服务器的实用且易懂教程！

## Credits

**作者**
Piotr J Kula

**审阅者**
Ian McAlpine
Cédric Verstraeten

**策划编辑**
Priya Singh

**收购编辑**
Usha Iyer
Reshma Raman

**内容开发编辑**
Sanjeet Rao

**技术编辑**
Jayesh Sonawane

**校对编辑**
Yesha Gangani

**项目协调员**
Judie Jose

**校对员**
Safis Editing

**索引员**
Monica Ajmera Mehta

**图形**
Disha Haria

**制作协调员**
Nilesh Mohite

**封面设计**
Nilesh Mohite

## About the Author

**Piotr J Kula** 从 6 岁时拆开父亲的 IBM PC/XT 机器开始，对计算机技术的热情就被点燃了。机器重建后成功启动，证明了 Piotr 在技术方面与生俱来的能力。

25 年后，Piotr 在不同国家的各种公司工作。他一直解决复杂问题或从事新体验。只要有某种处理器，Piotr 总是第一个尝试、修复或配置它。

今天，Piotr 是其在英国的公司 Kula Solution Ltd. 的首席执行官。Piotr 在多项 Microsoft 技术方面获得认证，并专门从事 Microsoft 和 Linux 技术。

Piotr 的梦想是 Linux 和 Microsoft 之间实现统一体验。他从未因任何一方的负面情绪而气馁，反而努力在这些遥远的巨头之间架起桥梁。在发现 Raspberry Pi 后，他完全用它取代了拥有的所有其他嵌入式设备，并成功发布了 *Raspberry Pi Server Essentials*，由 *Packt Publishing* 出版。

今天，随着 *Raspberry Pi 2 Server Essentials* 的发布，Piotr 距离实现梦想又近了一步。本书主要关于 Linux，但也谈到了在你心爱的 Raspberry Pi 上集成和运行 Windows。

我谨向我的了不起的妻子 Katarzyna Kula 表示感谢，她帮助我决定何时工作已足够！感谢她在我所有项目和无数在电脑前度过的时光中一直支持我。我还要感谢整个 Raspberry Pi 社区，特别是 Stack Exchange 上的成员，*Packt Publishing* 的每一个人，以及以各种方式帮助我完成本书的人。

## About the Reviewers

**Ian McAlpine** 第一次接触计算机是在他学校的 Research Machines RML-380Z，以及他的物理老师的 Compukit UK101。随后是 Sinclair ZX81，然后是他至今仍拥有的 BBC Micro Model A。这份兴趣促使他获得了 Aston University 的电子系统工程 MEng 学位和利物浦大学的信息技术 MSc 学位。Ian 目前在加拿大温哥华 SAP Labs 的 BI & Analytics 能力中心担任产品专家。

Raspberry Pi 的引入不仅重新点燃了他 *tinker* 的欲望，还提供了回馈社区的机会。因此，Ian 积极担任志愿者，参与 The MagPi 的工作，这是一本关于 Raspberry Pi 的月刊杂志，你可以在网上阅读或免费从 `https://www.raspberrypi.org/magpi` 下载。他还持有业余无线电执照（呼号 VE7FTO），并担任当地社区应急管理办公室的通讯志愿者。他曾担任 *Packt Publishing* 出版的 *Raspberry Pi Cookbook for Python Programmers* 和 *Raspberry Pi Projects for Kids* 书籍的技术审阅者。

我谨向我亲爱的妻子 Louise，以及我棒极了的孩子 Emily 和 Molly 表示感谢，感谢她们允许我消失在我的“办公室”里……并感谢她们训练我们的狗来把我找回来！

**Cédric Verstraeten** 拥有工程硕士学位，主要活跃于 C++ 社区。他担任软件工程师，是开源的狂热爱好者。他将大部分时间花在可以自动化和简化人们生活的副项目上。他是 Raspberry Pi Belgium 聚会的组织者，也是开源视频监控系统 Kerberos.io 的创始人。

> 我要感谢 Packt Publishing 允许我作为审阅者参与其中。我真的认为他们的书籍可以为人们提供对特定主题的深入概述。

www.PacktPub.com

## eBooks, discount offers, and more

你知道 Packt 提供每本书的电子书版本吗？文件格式包括 PDF 和 ePub。你可以在 www.PacktPub.com 升级到电子书版本，作为印刷书籍的客户，你有权享受电子书的折扣。请通过 customercare@packtpub.com 与我们联系了解更多详情。

在 www.PacktPub.com，你还可以阅读大量免费技术文章，订阅各种免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/a8753cacc9d454dfc09eda1ed5012bd3_7_0.png)

https://www2.packtpub.com/books/subscription/packtlib

你需要即时解决 IT 问题吗？PacktLib 是 Packt 的在线数字图书库。在这里，你可以搜索、访问和阅读 Packt 的整个图书库。

## Why subscribe?

- 在 Packt 出版的每本书中完全可搜索
- 复制、粘贴、打印和书签内容
- 按需访问，并通过网络浏览器访问

## 前言

本书的目的是帮助你入门 Raspberry Pi，但本书的章节侧重于 Raspberry Pi 2。此处的核心目标是为你的项目提供一些坚实的硬件和编程技巧，这些都是必不可少的。

## 本书涵盖内容

第 1 章《使用 Raspberry Pi 入门》将涵盖我们使用本书所需的基本设备。我们将研究已购买的其他外设，以了解 Raspberry 的工作原理。然后我们将了解如何将最新的 Raspbian 镜像刷写到 SD 卡上。

第 2 章《准备网络》将说明如何设置 LAN 和无线连接以连接到我们的网络连接到互联网。我们将设置一些网络基准并了解其一些局限性。我们还将研究动态 DNS 主机。

第 3 章《配置额外功能》将说明如何更新 Raspberry Pi 的软件和固件。你将学习有关看门狗的知识并了解如何购买额外的解码器许可证。

第 4 章《使用快速 Web 服务器和数据库》将说明如何使用 nginx 和 PHP 设置快速 Web 服务器，我们可以决定是否使用 MySQL 或 SQLite。

第 5 章《设置 Raspberry Pi 作为文件服务器》将说明如何连接和格式化 USB 存储介质。我们不仅会研究向 Raspberry Pi 发送数据的各种方式，还将了解如何在网络上共享媒体。作为额外任务，我们将研究创建硬件 RAID！

第 6 章《设置游戏服务器》将探索存储库中可用的开源游戏引擎。我们还将对处于测试阶段的 Jessie 存储库进行一窥。

第 7 章《流式传输实时高清视频》将探索摄像头模块并说明流式传输视频的简单技术。本章包含独家的流媒体教程。

第 8 章《将 Pi 设置为媒体中心服务器》将说明如何连接 HD 显示器并播放之前录制或存储的视频。我们还将研究 OSMC，并探索将其作为专用媒体播放器运行的好处。

第 9 章《使用电池电源运行 Pi》将解释各种电池技术的优点以及如何在不花费太多钱的情况下获得最佳性能。

第 10 章《Windows IoT Core》现已在嵌入式 ARM 上可用，它将探索将 IoT 作为操作系统运行的能力，并附带使用 C# 的基础编程教程。

第 11 章《运行自己的 ownCloud》将讨论如何在 Pi 上安装 ownCloud 软件，并让你摆脱私有服务。

第 12 章《物联网 – 云端传感器》更多的是一个案例研究，说明如何通过存储可在云端任何地方访问的准确数据来完成你的项目。

## 你需要为本书准备的内容

你至少需要一台 Raspberry Pi 2，推荐 8 GB SD 卡，1 安培 micro USB 电源，以及连接到具有互联网的路由器的网线。

## 本书适合谁

寻求为一些新技术项目获取灵感？想要从你的 Raspberry Pi 中获得更多？本书就是专门为你创建的！

## 约定

在本書中，你会找到多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义说明。



文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号显示如下："我们可以通过使用 `include` 指令来包含其他上下文。"

代码块设置如下：

```
auto lo

    iface lo inet loopback
    iface eth0 inet dhcp
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
allow-hotplug wlan0
**auto wlan0**

    iface wlan0 inet dhcp
```

任何命令行输入或输出均按如下方式编写：

```
sudo touch wpa_supplicant.conf
nano wpa_supplicant.conf
```

**新术语** 和 **重要词语** 以粗体显示。您在屏幕上看到的文字，例如菜单或对话框中的文字，在文本中如下所示："点击 **下一步** 按钮会将您带到下一个屏幕。"

![](img/a8753cacc9d454dfc09eda1ed5012bd3_19_0.png)

警告或重要说明以如下框形式出现。

![](img/a8753cacc9d454dfc09eda1ed5012bd3_19_1.png)

提示和技巧如下所示。

## 读者反馈

欢迎读者反馈。请告诉我们您对本书的看法——您喜欢或不喜欢的内容。读者反馈对我们非常重要，因为它有助于我们开发您真正能充分利用的标题。

要发送一般反馈，只需发送电子邮件至 `feedback@packtpub.com`，并在邮件主题中提及本书的标题。

如果您在某个主题方面有专业知识，并且有兴趣撰写或为书籍做出贡献，请参阅我们的作者指南 www.packtpub.com/authors。

## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供多项服务帮助您充分利用您的购买。

## 下载示例代码

您可以从 http://www.packtpub.com 上的账户下载本书的示例代码文件。如果您在其他地方购买了本书，可以访问 http://www.packtpub.com/support 并注册以便直接将文件发送到您的电子邮件。

您可以按照以下步骤下载代码文件：

- 1. 使用您的电子邮件地址和密码登录或注册我们的网站。
- 2. 将鼠标指针悬停在顶部的 **支持** 选项卡上。
- 3. 点击 **代码下载与勘误**。
- 4. 在 **搜索** 框中输入书名。
- 5. 选择您要下载代码文件的书籍。
- 6. 从下拉菜单中选择您购买此书的渠道。
- 7. 点击 **代码下载**。

您还可以通过点击 Packt Publishing 网站上本书网页上的 **代码文件** 按钮下载代码文件。可以通过在 **搜索** 框中输入书名来访问此页面。请注意，您需要登录到您的 Packt 账户。

下载文件后，请确保使用最新版本的以下工具解压或提取文件夹：

- WinRAR / 7-Zip for Windows
- Zipeg / iZip / UnRarX for Mac
- 7-Zip / PeaZip for Linux

## 下载本书的彩色图片

我们还为您提供一份 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图片。彩色图片将帮助您更好地理解输出的变化。您可以从 http://www.packtpub.com/sites/default/files/downloads/RaspberryPi2ServerEssentials_ColorImages.pdf 下载此文件。

## 勘误

虽然我们已尽一切努力确保内容的准确性，但错误难免。如果您在我们的书籍中发现错误——可能是文本或代码中的错误——我们将非常感谢您向我们报告。通过这样做，您可以避免其他读者的沮丧，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择您的书籍，点击勘误提交表单链接，并输入勘误的详细信息。一旦您的勘误得到验证，您的提交将被接受，勘误将被上传到我们的网站或添加到该标题勘误部分下已有的勘误列表中。

要查看之前提交的勘误，请访问 https://www.packtpub.com/books/content/support 并在搜索字段中输入书名。所需信息将显示在勘误部分下。

## 盗版

互联网上版权材料的盗版是所有媒体持续存在的问题。在 Packt，我们非常重视版权和许可证的保护。如果您在互联网上发现我们作品的任何形式的非法复制品，请立即向我们提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 与我们联系，并附上疑似盗版材料的链接。

我们感谢您帮助保护我们的作者以及我们为您提供有价值内容的能力。

## 问题

如果您对本书的任何方面有问题，可以联系我们 questions@packtpub.com，我们将尽力解决问题。
