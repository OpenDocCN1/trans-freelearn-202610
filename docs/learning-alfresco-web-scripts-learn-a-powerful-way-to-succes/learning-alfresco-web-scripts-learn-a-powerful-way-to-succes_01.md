# Alfresco web scripts 入门指南：学习使用Alfresco成功实现独特集成解决方案的强大方法

## 关于作者

**Ramesh Chauhan** 目前在 CIGNEX Datamatics 担任首席顾问。他拥有约 9 年的核心 IT 经验。他在 Alfresco 方面拥有深厚的专业技能，为全球客户在 Alfresco 中实施并交付了定制化业务解决方案，并在多个生产项目中广泛使用 Alfresco。他毕业于印度古吉拉特大学 Nirma 理工学院，获得信息技术工程学士学位。他还为 Alfresco 社区论坛做出贡献。

## 致谢

我首先要感谢斯瓦米纳拉扬主和我的上师 H. H. Pramukh Swami Maharaj 的所有祝福。我要感谢我的父母给予我无条件的爱和支持。特别感谢我挚爱体贴的妻子 Priyanka Chauhan，她在本书写作过程中始终鼓励和支持我。非常感谢我一岁的女儿 Misri，她一直是我灵感的源泉。

我要感谢 Packt Publishing 给我这个机会。我要感谢所有技术评审人员提出的宝贵反馈，并感谢 Packt Publishing 团队的 Kevin Colaco、Priyanka Shah 和 Nikhil Potdukhe。与整个 Packt Publishing 团队合作真的非常愉快。我衷心感谢内容开发编辑 Priyanka Shah 和技术编辑 Nikhil Potdukhe，他们用宝贵的反馈帮助我修正了写作风格。

我要感谢 CIGNEX Datamatics 的管理团队，鼓励我撰写本书，作为我们组织社区贡献的一部分。衷心感谢 Munwar Shariff 先生（CIGNEX Datamatics CTO）一直以来的支持。我也要感谢 CIGNEX Datamatics 的整个 Alfresco 实践团队，他们非常支持我。感谢 Aadit Majmudar 在本书写作过程中随时提供帮助。我要特别感谢 Dhaval Joshi，他是我在 CIGNEX Datamatics 的非常好的朋友和同事。每当我需要他的帮助和建议时，他总是陪在我身边。感谢他和我的朋友 Chirayu Joshi，他们相信我、支持我并激励我写这本书。

## 关于评审人员

**Snig Bhaumik** 是 InfoAxon Technologies Limited（www.infoaxon.com）的技术总监，负责设计由开源技术支持的内容和知识管理解决方案。他在开源技术（如 Alfresco、Liferay、Pentaho 和 Microsoft .NET）方面拥有超过 13 年的丰富经验。

Snig 的专业领域包括知识管理、协作式内容管理、业务流程自动化、社交网络、商业智能、搜索和新兴技术。他在许多项目中担任高级顾问，与高层利益相关者一起推动 BI、KM 和 ECM 议程。他是这些领域广受欢迎的演讲者，并定期以白皮书和博客的形式撰写文章。他的作品可以在 http://blog.infoaxon.com/、http://onalfresco.blogspot.in/ 和 https://github.com/SnigBhaumik/ 上查看。

他在向全球客户提供开源驱动的解决方案方面拥有丰富的经验，涉及国际发展、社会住房、建筑、银行、零售和保险等领域。他是各种开源社区的活跃参与者，并为一些领先的开源平台贡献了功能。

作为开源爱好者，Snig 是多个开源社区的活跃贡献者：

