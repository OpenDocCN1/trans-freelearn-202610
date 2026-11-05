# Spring Python 1.1

## Credits

**Author**
Greg Lee Turnquist

**Editorial Team Leader**
Aanchal Kumar

**Reviewers**
Bala Sundarasamy
Russ Miles
Sylvain Hellegouarch

**Project Team Leader**
Lata Basantani

**Project Coordinator**
Joel Goveya

**Acquisition Editor**
Steven Wilding

**Proofreader**
Lesley Harrison

**Development Editor**
Mehul Shetty

**Graphics**
Geetanjali Sawant

**Technical Editor**
Aditya Belpathak

**Production Coordinator**
Shantanu Zagade

**Indexer**
Hemangini Bari

**Cover Work**
Shantanu Zagade

## About the Author

Greg Lee Turnquist 自1997年起从事软件行业。他是开源社区的积极参与者，并为多个项目贡献了补丁，包括 MythTV、Spring Security、MediaWiki 和 TestNG Eclipse 插件。作为一名饱经测试的脚本爱好者，他始终寻求适合工作的正确工具。他坚信敏捷实践和自动化测试。他开发过分布式系统、基于 LAMP 的设置，并支持托管在各种平台上的关键任务系统。

在从 Auburn University 获得计算机工程硕士学位后，Greg 开始在 Harris Corporation 工作。他参与了利用多种技术类型的众多合同。2006年，他创建了 Spring Python 项目，该项目成为首个达到正式状态的 Spring 扩展。他最近加入 SpringSource，成为其国际软件开发团队的一员。

> 我要感谢 Russ Miles 和 Sylvain Hellegouarch 抽出时间对本书进行技术审阅并提供宝贵反馈。我还要感谢我的父亲 Dr. Paul Turnquist 给予我写作的灵感。最重要的是，我要感谢我的妻子 Sara 的支持、鼓励和耐心。

## About the Reviewers

**Bala Sundarasamy**，毕业于金迪工程学院。他在设计和构建使用 Java 和 .Net 技术的应用方面拥有超过17年的丰富经验。他在印度一些领先的软件咨询公司担任过技术职能和项目交付的多个角色。

他还教导过众多年轻开发者使用 Java 和 C# 编写良好的面向对象代码。他在培训新工程师采用软件编写中的行业标准最佳实践和流程方面拥有 proven 经验。

**Russ Miles** 是三本畅销书的作者：*AspectJ 实用手册*、*学习 UML 2.0* 和 *Head First 软件开发*（均由 O' Reilly Media 出版）。特别是，为 *Head First* 系列撰写文章是在如何将枯燥的技术主题转化为精彩学习体验方面的宝贵学习经历；他已将这些技能成功应用于多个成功的课程和工作坊的开发。

他是关于许多广泛技术主题的频繁演讲者，包括作为自称“多语言程序员”的乐趣。作为一名认证的 Scrum Master，他在更具体的技术辅导之外，还与许多公司合作，帮助他们接受软件开发最终关乎人的事实。

他是总部位于伦敦的高级软件开发咨询公司 OpenCredo 的首席顾问和创始人。作为日常工作的一部分，Russ 在找到时间时为多家出版物撰写关于当下热门软件开发主题的文章。

> 我要感谢我美丽的妻子 Corinne 和我的“小怪物” Mali 的支持、欢笑和头发拉扯（只是 Mali）。

**Sylvain Hellegouarch** 是 Ubikod 的高级软件开发人员，该公司致力于创新移动解决方案。他热爱开源软件，并创建了许多基于 AtomPub 和 XMPP 协议的针对社交通信的 Python 项目。他撰写了由 Packt Publishing 于2007年出版的 *CherryPy 精要* 一书，这是一本关于流行 Web 框架 CherryPy 的实践书籍。他目前的兴趣正推动他进入语义网、关联数据以及人们如何相互互动的丰富世界。

## 前言

2004 年，Java 行业正在酝酿一场新的革命：Spring Framework。它通过提供务实的解决方案而无需强加的要求，挑战了单体、一刀切的应用程序服务器的概念。与之前的框架不同，Spring 不要求全有或全无的采用。相反，它通过为常用模式提供便捷的抽象，提供了一种实用的方法。它还拥抱了依赖注入的概念，允许非侵入式地采用强大的选项。

Spring Framework 不依赖于其所编码的技术。被称为 Spring 之道 的概念和方法无处不在。2006 年，我创建了 Spring Python 项目。这是为了将 Spring 的非侵入式、强大的概念与 Python 的敏捷、动态灵活性相结合。

本书将指导您了解 Spring Python 的构建模块，旨在为您提供构建更好 Python 应用程序的关键。

前七章向您展示如何下载并开始使用 Spring Python。这些章节中的每一章都介绍了一个不同的模块，您可以使用它来构建应用程序。第八章展示了如何协同使用这些模块来构建更强大且更易于维护的应用程序。第九章介绍了一个用于快速创建应用程序的命令行实用程序。最后一章展示了如何快速、轻松地将 Python 和 Java 集成在一起。

我写这本书的目的是让开发人员能够发现 Spring 之道的广度和美感，以及它如何丰富开发，而不会使之复杂化。我经常将软件开发视为更多的是工艺而非科学，并相信 Spring Python 可以成为任何开发人员工具箱中的宝贵工具。

