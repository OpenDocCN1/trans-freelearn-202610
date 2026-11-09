# Microsoft BizTalk 2010：业务系统集成：BizTalk Server 2010 业务系统集成实践指南

## 致谢

### 作者
- Kent Weare
- Richard Seroter
- Sergei Moukhovitski
- Thiago Almeida
- Carl Darski

### 审阅者
- Mick Badran
- Stephen W. Thomas
- Ben Cline

### 收购编辑
Kerry George

### 开发编辑
Alina Lewis

### 技术编辑
Ajay Shanker

### 项目协调员
Zainab Bagasrawala

### 校对
Aaron Nash

### 索引
Monica Ajmera

### 图形
Geetanjali Sawant

### 生产协调员
Alwin Roy

### 封面工作
Alwin Roy

## 前言

我们知道，如今在 IT 部门中，解决方案无需与其他系统甚至外部实体集成的情况很少见。系统必须互联，以支持企业所要求的复杂业务流程和自动化水平。其中一些系统确实是业务的核心。它们可以支持财务、人力资源和客户关系等运营功能。它们也可能是关键任务；如果停机一段时间，业务将受到重大影响。

我们知道，连接系统很复杂，有很多选择、技术和产品发挥作用。开发人员经常自行构建集成解决方案和支持基础设施。集成问题的“最后一公里”是最困难的之一——你如何实际在两个系统之间交换数据和消息——如何将它们连接起来？

行业整体一直在关注的承诺是 Web 服务和面向服务架构（SOA）的世界。我们永远无法完全达到这个世界——有太多系统、标准和定制的解决方案没有可调用的“服务”。但如果有一个解决方案，甚至一个开发框架，能够在一端连接到这些系统，另一端将功能作为“服务”公开呢？我们称之为“适配器”。如果您可以将这些适配器连接到您的集成平台甚至您自己的自定义代码中呢？如果您可以轻松编写自己的“适配器”呢？

本书的全部内容都是关于如何利用微软的集成平台——BizTalk Server，以及如何使用随 BizTalk Server 提供的各种“适配器”与您的业务系统集成。

云也正在迅速成为这幅图景的重要组成部分，用于与云中运行的系统集成，以及在云中创建流程和移动数据。本书涵盖了为实现这一目标而创建的云平台，以及今天如何实际做到这一点。

> 本书对于希望了解技术以及连接系统在一起的实际机制的开发人员来说是一个极好的资源。

Tony Meleg

*高级技术产品经理*
*微软公司*

## 关于作者

**Kent Weare** 出生于加拿大萨斯喀彻温省里贾纳市，对冰球、足球和技术产生了热爱。他就读于里贾纳大学，获得计算机科学学位。完成本科学习后，他在印度学习，获得面向对象技术研究生文凭。他目前居住在加拿大阿尔伯塔省卡尔加里，但仍然是萨斯喀彻温粗骑兵队足球的铁杆粉丝。

Kent 的职业生涯始于一家小型互联网初创公司，随后在萨斯喀彻温省政府担任初级职位。此后他参与了加拿大联邦政府、美国一家跨国银行、加拿大东部和西部医疗保健项目的项目，并在卡尔加里一家大型电力配电公司工作了过去五年。Kent 目前的职责包括管理一个支持 BizTalk、Exchange、Office Communication Server (OCS) 和 System Center 的 Microsoft 解决方案团队。

在 Kent 在联邦政府工作期间，他有机会参与他的第一个 BizTalk 项目。七年后，他仍然对 BizTalk“上瘾”，自那以来他使用过每个发布的 BizTalk 版本。2008 年，Kent 因 BizTalk Server 获得了他的第一个微软 MVP 奖。他继续活跃于 BizTalk 社区，并最近获得了第四个连续的 MVP 奖。Kent 维护着一个活跃的博客 http://kentweare.blogspot.com，并可能在本地和国际用户组中发表与 BizTalk 相关的材料。

我首先要感谢我的父母（Keith 和 Joyce）在成长过程中给予我无条件的爱、支持和指导。他们是努力工作、尊重他人并教导我是非对错的坚定倡导者。

感谢我的双胞胎兄弟（Kurt）、姐姐（Kim）及其家人对本书的兴趣，以及鼓励我坚持不懈直到完成。

在我的职业生涯中，许多人看到了我自己未必看到的东西。我确信如果没有这种支持，我永远不会有机会进入 MVP 项目或撰写这本书。特别感谢 Les Phillips、Dave Patel、Ron Naidu、Vasu Iyengar、Lucie Duval、Neal Nishikawa、Darren Jeffrey、Brian Dempsey 和 Alan Skiffington 给予我机会并提供成功所需的工具。我还要感谢我的导师 Nipa Chakravarti 和 Karl Smith 的见解，并挑战我作为领导者成长。

