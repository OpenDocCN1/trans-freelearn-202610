

# 书籍章节

- [第 14 章](#B19922_14.xhtml#_idTextAnchor370)：功能监控、警报和操作监视器：高级监控技术。本章将向读者展示 Anypoint Monitoring 提供的功能，以及可以在 API 和服务器上配置的不同警报。读者将了解功能监控以及 Anypoint Monitoring 的一些高级功能。
- [第 15 章](#B19922_15.xhtml#_idTextAnchor405)：在一个平台上通过通用 API 管理控制 API 蔓延。本章将向读者展示如何使用 API Manager 管理 MuleSoft API 和非 MuleSoft API 的生命周期。读者将了解 Mule 网关、Flex 网关、服务网格、API 代理、分析、警报等。
- [第 16 章](#B19922_16.xhtml#_idTextAnchor442)：从构想到运营，应对非功能性需求。本章将向读者介绍实施非功能性需求，如高可用性 (HA)、容错、弹性，以及分配和优化 vCore 以提升应用程序性能。
- [第 17 章](#B19922_17.xhtml#_idTextAnchor466)：备考成功。本章将向读者展示如何准备 MuleSoft 认证平台架构师考试 - 1 级以及考试的资格标准。它将说明 MCPA 考试的重要主题，以及考试的建议和指南。
- [第 18 章](#B19922_18.xhtml#_idTextAnchor483)：攻克棘手主题。MuleSoft 考试并非旨在刁难。它们的设计和开发旨在将标准设定在恰当的水平，以确保考生掌握材料。偶尔，平台架构师考试涵盖的一些主题稍微难一些，可能显得棘手。本章将探讨那些可能更难的主题。

# 充分利用本书

| 本书涵盖的软件/硬件 | 操作系统要求 |
| --- | --- |
| Java 8 或 Java 17 | Windows、macOS 或 Linux |
| Maven | |
| Anypoint Studio IDE 和 Anypoint Platform | |

# 下载彩色图片

我们还提供了一个 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。您可以在此处下载：[https://packt.link/gbp/9781805126188](https://packt.link/gbp/9781805126188)。

## 图片免责声明

本书中的某些图片仅用于提供上下文，图形的可读性对讨论并非至关重要。请参考我们的免费图形包以下载图片。您可以从 [https://packt.link/gbp/9781805126188](https://packt.link/gbp/9781805126188) 下载图片。

# 约定用法

全书使用了多种文本约定。

## 文本中的代码

表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。例如："备份和恢复过程通过 `rtfctl` 很简单。"

## 代码块

代码块设置如下：

```
<plugin>
  <groupId>org.mule.tools.maven</groupId>
  <artifactId>mule-maven-plugin</artifactId>
  <version>3.8.6</version>
  <extensions>true</extensions>
  <configuration>
    <standaloneDeployment>
      <muleHome>${mule.home }</muleHome>
      <muleVersion>${app.runtime}</muleVersion>
    </standaloneDeployment>
  </configuration>
</plugin>
```

## 命令行输入/输出

任何命令行输入或输出均按如下方式编写：

```
./rtfctl backup <path_to_store_backup>
```

## 粗体

表示新术语、重要单词或您在屏幕上看到的单词。例如，菜单或对话框中的单词以**粗体**显示。例如："专门检查**设计架构与实现**，如*图 3.1*所示，我们发现了一个可能有用的剧本步骤。"

## 提示或重要说明

显示如下。

# 联系我们

我们始终欢迎读者的反馈。

## 一般反馈

如果您对本书的任何方面有疑问，请发送邮件至 [customercare@packtpub.com](mailto:customercare@packtpub.com)，并在邮件主题中注明书名。

## 勘误

虽然我们已尽一切努力确保内容的准确性，但错误难免发生。如果您在本书中发现错误，我们将不胜感激如果您能向我们报告。请访问 [www.packtpub.com/support/errata](http://www.packtpub.com/support/errata) 并填写表格。

## 盗版

如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激如果您能向我们提供位置地址或网站名称。请通过 [copyright@packt.com](mailto:copyright@packt.com) 联系我们，并附上相关材料的链接。

## 如果您有兴趣成为作者

如果您在某个主题上有专长，并有兴趣撰写或为书籍做贡献，请访问 [authors.packtpub.com](http://authors.packtpub.com)。

# 分享您的想法

读完 *MuleSoft Platform Architect's Guide* 后，我们非常希望听到您的想法！请[点击此处直接前往亚马逊评论页面](https://packt.link/r/1805126180)为本书留下反馈。

您的评论对我们和技术社区都很重要，将帮助我们确保提供优质内容。

# 下载本书的免费 PDF 副本

感谢您购买本书！

您是否喜欢随时随地阅读，却无法随身携带纸质书？

您购买的电子书是否与您选择的设备不兼容？

别担心，现在购买每本 Packt 书籍，您都可以免费获得该书的无 DRM 保护的 PDF 版本。

随时随地，在任何设备上阅读。直接从您喜爱的技术书籍中搜索、复制并粘贴代码到您的应用程序中。

福利不止于此，您还可以获得独家折扣、新闻通讯和每天发送到您收件箱的精彩免费内容。

请遵循以下简单步骤获取福利：

1. 扫描二维码或访问下方链接

![二维码](image/B19922_QR_Free_PDF.jpg)

[https://packt.link/free-ebook/9781805126188](https://packt.link/free-ebook/9781805126188)

2. 提交购买证明

3. 就这些！我们会将免费 PDF 和其他福利直接发送到您的邮箱。

# 1. 什么是 MuleSoft 平台？

在本章中，我们首先从高层视角审视 MuleSoft Anypoint Platform。然后我们将回顾不同集成方法的演变，以及现代集成方法如何融入 MuleSoft 平台。我们将了解构成 MuleSoft 核心构建块的不同组件，以及它们之间的关系。这些组件构成了该平台可用架构能力的基础。接下来，本章将描述这项技术在任何组织中的定位，特别是那些希望通过现代化、数字创新和业务转型来把握未来的组织。我们还将了解 MuleSoft 作为**集成平台即服务** 的能力，以及为什么 MuleSoft 在现代集成方法中至关重要。

在本章中，我们将涵盖以下主要主题：

- 什么是 MuleSoft 和 iPaaS？
- 集成方法是如何演变的？
- MuleSoft 的架构能力
- 解决现代集成挑战
- MuleSoft 架构如何交付现代集成

## 技术要求

本章讨论的许多 MuleSoft 组件和服务均可通过注册免费试用 MuleSoft 账户并下载 MuleSoft Anypoint Studio 获得。此外，您还可以在此处找到本章的 GitHub 仓库。

- 下载 MuleSoft Anypoint Studio（访问 [https://www.mulesoft.com/lp/dl/anypoint-mule-studio](https://www.mulesoft.com/lp/dl/anypoint-mule-studio)）。
- 填写表格，提供：
  - 电子邮件地址
  - 操作系统