## 本书涵盖的内容

第 1 章：开始使用 Spring Python 向您快速展示 Spring Python 的各种功能，然后提供如何下载和安装它的信息。

第 2 章：Spring Python 的核心 – 控制反转 向您介绍 Spring Python 的核心容器，该容器在整本书中重复使用，以增强测试和非侵入式功能。

第 3 章：向 API 添加服务 展示了如何使用 Spring Python 的面向方面编程平滑地向代码添加横切服务。

第 4 章：使用 Spring Python 轻松编写 SQL 查询 向您展示如何快速编写纯 SQL 查询，而无需处理令人发怵的样板代码。它还展示了 Spring Python 如何很好地与基于 ORM 的持久性配合使用。

第 5 章：使用事务为数据访问添加完整性 展示了如何无缝地用 SQL 事务包装业务代码。

第 6 章：使用 Spring Python 保护您的应用程序 展示了如何用灵活的身份验证和授权服务包装 Web 应用程序，同时轻松支持策略更改。它还展示了如何编写自定义扩展以集成当前不支持的安全系统。

第 7 章：使用 Spring Python 的远程调用跨节点扩展应用程序 展示了 Spring Python 如何很好地与 Pyro、Python 的 RPC 库集成。这种强大的组合将允许您在桌面上非侵入式地开发简单应用程序，并将其重新配置为多节点生产系统

第 8 章：案例研究 I – 将 Spring Python 与您的 Web 应用程序集成 邀请您使用 Spring Python 的所有组件构建一个强大且安全的银行应用程序。这展示了 Spring Python 的各个部分如何协同工作以增强开发人员的能力。

第 9 章：使用 Coily 创建骨架应用程序 介绍 Spring Python 基于插件的 Coily 工具，用于快速创建 Web 应用程序。

第 10 章：案例研究 II – 将 Spring Python 与您的 Java 应用程序集成 以展示使用 Jython 将 Java 和 Python 组件链接在一起是多么容易的方式结束本书。这允许多语言程序员混合他们喜欢的服务。

## 本书所需

您需要 Python 2.4 或更高版本以及 Spring Python 1.1.0.FINAL（在第 1 章中介绍）。

## 本书适合人群

本书适合希望通过添加/使用可扩展应用程序的部分来将应用程序提升到下一个水平，而不会增加不必要复杂性的 Python 开发人员。对于希望混合一些 Python 以加快编码工作的 Java 开发人员也很有帮助。

## 约定

在本 书中，您会发现多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义的解释。

代码中的单词显示如下：“我们向 `SpringBankView` 添加了 `closeAccount`，以便客户在余额为零时关闭现有账户。”

代码块设置如下：

```
def get_article(self, article):
    if article in self.cache:
        return self.cache[article]
    else:
        return self.delegate.get_article(article)
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
def get_article(self, article):
    if article in self.cache:
        return self.cache[article]
    else:
        return self.delegate.get_article(article)
```

任何命令行输入或输出均按如下方式编写：

```
$ svn checkout https://src.springframework.org/svn/se-springpython-py/trunk/springpython springpython
```

**新术语**和**重要单词**以粗体显示。例如，您在屏幕、菜单或对话框中看到的单词，会在文本中以如下方式出现：“让我们以 **gen-cherrypy-app** 插件的布局为例。”




![](img/bcf886d8b665372ee8afbb620d4273bb_16_0.png)

警告或重要注意事项会以如下框形式出现。

![](img/bcf886d8b665372ee8afbb620d4273bb_16_1.png)

提示和技巧会以如下方式出现。

## 读者反馈

我们始终欢迎读者的反馈。请告诉我们您对本书的看法——您喜欢或可能不喜欢的部分。读者的反馈对我们开发您真正能充分利用的书目非常重要。

如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中提及书名即可。

如果您需要某本书并希望我们出版，请在 www.packtpub.com 上的 SUGGEST A TITLE 表单中留言，或发送电子邮件至 suggest@packtpub.com。

如果您在某个主题方面有专业知识，并有兴趣撰写或为相关书籍贡献内容，请参阅 www.packtpub.com/authors 上的作者指南。

## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供多项帮助以助您充分利用购买。

![](img/bcf886d8b665372ee8afbb620d4273bb_16_2.png)

**下载本书的示例代码**
访问 https://www.packtpub.com//sites/default/files/downloads/0660_Code.zip 直接下载示例代码。
可下载文件包含使用说明。

## 勘误

尽管我们已尽最大努力确保内容的准确性，但错误仍可能发生。如果您在我们的某本书中发现错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。这样做可以避免其他读者感到沮丧，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/support，选择您的图书，点击 **告诉我们** 链接，并输入勘误详情。一旦您的勘误得到验证，您的提交将被接受并添加到现有勘误列表中。任何现有勘误都可以通过从 http://www.packtpub.com/support 选择您的书名来查看。

## 盗版

互联网上版权材料的盗版是各媒体持续存在的问题。在 Packt，我们非常重视对版权和许可的保护。如果您在互联网上发现我们作品的任何非法复制品，无论形式如何，请立即提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并附上疑似盗版材料的链接。

我们感谢您帮助保护我们的作者，以及我们为您带来有价值内容的能力。

## 问题

如果您在本书的任何方面遇到问题，可以联系 questions@packtpub.com，我们将尽力解决。
