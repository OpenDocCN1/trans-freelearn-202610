## 致谢

**作者**

Anita Graser, Ben Mearns, Alex Mandel, Víctor Olaya Ferrero, Alexander Bruy

**内容开发编辑**

Aishwarya Pandere

**制作协调员**

Aparna Bhagat

**审阅者**

Cornelius Roth, Ujaval Gandhi, Fred Gibbs, Gergely Padányi-Gulyás, Abdelghaffar KHORCHANI, Pablo Pardo, Mats Töpel, Jorge Arévalo, Olivier Dalang, Ben Mearns

## 前言

QGIS 是自由开源地理空间世界的交汇点。虽然 QGIS 中有大量的工具，但它并不是一个做所有事情的庞大应用，从一开始它也从未被设计成那样。它更像是开源地理空间世界的视觉接口。你可以从专有和开放格式加载数据到各种风格的空间数据库中，然后使用众所周知的分析后端分析数据，最后创建印刷或基于网络的地图来显示和与你的结果交互。QGIS 在其中扮演什么角色？它是在你沿途检查数据、构建并排队分析、可视化结果以及开发制图最终产品的场所。

本学习路径将以动手实践的学习方式教你所有这些内容以及更多。凭借这本实用的伴侣，成为 QGIS 专家。

### 本学习路径涵盖的内容

*Module 1, Learning QGIS, Third edition*, 涵盖了创建出色地图所需的重要功能。然后，我们将通过标注点位置的示例以及创建带有道路盾牌图形的更详细道路标签来讲解标注。我们还将讲解如何手动调整标签。我们将了解打印布局以及如何使用它来创建可打印地图和地图集。最后，我们将讲解在网络上展示地图的解决方案。

*Module 2, QGIS Blueprints*, 将演示可视化和分析技术，以探索地点与时间之间的关系以及地点之间的关系。你将使用人口普查的选民数据，通过时间轴控制的动画进行工作。

*Module 3, QGIS 2 Cookbook*, 涉及将数据转换为分析所需的格式，包括矢量与栅格之间的转换、不同类型矢量之间的转换，以及将数据裁剪到仅重要的区域。它还展示了你如何通过插件、定制和附加工具将 QGIS 扩展到开箱即用的功能之外。

### 本学习路径所需

**Module 1**

要跟随本书中的练习，你需要 QGIS 2.14。QGIS 安装在第一章中介绍，练习数据的下载链接在相应章节中提供。

**Module 2**

你需要：

- QGIS 2.10
- 运行 OS X、Windows 或 Linux 的计算机

**Module 3**

我们建议安装 QGIS 2.8 或更高版本；你至少需要 QGIS 2.4。在本书编写期间，大约每 4 个月发布了几个新版本，最近发布的是 2.14。大多数食谱可在旧版本上运行，但有些可能需要 2.6 或更新版本。总的来说，如果可以，请升级到最新稳定版或长期支持版 LTS。

在这些食谱中还有很多与其它软件的交互，包括但不限于 PostGIS 2+、GRASS 6.4+、SAGA 2.0.8+ 和 SpatiaLite 4+。在 Windows 上，大多数这些都可以使用 OSGeo4W 安装；在 Mac 上，你可能需要来自 KyngChaos 的一些额外框架，或者如果你熟悉 Brew，可以使用 OSGeo4Mac Tap。对于 Linux 用户，特别是 Ubuntu 和 Debian，请参考 UbuntuGIS PPA 和 DebianGIS 混合包。

所有这些听起来有点复杂吗？如果是，那么请考虑使用运行 OSGeo-Live 的虚拟机（http://live.osgeo.org）。所有软件都已为你预装，并且已知可以协同工作。

最后，你需要数据。大多数情况下，我们提供了来自各种来源的大量免费和开放数据，包括 OSGeo 教育数据集（北卡罗来纳州）、Natural Earth Data、OpenFlights、Wake County、City of Davis 和武装冲突位置与事件数据项目（ACLED）。如果你想要更多数据，这里提供了我们的数据源完整列表。

我们建议你先用示例数据尝试方法，因为我们已经测试过它。

