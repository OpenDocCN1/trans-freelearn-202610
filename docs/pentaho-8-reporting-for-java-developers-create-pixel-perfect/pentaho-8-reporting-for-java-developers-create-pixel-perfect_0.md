# Java 开发者 的 Pentaho 8 报表：使用报表工具创建像素级完美的分析报表




## 致谢

| **作者** | **校对编辑** |
|---|---|
| Francesco Corti | Safis Editing |
| **审阅者** | **项目协调员** |
| Ruben Oliva Ramos
Juan Tomas Oliva Ramos | Nidhi Joshi |
| **委约编辑** | **校对员** |
| Amey Varangaonkar | Safis Editing |

| **收购编辑**<br>Viraj Madhav | **索引员**<br>Tejal Daruwale Soni |
|---|---|
| **内容开发编辑**<br>Cheryl Dsa | **图形**<br>Tania Dutta |
| **技术编辑**<br>Dinesh Pawar | **生产协调员**<br>Arvindkumar Gupta |

## 关于作者

**Francesco Corti** 是一位充满热情的软件解决方案顾问，喜欢在开发、销售和客户团队中工作。作为一名软件工程师，他经常参与售前演示、公开演讲和 IT 课程。软件开发、架构设计以及在 ECM/BPM 和 BI 领域的解决方案定义是他最感兴趣的领域。他在近 20 年的经验中完成了数十个项目，从非常小的项目到更复杂的项目。

在 Alfresco 任产品布道师，Francesco 在开发者社区代表著名的开源 ECM。除了帮助开发者采用 Alfresco 技术外，他还经常通过演讲、文章、博客、用户演示、录制演示或创建示例项目来帮助 Alfresco 改善开发者体验。

他是 Alflytics（之前名为 Alfresco Audit Analytics and Reporting）的发明者和主要开发者，该业务智能解决方案基于 Alfresco ECM，完全基于 Pentaho 套件构建。他撰写了包含 40 多个视频的 Pentaho Reporting 视频课程，以及关于 Pentaho Reporting Designer 和 SDK 的课程。

Francesco 在企业内容管理解决方案方面拥有专业和主要经验，使用 Alfresco ECM 和 Hyland OnBase（他是 OnBase 认证安装人员）；使用 Activiti、JBPM 和 Hyland OnBase 的业务流程管理解决方案；使用 Ephesoft、Hyland OnBase 和定制软件的数据捕获解决方案；使用 O'Neil 软件和定制软件（使用 Alfresco ECM 和 Hyland OnBase）的记录管理解决方案；以及使用 Liferay 和 MS SharePoint 的门户和协作。

## 关于审阅者

**Ruben Oliva Ramos** 是 Tecnologico de Leon Institute 的计算机系统工程师，拥有萨尔瓦多巴霍大学 Leon 分校计算机与电子系统工程硕士学位，专业为电信信息和网络。他拥有超过 5 年的开发用于控制和监控与 Arduino 和 Raspberry Pi 相连设备的 Web 应用的经验，使用 Web 框架和云服务构建物联网应用。

他是萨尔瓦多巴霍大学的机电一体化教师，教授机电一体化系统设计与工程硕士课程的学生。Ruben 还在墨西哥 Leon 的 Centro de Bachillerato Tecnologico Industrial 225 工作，教授机电一体化技术专业的电子、机器人与控制、自动化和微控制器等课程；他是监控系统和数据记录器数据项目的顾问和开发者，使用技术包括 Android、iOS、Windows Phone、HTML5、PHP、CSS、Ajax、JavaScript、Angular 和 ASP.NET，数据库包括 SQLite、MongoDB 和 MySQL，Web 服务器包括 Node.js 和 IIS，硬件编程包括 Arduino、Raspberry Pi、Ethernet Shield、GPS 和 GSM/GPRS、ESP8266，以及用于数据采集和编程的控制和监控系统。

他撰写了 Packt Publishing 出版的书籍 *Internet of Things Programming with JavaScript*。他还参与使用 Arduino 和 Visual Basic .NET 为 Alfaomega 进行监控、控制和数据采集。

*我谨向我的救世主和主耶稣基督致谢，感谢祂给予我追求此项目的力量和勇气；我最亲爱的妻子 Mayte；我们两个可爱的儿子 Ruben 和 Dario；我亲爱的父亲 Ruben；我最亲爱的母亲 Rosalia；我的兄弟 Juan Tomas；以及我深爱的姐姐 Rosalia，感谢你们在审阅本书期间给予我的所有支持，让我得以追求我的梦想，并在我忙碌的工作日后容忍我不在你们身边。*