- 他撰写了 Packt Publishing 出版的 *Alfresco Cookbook 3*（http://www.amazon.com/Alfresco-3-Cookbook-Snig-Bhaumik/dp/184951108X）
- 他领导了 Alfresco 和 InfoAxon 市场上 Alfresco 插件路线图的开发和管理（http://www.infoaxon.com/solutions/addons/#all）
- 他在 Alfresco 上发布的主要插件包括：
  - Bootfresco (https://github.com/SnigBhaumik/Bootfresco)
  - Calendar Event Participant (https://addons.alfresco.com/addons/calendar-event-participant)

**Bernd Krumböck** 是一名自雇 IT 专业人士。在过去 7 年里，他为 NGO/NPO Volkshilfe NÖ（一家在下奥地利州提供护理和其他服务、拥有约 1,000 名员工的 NPO）实施并支持了许多项目。其中一个项目是基于 Alfresco 的文档管理系统，用于 ISO 标准化流程。因此，他成为了 Alfresco Community 扩展的 Oracle Server 支持的新维护者。

除此之外，他还致力于各种主要基于 Java/J2EE 的业务应用程序。此外，他的技能也用于服务器、数据库和网络管理。

几年前，他在奥地利 REWE Group 担任系统管理员。他的主要任务是管理 HP-UX、Linux 和 Oracle 数据库系统。在此期间，他为 CUPS 和其他项目贡献了各种代码。

在空闲时间，他从事各种扩展智能家居系统的小项目。其中一个项目为来自 8 devices 的 USB-CAN 适配器编写了一个 Linux 内核驱动程序（https://github.com/krumboeck/usb2can）。

**Piergiorgio Lucidi** 是开源企业内容管理（ECM）专家，也是 Sourcesense 的认证 Alfresco 培训师。Sourcesense 是一家欧洲开源系统集成商，围绕关键开源技术提供咨询、支持和服务。

他担任导师、技术负责人和软件工程师，在 ECM 和系统集成领域拥有 10 年经验。他是将 ECM 解决方案集成到 Web 和门户应用程序方面的专家。

他作为 Alfresco Wiki Gardener 定期为 Alfresco 社区做出贡献。在 2012 年柏林 Alfresco DevCon 期间，他被评为 Alfresco Community Star。

他为 Apache 软件基金会做出贡献，担任 Apache ManifoldCF 的 PMC 成员和提交者，并且是 CMIS、Alfresco 和 Elasticsearch 连接器的项目负责人。他是 JBoss 社区的项目负责人和提交者。他还为 JBoss Portal 平台的一些项目做出贡献。

他是致力于 Java、Spring Framework、开源产品以及与 ECM 和 WCM 世界相关技术的各种会议的演讲者。

他是 Packt Publishing 的作者、技术评审人员和联盟合作伙伴。在 Packt Publishing，他与 Ugo Cei 合著了 *Alfresco 3 Web Services*，与 Ken Finnigan 和 Luca Stancapiano 合著了 *GateIn Cookbook*。作为技术评审人员，他还为 *Alfresco 3 Cookbook*、*Alfresco Share* 和 *Alfresco 4 Enterprise Content Management Implementation* 等书籍做出了贡献，这些书籍均由 Packt Publishing 出版。

作为联盟合作伙伴，他还在自己的网站 Open4Dev（http://www.open4dev.com/）上撰写和发布书评。

> 我要感谢 Packt Publishing 又一次提供了为致力于 Alfresco 平台的项目做出贡献的好机会。





## 支持文件、电子书、折扣优惠等

您可能想访问 www.PacktPub.com 获取与您的书籍相关的支持文件和下载。

您知道吗？Packt为每本已出版的书提供电子书版本，包括PDF和ePub格式？您可以在 www.PacktPub.com 升级到电子书版本，作为印刷书客户，您有权获得电子书副本的折扣。如需更多详情，请通过 service@packtpub.com 与我们联系。

在 www.PacktPub.com，您还可以阅读一系列免费技术文章，订阅各种免费通讯，并获得Packt书籍和电子书的独家折扣和优惠。

![](img/f02f759059b86af67e38b543d498a0d8_10_0.png)

http://PacktLib.PacktPub.com

您是否需要IT问题的即时解决方案？PacktLib是Packt的在线数字图书库。在这里，您可以访问、阅读和搜索Packt的全部图书库。

### 为什么要订阅？

-   可在Packt出版的每一本书中进行全文搜索
-   可复制、粘贴、打印和书签内容
-   按需访问，可通过网页浏览器访问

### Packt账户持有人免费访问

如果您在 www.PacktPub.com 拥有Packt账户，今天就可以使用该账户访问PacktLib，并查看九本完全免费的书籍。只需使用您的登录凭据即可立即访问。

## 前言

当我获得写作这本书的机会时，我脑海中浮现的第一个想法是我能否胜任这项工作，因为在项目中实现Web脚本与撰写一本关于Web脚本的书是完全不同的。然而，与此同时，我也对写自己的第一本书感到好奇和兴奋。我一直相信——如果你认为你能做到，那就大胆去做，并尽最大努力去实现它。于是，我抓住了这个机会，开始了写第一本书的新旅程，与你们分享我的知识、学习和经验。

简要介绍本书：Web脚本是Alfresco的关键特性之一，也是每一位从事Alfresco开发的开发者的必备知识。对于从事Alfresco开发的开发者来说，熟悉强大的Web脚本框架并理解如何实际实现Web脚本至关重要。在本书中，您将学习在Alfresco中使用Web脚本所需的所有基本细节，并使自己熟练掌握Web脚本开发，以便在项目中实现与Alfresco的有用集成解决方案。

几年前，我也是个初学者。现在，我在不同的实际业务实施中拥有了大量使用Web脚本的经验。基于我从初学者到目前关于实现Web脚本的专业水平的旅程中的学习和经验，我试图提供最有用的信息，以帮助那些期待探索和学习Web脚本的新手或有经验的开发者。

我试图分享我在处理Alfresco Web脚本过程中获得的知识和学习。我试图以一种简单的方式解释Web脚本的关键概念，并通过实际示例来理解如何实现Web脚本。我也深入到了我任务中需要的技术细节，以帮助您更好地了解Web脚本框架。

在本书结束时，您将能够说您已经熟悉了Web脚本，并准备好承担新任务来开发自定义Web脚本，因为您已经获得了在Alfresco中实际实现它们所需的知识。您还将能够调试、排除故障并修复在使用Web脚本时可能遇到的问题。此外，您还能与同事分享有关强大Web脚本框架的核心技术知识。

我写这本书的旅程已经到达终点，因为这本书现在已经在你手中。现在，你即将开始阅读这本书并学习Alfresco Web脚本的旅程。祝你在前方有一段美妙的旅程！

## 本书涵盖内容

第1章“熟悉Web脚本”通过探索关于Web脚本的一些基本问题的答案，向您介绍Web脚本，这将让您清楚地了解Web脚本如何有助于实现与Alfresco的集成解决方案。

第2章“是时候编写第一个Web脚本了”将引导您在Alfresco中实现第一个Web脚本，这将为您提供关于核心基础知识的知识，以及在项目中实际实现Web脚本时需要注意的一些关键点。

第3章“理解Web脚本框架”介绍了强大的Web脚本框架，它将使您充分了解Web脚本执行的详细技术知识以及Web脚本框架的支柱。

第4章“Web脚本的构建模块”为您提供了构建Web脚本的所有组件的完整细节。

第5章“调用Web脚本”描述了您可以执行Web脚本的各种方式，这将帮助您根据需要选择执行Web脚本的方式。

第6章“创建Java后端的Web脚本”通过一个示例用例的实现，帮助您理解如何实际实现Java后端的Web脚本。

第7章“深入理解基于JavaScript的Web脚本”解释了您可以在Alfresco Web脚本中使用JavaScript API做什么，并让您了解用于访问这些API的不同根对象以及Web脚本框架公开的有用根对象。它还带您浏览了一些基于JavaScript的Web脚本中常见功能的一些代码示例。




*第8章：Web 脚本的部署、调试与故障排查*，详细指导你了解部署 Web 脚本的选项，并介绍一些有用的调试技巧和故障排查指引，以及在生产环境中执行 Web 脚本的一些重要要点。

*第9章：Web 脚本的 Maven 化*，介绍使用 Alfresco Maven SDK 在 Eclipse 中搭建开发环境以开发 Web 脚本的基础知识。

*第10章：扩展 Web 脚本框架*，让你熟悉在 Alfresco 中扩展 Web 脚本框架核心组件的可能性，帮助你理解在开源技术中工作的强大之处。

## 阅读本书需要什么

你需要有一个可正常运行的 Alfresco 实例，它将帮助你完成本书的整个学习过程。你应该使用 Alfresco 4.2 或更高版本。

## 本书适合谁

如果你是 Alfresco 新手开发者，正在探索 Alfresco 中的 Web 脚本，那么本书适合你。本书将帮助你掌握在 Alfresco 中实现 Web 脚本所需的关键基础知识。如果你是有经验的 Alfresco 开发者，熟悉 Web 脚本开发的基础知识，但尚未深入探索 Web 脚本框架，那么本书也适合你，因为它将帮助你获得有关该框架的必备知识。如果你已经深入探索过 Alfresco 中的 Web 脚本，并且对它有很好的理解，那么你会发现本书作为快速、方便的参考手册非常有用。

## 排版约定

本书使用多种文本样式来区分不同类型的信息。以下是一些样式示例及其含义说明。

正文中的代码词语、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 用户名采用如下格式显示：

“DeclarativeWebScript 设置要渲染的响应。”

代码块如下所示：

```
private RestTemplate restTemplate;
public void setRestTemplate(RestTemplate restTemplate) {
    this.restTemplate = restTemplate;
}
```

当我们希望您注意代码块的某个特定部分时，相关行或项目会以粗体显示：

```
<html>
    <body>
        <p>Hello! ${args.name}.</p>
    </body>
</html>
```

任何命令行输入或输出都按如下方式编写：

```
curl -u admin:admin "http://localhost:8080/alfresco/service/helloworld?name=Ramesh"
```

**新术语**和**重要词语**以粗体显示。您在屏幕上、菜单中或对话框中看到的文字，例如，在文本中显示为：“单击 **List Web Scripts** 链接。”

警告或重要提示会显示在类似这样的框中。

提示与技巧显示如下。

## 读者反馈

我们始终欢迎读者的反馈。请告诉我们您对本书的看法——您喜欢哪些内容，或可能不喜欢哪些内容。读者反馈对我们开发真正让您获益的图书非常重要。

如需发送一般性反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中注明书名即可。

如果您在某个主题上具备专业知识，并且有兴趣撰写或参与编写图书，请参阅 www.packtpub.com/authors 上的作者指南。

## 客户支持

既然您已成为 Packt 图书的拥有者，我们提供多项服务帮助您充分利用所购书籍。

### 下载示例代码

您可以从账户中下载所购所有 Packt 图书的示例代码文件，地址为 http://www.packtpub.com。如果您从其他渠道购买本书，可以访问 http://www.packtpub.com/support 并注册，以便将文件直接通过电子邮件发送给您。

### 勘误

尽管我们已尽一切努力确保内容准确，但错误仍在所难免。如果您在我们的某本书中发现错误——无论是文本还是代码中的错误——我们将不胜感激您能向我们报告。这样做可以帮助其他读者避免困扰，也有助于我们改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择您的图书，点击勘误提交表单链接，并输入勘误详情。您的勘误一经核实，提交即会被接受，勘误将上传到我们的网站，或添加到该图书“勘误”部分的现有勘误列表中。您可以在 http://www.packtpub.com/support 中选择自己的图书来查看任何现有勘误。

### 盗版

互联网上对版权材料的盗版是所有媒体持续面临的问题。在 Packt，我们非常重视对版权和许可的保护。如果您在互联网上发现任何形式的我们作品的非法副本，请立即将位置地址或网站名称告知我们，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并提供疑似盗版材料的链接。

我们感谢您帮助保护我们的作者，以及我们为您提供有价值内容的能力。

### 问题

如果您在本书的任何方面遇到问题，可以通过 questions@packtpub.com 联系我们，我们将尽力解决。

- 它们可在不同平台上重用
- Web 脚本为广泛的客户端应用提供对内容的统一访问

从技术角度来说，Web 脚本可以定义如下：

- 它们是 RESTful Web 服务

> **什么是 RESTful？**
> REST 代表 Representational State Transfer（表述性状态转移），本质上是一种架构风格。通过 HTTP 请求方法对资源进行清晰且统一的访问、通过 URI 唯一标识资源，以及这些资源的表示形式，是 REST 的关键原则。遵循这种架构风格的任何实现都被称为 RESTful。

- 它们绑定到特定的统一资源标识符（URI）
- 它们响应 GET、POST、PUT 和 DELETE 等 HTTP 方法
- 它们是轻量级实现

从开发者的角度来看，Web 脚本具有以下特性：

- 它们易于理解与学习
- 它们易于开发
- 它们易于调试
- 它们易于维护
- 它们易于部署
- 它们实现速度更快
- 当需要从外部应用安全地访问 Alfresco 存储库时，它们将是首选

从业务用户的角度来看，Web 脚本具有以下用途：

- 它们弥合了使用 Alfresco 构建业务解决方案时业务需求与技术实现之间的差距
- 它们增强了 Alfresco 的集成能力，使其能够与外部系统开发有用的 Alfresco 集成解决方案
- 它们是在 Alfresco 存储库之上实现集成解决方案的独特方式
- 它们是使用 Alfresco 存储库实现集成解决方案的支柱

- 另一个需要重点注意的是，借助 Web 脚本的事务支持，可以在一个 Web 脚本中同时执行一组操作；而在 CMIS 中，事务的使用存在限制。可以单独执行每个操作，但无法像 Web 脚本那样在单个事务中同时执行一组操作。

基于 SOAP 的 Web 服务并不可取，原因如下：

- 开发它们需要很长时间
- 它们依赖 SOAP
- 客户端需求更重
- 它们需要维护资源目录
- 可扩展性是个挑战
- 它们只支持 XML

相比之下，Web 脚本具有以下特性：

- 没有复杂的规范
- 不依赖 SOAP
- 无需维护资源目录
- 由于无需维护会话状态，它们更具可扩展性
- 它们是轻量级实现
- 它们简单且易于开发
- 它们支持多种格式

在开发者看来：

- 它们可以使用任何文本编辑器轻松开发
- 使用脚本语言时无需编译
- 使用脚本语言时无需重启服务器
- 无需复杂安装

本质上：

- 与传统的基于 SOAP 的 Web 服务和 CMIS 替代方案相比，Web 脚本是一种基于 REST 且功能强大的与 Alfresco 存储库交互的选择
- 它们提供对 Alfresco 存储库中内容的 RESTful 访问，并为广泛的客户端应用提供统一访问




- 它们易于开发，并提供了一些最有用的功能，例如无需服务器重启、无需编译、无需复杂安装，以及无需特定工具来开发它们
- 所有这些特点使得Web脚本成为开发者和架构师在与Alfresco仓库交互时的首选

## 理解何时使用Web脚本

在了解了什么是Web脚本以及使用Web脚本的原因之后，现在让我们理解何时使用Web脚本。在使用Alfresco时，了解Web脚本可以在哪些场景下使用是很重要的。Web脚本允许你构建自定义的、由URI标识且可通过HTTP访问的内容管理Web服务，以访问Alfresco内容仓库，因此它们可以用于各种类型的实现，以使用Alfresco内容管理系统构建有用的业务解决方案。

在开发项目中：

- 你可以使用Web脚本开发将Alfresco与外部应用程序集成的业务实现
- 你可以根据业务需求开发自定义的Web脚本
- 你可以在自定义实现中利用Alfresco提供的开箱即用的Web脚本，只要它们符合你的需求

在支持项目中：

- 当你在支持或维护项目中为客户维护基于Alfresco构建且拥有数百万条记录的生产系统时，Web脚本可能是你的救星。
- 你可能会遇到一系列问题，例如，更新现有内容以修复某些数据问题、删除一些不需要的内容、获取符合业务团队指定条件的所需内容等。在这些场景中，可以使用Web脚本来达成这些目的。
- 在生产环境中，无法重启服务器，而重启服务器可能会影响客户的业务。考虑这样一个场景：你需要修复生产系统上的一些关键问题，并且希望在不重启服务器的情况下解决问题。Web脚本无需重启服务器即可与Alfresco仓库交互的强大功能在这种情况下会帮助你，你可以使用Web脚本修复一些关键问题，从而为业务用户提供良好的内容管理体验。

一般来说，Web脚本可以用于各种解决方案，例如：

- 将Alfresco与可通过HTTP与Alfresco通信的外部应用程序集成
- 开发JSR-168 portlet以构建UI服务
- 开发数据服务
- 为仓库内容提供提要（feeds）
- 将Alfresco与Office集成
- 开发Facebook应用程序
- 在Alfresco SURF中构建UI组件

## 理解Web脚本可以在哪里使用

现在，是时候了解Web脚本可以在哪里使用了。有许多不同的环境可以访问和使用Web脚本。

在不同的环境中：

- 它们可以从HTTP客户端（如Web浏览器和HTTP客户端API）使用
- 也可以在JSF页面中访问Web脚本
- 它们可以在JSR-168门户中使用
- 它们可以从Facebook应用程序中使用
- Web脚本也可以用于Alfresco SURF平台

## 理解Web脚本如何工作

Alfresco中的Web脚本大多使用模型-视图-控制器（MVC）模式。然而，并不强制要求始终遵循这种模式。MVC模式在Web脚本中的工作方式如下：

- **控制器**负责根据业务需求执行所需的业务逻辑。处理完业务逻辑后，控制器将所需数据填充到**模型**对象中。完成后，控制器将请求转发给**视图**。

## Web脚本中的MVC

Web脚本中的模型-视图-控制器模式解释如下：

- 控制器可以是服务器端的JavaScript或Java类，也可以两者兼有
- 模型是在控制器和视图之间传递的数据结构对象
- 视图是负责以所需格式生成响应的FreeMarker模板

通过描述符文件完成Web脚本URI到控制器的映射。它主要是一个XML文件，其中包含Web脚本所需的信息，例如URL、描述、参数、事务、身份验证和响应格式。

响应格式通过命名约定映射到FreeMarker模板。例如，返回HTML响应的FreeMarker模板将具有`html.ftl`扩展名。

Web脚本由Alfresco中的Web脚本引擎注册和执行。

> 在一般场景下，描述符、可选控制器以及一个或多个FreeMarker响应模板共同构成一个Web脚本。所有这些组件通过特定的文档命名约定绑定在一起。

## 使Web脚本工作

要使Web脚本工作，将Web脚本文件部署到合适的位置非常重要。

描述符文件和根据响应格式生成的FreeMarker模板既可以放在文件系统上，也可以放入Alfresco仓库中。

如果Web脚本使用基于JavaScript的控制器，则它可以与描述符和FreeMarker模板一起放在文件系统上或放入Alfresco仓库中。

如果Web脚本使用基于Java的控制器，则控制器的类文件必须位于类路径上。

> Web脚本有两种可用的控制器类型：基于JavaScript的控制器和基于Java的控制器。一个Web脚本可以有其中一种、两种都无，甚至两种都有。

## Web脚本的类型

Alfresco中有两种类型的Web脚本：数据Web脚本和表示Web脚本。

### 数据Web脚本

数据Web脚本为客户端应用程序提供仓库接口，以便在仓库中创建、检索、更新和删除内容/数据。这些Web脚本通常以XML和JSON等格式发送响应，客户端应用程序必须解析这些响应才能进一步使用。

数据Web脚本封装了对仓库中内容和数据的访问和修改。这些Web脚本仅由Alfresco仓库服务器提供和暴露。

### 表示Web脚本

表示Web脚本可用于构建用户界面，例如用于Alfresco Explorer的dashlet、用于Alfresco Share的dashlet、用于JSR-168门户的portlet、Alfresco SURF中的UI组件或自定义应用程序。

这类Web脚本通常呈现HTML响应。

可以将这些Web脚本托管在独立的表示服务器上，也可以由Alfresco仓库服务器暴露。

表示Web脚本通常会调用数据Web脚本以从仓库获取所需数据。

> 在Alfresco安装中可用的开箱即用Web脚本可以在`ALFRESCO_HOME\tomcat\webapps\alfresco\WEB-INF\classes\alfresco\templates\webscripts\org\alfresco`找到，其中ALFRESCO_HOME是Alfresco安装的基础目录。
> 如果你使用的是Alfresco Community 5，你可以在`alfresco-remote-api-*.jar`中的alfresco包下找到开箱即用的Web脚本，该JAR位于`ALFRESCO_HOME\tomcat\webapps\alfresco\WEB-INF\lib`。

## 总结

在本章中，我们通过一些关于Web脚本的基本问题对Web脚本有了更好的理解。我们学习了Alfresco中的Web脚本是什么、为什么它们是开发者的首选、何时何地可以使用Web脚本、Web脚本如何工作，以及Alfresco中可用的不同Web脚本类型。

在下一章中，我们将进行一个动手练习，学习如何在Alfresco中实现我们的第一个Web脚本。