最后，如果没有我深爱的妻子 Melissa 和女儿 Brooke 的支持，这本书就不可能成为现实。写书是一项巨大的工作，占用了深夜、清晨以及几乎所有空闲时间。谢谢 Melissa 的耐心、支持，以及在我工作时容忍我长时间坐在电脑前。Brooke，谢谢你写作时没有太多次按蓝色的（电源）按钮。有时你真的把 Microsoft Word 的自动恢复功能推到了极限。

**Richard Seroter** 是行业领先的生物技术公司的解决方案架构师、BizTalk Server 的微软 MVP 以及微软互联技术顾问。他的大部分职业生涯都花在为客户提供咨询，帮助他们规划和实施企业软件解决方案。Richard 的职业生涯始于两家全球 IT 咨询公司，这使他接触到各种各样的行业、技术和业务挑战。然后，他加入微软担任 SOA/BPM 技术专家，其唯一目标是教育和与客户合作，在他们考虑、设计和构建 BizTalk 解决方案时提供帮助。其中一位客户足够喜欢 Richard，在他们承诺使用 BizTalk Server 作为企业服务总线后，将他全职聘为架构师。一旦 BizTalk 环境成功建立，Richard 转型为解决方案架构师，现在帮助识别企业最佳实践并将良好的架构原则应用于广泛的 IT 计划。

Richard 是两本书的作者，包括 *Applied Architecture Patterns on the Microsoft Platform* (Packt Publishing, 2010)，讨论在何处使用微软平台技术的哪个部分。他也是 *SOA Patterns for BizTalk Server 2009* (Packt Publishing, 2009) 的作者，探讨如何将良好的 SOA 原则应用于各种 BizTalk 场景。

> 我想感谢 Kent 将我带入这个有趣的项目，并允许我贡献一些章节。我对企业集成有着病态的热情，Kent 为我提供了深入研究该主题的完美途径。
>
> 感谢所有不可避免地影响了我的思维并使我成为更好架构师和技术人员的同事。我的经理 Nancy Lehrer 在我坎坷的走向超级明星的道路上继续成为一位出色的导师。最后，感谢我的家人和我的孩子们（人类 Noah，狗 Watson），他们忍受了我的深夜工作和心不在焉。



**Sergei Moukh... 是一名拥有15年软件和系统开发经验的软件架构师。他的专业兴趣领域是应用于 ERP 和 CRM 系统（如 Microsoft Dynamics 和 SAP）的业务流程和集成技术。**

**Thiago Almeida** 是新西兰最大 IT 服务提供商之一的高级集成顾问。他拥有超过八年的经验，曾担任解决方案架构师和高级开发人员，参与该国一些最大公司的项目。

Thiago 于 2009 年和 2010 年获得微软 BizTalk 最有价值专业人员称号，以表彰他对 BizTalk Server 技术社区的卓越贡献。他运营奥克兰互联系统用户组，经常担任演讲者，并在 `http://connectedthoughts.wordpress.com` 维护关于 BizTalk Server 的博客

他同时也是微软互联技术顾问和微软集成领域的虚拟技术解决方案专家，在与微软的客户项目中提供 BizTalk Server 及相关集成技术的先进技术支持。

我要感谢我的团队成员一起完成了许多优秀的项目，并与我分享真实世界的 BizTalk Server 知识。我还要感谢微软员工、同事 MVP 以及合著者们提供的所有意见和帮助。在更个人层面上，我要感谢我美丽的妻子 Karla 的鼓励，以及她忍受我为本书工作而熬夜和占用周末。特意不在此致谢中提及新西兰在这些周末期间的壮丽景色、美丽海滩和宜人天气。

**Carl Darski** 是一名软件顾问，在电信、石油天然气、电力和采矿等行业拥有多元化的专业经验。作为一名电气工程师，Carl 很快认识到软件集成技术日益增长的重要性。他曾协助多家客户将 BizTalk 实施为集成标准。

> 我要感谢 Ideaca 的所有同事在过去几年里在技术和职业上给予我的帮助。特别感谢 Shane James，他的 BizTalk 知识从未停止让我惊叹，也从未让我失望。

## 关于审阅者

**Mick Badran** 多年来一直从事软件集成工作，从架构/设计到实施全程参与。在过去 10 年里，他的专业知识集中在 Microsoft Integration Stack，涵盖 BizTalk、SharePoint、WCF、WF 等产品以及许多相关技术。

微软已将 Mick 评为 MVP，他代表微软担任 BizTalk 虚拟技术专家（V-TSP）。

Mick 最初的热情是教授微软技术课程分享他的知识。随着时间的推移，Mick 仍在继续，同时也开展服务项目，参与中大型项目。