**Juan Tomas Oliva Ramos** 是瓜纳华托大学的环境工程师，拥有行政工程与质量硕士学位。他拥有超过 5 年在专利管理与开发、技术创新项目以及通过过程统计控制开发技术解决方案的经验。他自 2011 年起担任统计学、创业和项目技术开发的教师。他一直对通过技术改进和创新流程感兴趣。他成为创业导师和技术管理顾问，并在 Instituto Tecnologico Superior de Purisima del Rincon 创立了技术管理与创业新部门。

他参与了书籍 *Wearable Designs for Smart Watches, Smart TVs and Android Mobile Devices* 的工作。

他通过编程和自动化技术开发了用于改进运营的原型，已申请专利。

> *我想感谢上帝赐予我审阅本书的智慧和谦卑。我想感谢 Ruben 邀请我参与这场冒险。我还要感谢我的妻子 Brenda，我们两个神奇的公主 Regina 和 Renata，以及我们即将到来的成员 Tadeo。你们都是我的力量和幸福，我的愿望是为你们做到最好。*

## www.PacktPub.com

有关您本书的支持文件和下载，请访问 [www.PacktPub.com](www.PacktPub.com)。您知道 Packt 为每本出版的书籍提供电子书版本，可用 PDF 和 ePub 文件吗？您可以在 [www.PacktPub.com](www.PacktPub.com) 升级到电子书版本，并且作为印刷书籍客户，您有权享受电子书副本的折扣。

如需更多详情，请联系我们 `service@packtpub.com`。在 [www.PacktPub.com](www.PacktPub.com)，您还可以阅读大量免费技术文章，订阅一系列免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/9337ac94537bf7e092e1ea6531dce8fd_10_0.png)