请随意使用自己的数据测试许多食谱；不过，请记住，你可能需要更改结构才能使其工作。毕竟，这就是你通常会处理的内容。

本书的数据来源如下：

- OSGeo 教育数据：http://grass.osgeo.org/download/sample-data/
- 美国 Wake County：http://www.wakegov.com/gis/services/pages/data.aspx
- Natural Earth Data：http://www.naturalearthdata.com/
- 美国 Davis 市：http://maps.cityofdavis.org/library
- Stamen Designs：http://stamen.com/
- 武装冲突位置与事件数据项目：http://www.acleddata.com/

### 本学习路径适合谁

如果你是用户、开发人员或顾问，想知道如何使用 QGIS 实现你习惯于其它类型 GIS 的结果，那么本学习路径适合你。预计你对核心 GIS 概念感到舒适。本学习路径将通过向你展示如何开发更复杂、分层的地图应用，让你成为 QGIS 专家。它将把你提升到 GIS 用户的下一个层次。

### 读者反馈

我们非常欢迎读者的反馈。请告诉我们你对本课程的看法——你喜欢或不喜欢什么。读者的反馈对我们很重要，因为它帮助我们开发出你真正能从中获益的书目。

如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中提及课程标题即可。

如果你在某个主题有专业知识，并且有兴趣撰写或为课程做出贡献，请查看我们的作者指南 www.packtpub.com/authors。

### 客户支持

现在你已成为 Packt 课程的自豪拥有者，我们有许多方式帮助你充分利用你的购买。

**下载示例代码**

你可以在 http://www.packtpub.com 上从你的账户下载本课程的示例代码文件。如果你是在其它地方购买本课程，你可以访问 http://www.packtpub.com/support 并注册以便直接将文件发送到你的电子邮件。

你可以通过以下步骤下载代码文件：

- 1. 使用你的电子邮件地址和密码登录或注册我们的网站。
- 2. 将鼠标指针悬停在顶部 **SUPPORT** 选项卡上。
- 3. 点击 **Code Downloads & Errata**。
- 4. 在 **Search** 框中输入课程名称。
- 5. 选择你想要下载代码文件的课程。
- 6. 从下拉菜单中选择你购买本课程的渠道。
- 7. 点击 **Code Download**。

你也可以通过在 Packt Publishing 网站的课程网页上点击 **Code Files** 按钮来下载代码文件。此页面可通过在 **Search** 框中输入课程名称访问。请注意，你需要登录你的 Packt 账户。

下载文件后，请确保使用最新版本的以下工具解压或提取文件夹：

- WinRAR / 7-Zip for Windows
- Zipeg / iZip / UnRarX for Mac
- 7-Zip / PeaZip for Linux

本课程的代码包也托管在 GitHub 上：https://github.com/PacktPublishing/QGIS-Becoming-a-GIS-Power-User。

我们还有来自我们丰富的图书、视频和课程目录的其它代码包，可在 https://github.com/PacktPublishing/ 上找到。快来看看吧！





## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍可能发生。如果您在我们的图书中发现错误——可能是文本或代码中的错误——我们将非常感激您能向我们报告。通过这样做，您可以帮助其他读者避免挫折，并帮助我们改进本课程的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/submit-errata， 选择您的课程，点击勘误提交表单链接，并输入勘误的详细信息。一旦您的勘误得到验证，您的提交将被接受，勘误将被上传到我们的网站或添加到该标题勘误部分下现有勘误列表中。

要查看之前提交的勘误，请前往 https://www.packtpub.com/books/content/support 并在搜索字段中输入图书名称。所需信息将出现在勘误部分下。

## 盗版

互联网上版权材料的盗版是一个持续存在于所有媒体中的问题。在 Packt，我们非常重视对版权和许可的保护。如果您在互联网上发现我们作品的任何形式的非法复制品，请立即提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并附上疑似盗版材料的链接。

我们感谢您帮助保护我们的作者以及我们为您提供有价值内容的能力。

## 问题

如果您对本课程的任何方面有问题，您可以联系我们 questions@packtpub.com，我们会尽力解决该问题。