当 Mick 不在“极客群”中时，他喜欢武术、网球，以及在当地海滩担任救生员。浮潜和潜水在他的喜好清单上也排名相当靠前！

**Stephen W. Thomas** 是一名独立顾问，专长于 BizTalk Server 以及包括 Workflow 和 AppFabric 在内的其他 Microsoft 服务器技术。他从事 BizTalk 工作已有十多年。在过去七年里，Stephen 被微软评为 BizTalk Server 领域的 MVP。

Stephen 为众多客户提供过咨询服务，其中包括许多财富 500 强企业。Stephen 运营着 BizTalk 社区网站 http://www.BizTalkGurus.com。该网站提供社区论坛、示例、各种操作视频以及 Stephen 的博客。Stephen 曾在多场微软 TechEd 大会、多个 SOA 大会以及各种用户组中发表演讲。Stephen 是 Packt Publishing 出版的书籍 *在 Microsoft 应用平台上应用架构模式* 的合著者。

**Ben Cline** 目前在 Paylocity 担任 BizTalk 架构师。他是 MSDN 论坛的活跃贡献者和版主，也是 BizTalk MVP 计划的成员。除了在论坛中担任思想领袖外，他还维护着一个关于 BizTalk 及其他微软技术技巧、窍门和变通方法的博客。业余时间，他喜欢在微软产品的预发布版本中查找并报告错误。

Ben 积极参与 BizTalk 开发项目咨询工作，并为大型国际客户提供管理和支持项目。他还经常参与微软学习项目的开发。

我要感谢我的家人帮助我每天挤出更多高效的工作时间。

## www.PacktPub.com

### 支持文件、电子书、折扣优惠等

您可能希望访问 www.PacktPub.com 获取与本书相关的支持文件和下载内容。

您知道 Packt 为出版的每本书都提供电子书版本，支持 PDF 和 ePub 文件吗？您可以在 www.PacktPub.com 升级到电子书版本，作为印刷版书籍的客户，您有权享受电子书的折扣。欲了解更多详情，请联系 service@packtpub.com。

在 www.PacktPub.com，您还可以阅读大量免费技术文章，订阅各类免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/23d1402649b6528b085cd6b900a2c9c3_13_0.png)

http://PacktLib.PacktPub.com

您是否需要即时解决 IT 问题？PacktLib 是 Packt 的在线数字图书库。在这里，您可以访问、阅读和搜索 Packt 整个图书库。

### 为什么订阅？

- 可在 Packt 出版的每本书中完全搜索
- 复制粘贴、打印和书签内容
- 按需访问并可通过网页浏览器访问

### Packt 账户持有者免费访问

如果您在 www.PacktPub.com 拥有 Packt 账户，您可以使用它立即访问 PacktLib 并查看九本完全免费的书籍。只需使用您的登录凭据即可立即访问。

### 新书即时更新

获取通知！关注 Twitter 上的 @PacktEnterprise 或 Packt Enterprise Facebook 页面，了解新书出版信息。

## 前言

Microsoft BizTalk 是一种集成服务器解决方案，允许企业连接不同的系统。在当今并购频繁的商业环境中，越来越多的企业被迫使用 BizTalk Server 2010 等集成代理在业务系统之间交换数据。在集成这些系统时经常被忽视的是，为了有效集成它们，ERP 和 CRM 系统所要求的先决知识。这些知识不再被锁在昂贵顾问的脑海中。通过本书培养业务系统集成方面的宝贵技能，在组织内获得竞争优势。

本书将通过实际场景向您展示如何将 BizTalk 与业务系统集成。每章将介绍一个业务系统，介绍一些先决知识并演示如何将 BizTalk 与该业务系统集成，然后基于实际经验提供指导，进一步提升您的 BizTalk 知识。

本书将使您掌握如何将 BizTalk 与业务系统集成。本书首先强调 WCF-LOB 适配器的技术基础以及与流行 WCF-LOB 适配器相关的常见步骤和重要属性。然后您将了解如何使用基于 WCF 的 SQL Server 适配器与 Microsoft SQL Server 集成的概述。本书随后深入探讨诸如与 Dynamics CRM 集成、使用 IDOC 构建 BizTalk/SAP 集成解决方案、IDOC 与 RFC/BAPI 之间的差异，以及通过 Windows Azure AppFabric Service Bus 进行 WCF 集成等主题。



### 本书涵盖内容