[https://www.packtpub.com/mapt](https://www.packtpub.com/mapt)

使用 Mapt 获得最受欢迎的软件技能。Mapt 可让您完全访问所有 Packt 书籍和视频课程，以及业界领先的工具，帮助您规划个人发展并推进职业生涯。



## 为什么订阅？
-   在 Packt 发行的每本书中均可完全搜索
-   复制粘贴、打印和书签内容
-   按需访问，并通过 Web 浏览器访问

## 客户反馈
感谢您购买本 Packt 出版的图书。在 Packt，质量是我们编辑流程的核心。为了帮助我们改进，请在本书的亚马逊页面上留下诚实的评价，网址为 https://www.amazon.com/dp/1788298993。

如果您想加入我们的常驻评审团队，您可以发送电子邮件至 customerreviews@packtpub.com。我们会用免费的电子书和视频奖励我们的常驻评审，以换取他们宝贵的反馈。帮助我们不断改进产品！

献给我的妻子，Natalija

## 前言

你有多少次被要求在几天或更短时间内使用基于 Web 的解决方案或自定义 Java 项目，以最先进的技术开发出具有吸引力的报表，并且格式各不相同？如果是这种情况，本书将帮助你使用 Pentaho 套件中的专用工具 Pentaho Reporting 实现这一目标。

本书是面向 Java 开发者和信息技术人员的权威指南，介绍这款开源报表工具，可让你基于各种数据源，使用可视化设计环境和 SDK 将 Pentaho Reporting Engine 嵌入应用程序中，创建像素级完美的 PDF、Excel、HTML、文本、RTF、XML 和 CSV 报表。

本书在 Pentaho 版本 8 即将正式发布时撰写，主要基于该版本。功能、技巧和最佳实践的描述采用学习-by-示例的方法。为了更好地指导你了解不同主题并定义学习路径，本书分为四个主要部分：

- Introduction and basics of Pentaho Reporting
- Core concepts and advanced features of Report Designer
- All about the Reporting SDK
- Additional Pentaho Reporting topics



## 本书涵盖内容

第 1 章，*Pentaho Reporting 简介*，涵盖了 Pentaho Reporting 的典型用途、历史和起源，以及 Pentaho Reporting 提供的报告功能的更详细概述。

第 2 章，*Report Designer 入门*，专注于快速上手 Pentaho Reporting。它将首先介绍构建和嵌入报告的环境设置。从那里开始，读者将逐步学习使用向导创建报告，并在本章结束时学习如何将报告保存到文件系统。

第 3 章，*Reporting SDK 入门*，介绍 Pentaho Reporting SDK。读者将逐步学习如何将现有报告嵌入到通用的 Java 应用程序中。

第 4 章，*使用 Report Designer 创建报告*，更进一步，涵盖了从头创建报告（而不是使用向导）时最常用的功能。核心概念，例如自定义报告、将其保存到 Pentaho 仓库中以及预览内容，将逐步详细说明。

第 5 章，*Report Designer 中的设计与布局*，深入探讨 Pentaho Report Designer 与设计和布局相关的所有概念和功能。读者将深入探索使用 Pentaho Report Designer 进行报告设计和布局的方方面面。

第 6 章，*配置 JDBC 和其他数据源*，教你 Pentaho Reporting 能够连接的所有方式中数据源的配置。数据源定义将使用 Pentaho Reporting Designer 进行演示。

第 7 章，*在报告中包含图形和图表*，展示如何在 Pentaho 报告中加入图表和图形。你将了解支持的不同类型的图表以及如何在 Pentaho Report Designer 中配置它们。本章还将帮助你使用各种类型的数据填充图表。

第 8 章，*参数化、函数、变量和公式*，首先学习如何参数化报告。本章涵盖可在报告中使用的所有预定义函数和表达式。它将帮助读者了解 Pentaho Reporting 的公式功能，包括正确的语法和可用的公式方法。

第 9 章，*国际化与本地化*，展示国际化和本地化报告的细节。

第 10 章，*子报表和交叉表*，涵盖从头构建一些子报表和交叉表的示例，探索 Pentaho Reporting 提供的功能。我们将构建一个多页子报表，演示用于管理页眉的 sticky 标志的使用，并构建一个带有汇总图表的子报表。

第 11 章，*PRPT 格式及构建它的 Java API*，讲解 Pentaho Reporting 的 PRPT 包格式以及 Pentaho Reporting Java API 的细节。本章重点介绍代表 Pentaho 报告的 PRPT 文件的组成以及如何使用 Java API 构建它。

第 12 章，*使用数据源开发*，展示将数据加载到 Pentaho Reporting Engine 的各种方法，并深入探讨 Pentaho Reporting Engine 如何与这些数据源交互以呈现报告。

第 13 章，*使用 Java 进行国际化、子报表和交叉表*，涵盖报告参数化的细节。你还将学习动态呈现报告的各种方式，包括从 Java 角度学习所有可用的函数和表达式。

第 14 章，*构建交互式报告*，教你如何在报告中启用交互功能。你还能够修改报告定义以生成超链接事件和许多不同的 HTML JavaScript 事件。你将学习如何在服务器上呈现 HTML 报告，以及在呈现的 HTML 报告中包含外部脚本文件。

第 15 章，*在 Pentaho Business Analytics Platform 中使用报告*，展示 Pentaho Business Analytics Platform 的基础知识以及使用存储在仓库中或通过仪表板访问的 Pentaho 报告的最佳实践。

第 16 章，*在 Pentaho Data Integration 中使用报告*，介绍如何在 Pentaho 套件内部使用开发的 Pentaho 报告，特别是与 Pentaho Data Integration（Kettle）一起开发管理 Pentaho 报告的作业/转换。

第 17 章，*Pentaho Reporting 每日构建与支持*，涵盖各种有用、简短和高级的 Pentaho Reporting 主题。

## 阅读本书所需条件

Pentaho Reporting 工具是跨平台应用程序，可在 Linux、Windows、macOS 和其他 Java 支持的环境中运行。Reporting Engine 与之前版本的 JDK 向后兼容，但始终建议使用最新版本。对于 Pentaho 8，建议使用 JDK 版本 1.8。

本书中的所有示例均使用 Ubuntu 操作系统 v16.04 LTS、4 GB RAM 和 Intel i7 处理器开发。开发环境由 Java JVM 1.8.0_131、Apache Maven 3.3.9、Git 版本 2.7.4 组成。此配置并非运行示例的强制要求，但作为参考和建议分享，以帮助成功运行示例。

如果你是信息技术人员且不想涉及开发任务，则无需在你的笔记本电脑上安装 Apache Maven 和 Git。

## 本书适合谁阅读

本书面向两类专业人士和学生：具备数据库基础知识的信息技术人员以及具有中等资历的 Java 开发者。开发者将有兴趣了解如何在第三方 Java 应用程序中嵌入报告。

## 约定

本书中你会找到多种文本样式，用于区分不同类型的信息。以下是这些样式的示例及其含义说明。文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、示例 URL、用户输入和 Twitter 账号显示如下："你还将学习 Pentaho Reporting Engine 数据 API 规范的细节，如果必要，可以实现自己的 DataFactory。"

代码块设置如下：

// Defining the connection provider.
DriverConnectionProvider provider = new DriverConnectionProvider();
provider.setDriver("org.hsqldb.jdbcDriver");
provider.setProperty("user", "pentaho_user");
provider.setProperty("password", "password");
provider.setUrl("jdbc:hsqldb://resources/sampledata/sampledata");

当我们希望引起你对代码块中特定部分的注意时，相关行或项目以粗体显示：

// Defining the connection provider.
DriverConnectionProvider provider = new DriverConnectionProvider();
provider.setDriver("org.hsqldb.jdbcDriver");
provider.setProperty("user", "pentaho_user");
provider.setProperty("password", "password");
**provider.setUrl("jdbc:hsqldb://resources/sampledata/sampledata");**

任何命令行输入或输出均按如下方式书写：

```
mvn clean install
mvn package
java -jar target/dependency/jetty-runner.jar target/*.war
```

**新术语**和**重要词语**以粗体显示。你在屏幕上看到的文字，例如在菜单或对话框中，会在文本中这样出现："点击 OK 按钮会带你进入下一个屏幕。"

![](img/9337ac94537bf7e092e1ea6531dce8fd_37_0.png)

*警告或重要说明以这种方式出现。*

![](img/9337ac94537bf7e092e1ea6531dce8fd_38_0.png)

*技巧以这种方式出现。*

## 读者反馈

读者反馈总是受欢迎的。请告诉我们你对本书的看法——你喜欢或不喜欢的地方。读者的反馈对我们很重要，因为它帮助我们开发出你真正能充分利用的书目。如需发送一般反馈，只需发送电子邮件至 `feedback@packtpub.com`，并在邮件主题中提及书名。如果有你擅长的主题，并且你有兴趣撰写或为一本书做出贡献，请参阅我们的作者指南 [作者指南](http://www.packtpub.com/authors)。



## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供多项帮助，让您充分利用此次购买。

## 下载示例代码

您可以在 http://www.packtpub.com 上的账户中下载本书的示例代码文件。如果您在其他地方购买了本书，您可以访问 http://www.packtpub.com/support 并注册，以便将文件直接通过电子邮件发送给您。

您可以通过以下步骤下载代码文件：

- 1. 使用您的电子邮件地址和密码登录或注册我们的网站。
- 2. 将鼠标指针悬停在顶部的 SUPPORT 选项卡上。
- 3. 点击 Code Downloads & Errata。
- 4. 在搜索框中输入书名。
- 5. 选择您要下载代码文件的书籍。
- 6. 从下拉菜单中选择您购买本书的渠道。
- 7. 点击 Code Download。

下载文件后，请确保使用最新版本的以下工具解压或提取文件夹：

- Windows 系统的 WinRAR / 7-Zip
- Mac 系统的 Zipeg / iZip / UnRarX
- Linux 系统的 7-Zip / PeaZip

本书的代码包也在 GitHub 上托管，地址为 https://github.com/PacktPublishing/Pentaho-8-Reporting-for-Java-Developers。我们丰富的图书和视频目录中的其他代码包也可在 https://github.com/PacktPublishing/ 获取。快去看看吧！

## 下载本书的彩色图片

我们还为您提供一份 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图片。彩色图片将帮助您更好地理解输出的变化。您可以从 https://www.packtpub.com/sites/default/files/downloads/Pentaho8ReportingforJavaDevelopers_ColorImages.pdf 下载此文件。

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误难免。如果您在我们的某本书中发现错误——可能是文本或代码中的错误——我们将非常感谢您向我们报告。这样做可以避免其他读者感到沮丧，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 [http://www.packtpub.com/submit-errata](http://www.packtpub.com/submit-errata)，选择您的书籍，点击勘误提交表单链接，并输入勘误详情。一旦您的勘误得到验证，您的提交将被接受，勘误将被上传到我们的网站，或添加到该标题勘误部分下现有勘误列表中。要查看之前提交的勘误，请前往 [https://www.packtpub.com/books/content/support](https://www.packtpub.com/books/content/support)，并在搜索字段中输入书名。所需信息将显示在勘误部分下。

## 盗版

互联网上版权材料的盗版是所有媒体持续存在的问题。在 Packt，我们非常重视版权和许可的保护。如果您在互联网上发现我们作品的任何形式的非法复制品，请立即提供位置地址或网站名称，以便我们采取补救措施。请通过 `copyright@packtpub.com` 联系我们，并附上疑似盗版材料的链接。我们感谢您帮助保护我们的作者以及我们为您提供有价值内容的能力。

## 问题

如果您对本书的任何方面有问题，可以通过 `questions@packtpub.com` 联系我们，我们会尽力解决问题。
