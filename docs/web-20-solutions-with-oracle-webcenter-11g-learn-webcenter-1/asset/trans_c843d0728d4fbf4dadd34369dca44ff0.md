

## 第 10 章：Oracle WebCenter Spaces 概述

- Oracle WebCenter Spaces
  - Oracle WebCenter Personal Spaces
  - Oracle WebCenter Group Spaces
- Oracle WebCenter Spaces 安装
- Oracle Spaces 配置
- 员工工作环境开发
- 构建业务角色页面
- 实践操作
- 开发 Group Space
- 总结
- 索引

## 前言

Oracle WebCenter Suite 是业内最全面的企业门户平台，专为业务用户设计，统一了业务应用程序、Enterprise 2.0 服务和社交社区。它包含 Web 2.0 内容、协作和通信服务。但是，如果没有关于开发性能关键型企业应用程序的适当指导，很容易做出错误的设计和技术决策。

本书包含大量截图，为您提供开发自定义 WebCenter 应用程序以满足各种企业需求的深入知识。集成各种 Web 服务，使开发更简单、更快速。通过本实践教程，构建快速、可靠且强大的 Web 2.0 应用程序。

Oracle WebCenter Suite 是一组产品，可帮助组织实施 Web 2.0 解决方案。本书使开发人员能够学习重要概念。它展示了几个场景，其中 Oracle WebCenter 11g 技术可用于提高组织的生产力。每一章都提供了技术的实际应用。它将指导您了解许多网页构建功能，例如页面和 portlet。

最后，本书描述了完整的解决方案，例如 Oracle WebCenter Spaces，以及可直接使用的协作环境。

## 本书涵盖内容

_第 1 章，什么是 Oracle WebCenter？_，总结了 Oracle WebCenter 的功能，并展示了该技术解决的业务需求。

_第 2 章，WebCenter 安装和配置_，提供了在标准开发环境中成功安装 WEB Center Suite 所需的说明。

## 前言

第 3 章，设置开发环境，帮助我们准备开发环境和应用程序服务器，以支持我们的 WebCenter 应用程序。

第 4 章，WebCenter 开发入门：Hello World(s)，帮助我们创建 HelloWorld Web 服务。我们还完成了开发环境的验证，并增强了构建 WebCenter 开发所需的基础组件和技能的信心。

第 5 章，设计和个性化，解释了如何开发模板并向我们的 WebCenter 应用程序添加数据访问功能。

第 6 章，Portlets，解释了如何在我们的应用程序中使用 portlet 实现功能，因为组件重用是我们应用程序的主要目标之一。

第 7 章，讨论、Wiki 和博客服务，演示了将 Wiki 和博客服务纳入自定义 WebCenter 应用程序的简单方法。

第 8 章，搜索服务，探索了 WebCenter 搜索服务、标签、链接和 RSS。通过创建示例应用程序，它解释了如何将 WebCenter 搜索服务集成到自定义 WebCenter 应用程序中。

第 9 章，使用 Oracle Composer 进行个性化，分两部分使用 Oracle WebCenter。在第一部分中，作为开发人员，我们为最终用户准备环境。然后，我们承担最终用户的角色，对前一步中定义的页面进行个性化设置。

第 10 章，Oracle WebCenter Spaces 概述，展示了如何配置和自定义 Oracle WebCenter Spaces。

## 本书所需内容

要开发和测试本书中的示例，您的系统上需要安装 Oracle SOA Suite 11g Patch Set 2 (11.1.1.3)。对于第 9 章，您还需要 Oracle Service Bus 11g (11.1.1.3) 和 Oracle Service Registry 11g (11.1.1.2)，对于第 10 章，您需要 Oracle BPA Suite 11g (11.1.1.2)。

- Oracle WebCenter Suite 11g
- Oracle JDeveloper 11g

## 本书适用人群

本书适用于需要使用 Web 2.0 功能改进其网站或业务应用程序的 Web 开发人员。

如果您是以下人员，则推荐您阅读本书：

- 使用传统门户并希望转向新技术的门户开发人员
- 希望为业务应用程序添加生产力功能的 Java 开发人员
- 希望创建协作环境并提高员工和组织群体生产力的知识管理人员

## 约定

在本书中，您会发现多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义说明。

文本中的代码词显示如下："我们可以通过使用 `include` 指令来包含其他上下文。"

代码块设置如下：

```
<Location /svn>
DAV svn
SVNParentPath C:\svn_repository
</Location>
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目将以粗体显示：

```
<Location /svn>
DAV svn
**SVNParentPath C:\svn_repository**
</Location>
```

任何命令行输入或输出均写作如下：
`SQL> create user paracas identified by paracas`

**新术语**和**重要词汇**以粗体显示。您在屏幕、菜单或对话框中看到的单词，例如，在文本中显示如下："选择 Default Role 并按 OK"。

![](img/fccf6c511126cf79e98510edd3b343ac_21_0.png)

![](img/fccf6c511126cf79e98510edd3b343ac_21_1.png)

## 读者反馈

我们始终欢迎读者的反馈。请告诉我们您对本书的看法——您喜欢或不喜欢的地方。读者反馈对我们开发您能真正从中获益的书籍非常重要。

要向我们发送一般反馈，只需发送电子邮件至 <feedback@packtpub.com>，并在邮件主题中注明书名。

如果您需要某本书并希望我们出版，请通过 `www.packtpub.com` 上的 **SUGGEST A TITLE** 表单或发送电子邮件至 `suggest@packtpub.com` 与我们联系。

如果您在某个领域有专长，并有兴趣编写或参与编写书籍，请参阅 `www.packtpub.com/authors` 上的作者指南。

## 客户支持

既然您是 Packt 图书的骄傲拥有者，我们有许多资源可以帮助您充分利用您的购买。

**下载本书的示例代码** 您可以从 `http://www.PacktPub.com` 上的帐户下载购买的所有 Packt 图书的示例代码文件。如果您在其他地方购买了本书，可以访问 `http://www.PacktPub.com/support` 并注册，以便文件直接发送到您的电子邮箱。

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误难免会发生。如果您在我们的某本书中发现错误——也许是文本或代码中的错误——我们将不胜感激如果您能将其报告给我们。这样做，您可以帮助其他读者避免沮丧，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 `http://www.packtpub.com/support`，选择您的书，单击 **errata submission form** 链接，并输入您的勘误详细信息。一旦您的勘误得到验证，您的提交将被接受，勘误将上传到我们的网站，或添加到该书名下的现有勘误列表中，位于该书名的 Errata 部分。任何现有勘误都可以通过从 `http://www.packtpub.com/support` 选择您的书名来查看。

## 盗版

互联网上的版权材料盗版是所有媒体中持续存在的问题。在 Packt，我们非常重视对我们的版权和许可的保护。如果您在互联网上以任何形式发现我们作品的非法副本，请立即向我们提供位置地址或网站名称，以便我们可以采取补救措施。

请通过 <copyright@packtpub.com> 与我们联系，并提供涉嫌盗版材料的链接。

我们感谢您在保护我们的作者以及我们为您提供有价值内容的能力方面提供的帮助。

## 问题

如果您在本书的任何方面遇到问题，可以联系 <questions@packtpub.com>，我们将尽力予以解决。