- 第 1 章，使用基于 ASDK 的适配器：探索 WCF LOB SDK 和 WCF 自定义适配器的一些内部工作原理。
- 第 2 章，WCF-SQL 适配器：学习如何使用 WCF-SQL 适配器公开的流行操作（包括轮询、通知和复合操作）检索和操作数据。
- 第 3 章，集成 BizTalk Server 和 Microsoft Dynamics CRM：发现与 Dynamics CRM 2011 和 BizTalk Server 集成的不同方式，包括调用原生 Web 服务和代理解决方案。还学习如何通过 CRM 注册的插件调用 BizTalk 暴露的 WCF 服务。
- 第 4 章，WCF SAP 适配器发送和接收 IDOC：了解如何安装 WCF-SAP 适配器的先决条件 DLL。了解扩展的、定制的和开箱即用的 IDOC 以及如何发送和接收它们。
- 第 5 章，WCF SAP 适配器 RFC 和 BAPI：区分 SAP IDOC、BAPI 和 RFC 之间的差异以及何时使用它们。
- 第 6 章，BizTalk 与 Windows Azure AppFabric 服务总线的集成：发现 Microsoft 的 AppFabric 服务总线并学习如何构建与 Windows Azure 云中 Microsoft 服务总线互补的 BizTalk 解决方案。
- 第 7 章，集成 SharePoint 2010：使用 Windows SharePoint Services 适配器和 InfoPath 构建集成的 SharePoint 解决方案。
- 第 8 章，集成 SharePoint 2010 Web 服务：通过使用 SharePoint 的开箱即用列表 Web 服务来学习操作 SharePoint 自定义列表。
- 第 9 章，Microsoft Dynamics AX：了解如何使用 BizTalk 适配器和 .Net 业务连接器集成 Microsoft Dynamics AX 2009。
- 第 10 章，集成 BizTalk Server 和 Salesforce.com：了解如何建立 SalesForce.com CRM 与本地服务之间的双向连接。

### 本书适合谁

如果您是一名经验丰富的 BizTalk 开发人员，希望使用实际场景将 BizTalk 与业务系统集成，那么本书适合您。需要对 BizTalk 有扎实的 intermediate 中级理解。本书假定开发人员能够在 Visual Studio 中创建架构、映射、编排、端口和消息，并在 BizTalk 管理控制台中配置应用程序。然而，集成业务系统的经验并非绝对必要。

### 约定

在本书中，您会找到多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义的解释。

Code words in text are shown as follows: "The AfterReceiveRequest operation can now validate the token through the helper function."

A block of code is set as follows:

```
private string serviceNamespace;
private string trustedTokenPolicyKey;
private string acsHostName;
private string trustedAudience;
```

Any command-line input or output is written as follows:

```
SAPCAR: processing archive RFC_25-20001765.SAR (version 2.01)
```

New terms and important words are shown in bold. Words that you see on the screen, in menus or dialog boxes for example, appear in the text like this: "Accept the license terms and click Next".

![](img/23d1402649b6528b085cd6b900a2c9c3_23_0.png)

Warnings or important notes appear in a box like this.

![](img/23d1402649b6528b085cd6b900a2c9c3_23_1.png)

Tips and tricks appear like this.

### 读者反馈

我们始终欢迎读者的反馈。请告诉我们您对本书的看法——您喜欢什么或可能不喜欢什么。读者的反馈对我们开发您真正受益的书名非常重要。

To send us general feedback, simply send an e-mail to `feedback@packtpub.com`, and mention the book title via the subject of your message.

If there is a book that you need and would like to see us publish, please send us a note in the **SUGGEST A TITLE** form on `www.packtpub.com` or e-mail `suggest@packtpub.com`.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide on `www.packtpub.com/authors`.

### 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供多项帮助，让您充分利用您的购买。

### 下载示例代码

您可以在 `http://www.PacktPub.com` 的账户中下载您购买的所有 Packt 书籍的示例代码文件。如果您在其他地方购买了本书，您可以访问 `http://www.PacktPub.com/support` 并注册以直接通过电子邮件接收文件。

### 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果您在我们的某本书中发现错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。这样做可以让其他读者免于沮丧，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 `http://www.packtpub.com/support`，选择您的书籍，单击 **errata submission form** 链接并输入勘误详情。一旦您的勘误得到验证，您的提交将被接受，勘误将被上传到我们的网站，或添加到该标题勘误部分下现有勘误列表中。任何现有勘误都可以通过从 `http://www.packtpub.com/support` 选择您的标题来查看。

### 盗版

互联网上版权材料的盗版是所有媒体持续存在的问题。在 Packt，我们非常重视对版权和许可的保护。如果您在互联网上发现我们作品的任何非法副本，无论形式如何，请立即提供位置地址或网站名称，以便我们采取补救措施。

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and our ability to bring you valuable content.

### 问题

如果您在本书的任何方面遇到问题，可以联系我们 questions@packtpub.com，我们将尽力解决。
