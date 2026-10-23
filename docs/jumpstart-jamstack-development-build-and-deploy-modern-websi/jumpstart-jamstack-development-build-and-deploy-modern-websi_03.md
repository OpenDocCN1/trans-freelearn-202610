## 前言

Jamstack（JavaScript、API 和标记）通过利用 Gatsby、Sanity 和 Netlify 等工具，使 Web 开发人员能够创建和发布以速度、安全性和可访问性为核心的现代且易于维护的网站和 Web 应用。使用 Jamstack 工作的开发人员将通过这份关于静态站点生成和内容管理的实用指南，很好地利用他们的知识。这本 Jamstack 书籍采用实践方法，介绍实施和相关方法论，让您迅速掌握现代 Web 开发。

通过逐步解释核心概念、实用示例和自测问题，您将从构建活动和场地模式结构开始，然后扩展功能，探索 Jamstack 所提供的所有内容。您将学习如何构建一个示例 Jamstack，如何使用模式构建结构化内容，如何使用 GraphQL 公开内容，以及如何使用 Gatsby 通过页面和模板组件构建活动网站。最后，您将把网站部署到 Netlify 服务器。

读完本书后，您将获得使用 Jamstack 安装、配置、构建、扩展和部署简单活动网站所需的知识和技能。

### 本书面向读者

本书面向希望实际实施 Jamstack 的 Web 开发人员。希望构建现代、快速且安全的 Web 应用的 JavaScript 开发人员也会发现本书很有用。假设读者熟悉 JavaScript 和数据库编程。

### 本书内容

*第 1 章，Jamstack 的历史*，为您提供 Web 设计和开发历史的概览，以及后端驱动网站面临的问题，即安全性、速度、可访问性、托管和部署问题。

*第 2 章，Sanity 简介*，教您如何使用基于网络的示例项目创建工具创建 Sanity.io 网站。

第 3 章，探索 Sanity Studio，向您介绍 Sanity Studio，一个结构化内容管理工具。然后，您将学习如何配置网站以及创建、修改和删除内容。

第 4 章，Sanity 配置与模式，帮助您理解 Sanity.io 内容模式的术语和语法，以及它们如何定义内容结构。您还将学习如何定义实体之间的一对一关系和一对多关系。

第 5 章，Sanity 的 GROQ 语言，向您介绍 GROQ，即 Sanity.io 查询其数据的专有方式。将展示它与标准 SQL 的相似之处和不同之处，并详细说明各种示例。

第 6 章，Sanity 的 GraphQL 游乐场，向您介绍 Sanity.io 的 GraphQL 游乐场，如何启动它，以及它与标准 GraphQL 有何不同。您将学习如何获取单条记录类型或多条记录。

第 7 章，Gatsby – 简介，向您介绍 Jamstack 的 J 部分。您将了解 GatsbyJS，一个基于 ReactJS 的静态生成器，它被创建的原因，它是如何构建的，以及它解决的问题。最后，您将了解涉及的相关配置文件和关键文件（如 ssr 和 node）。

第 8 章，Gatsby 与 GraphQL，向您介绍 Gatsby 的 GraphQL 工具，以及它与标准 GraphQL 有何不同。本章将帮助您理解如何通过 Gatsby 源代码使用 GraphQL 将源内容连接并服务于静态站点生成器。

第 9 章，Gatsby 源插件，将向您介绍由 Jamstack 社区构建的各种插件，这些插件允许 GatsbyJS 连接到各种类型的知名后端系统，从文件系统到现有的 CMS 框架，如 Drupal。

第 10 章，构建 Gatsby 组件，将向您介绍 Jamstack 的 M 部分，即标记，以及 Gatsby 的构建块：组件，同时学习页面、模板和部分的结构以及它们之间的差异。您将学习如何创建、编辑和扩展组件。

第 11 章，API – 扩展 Gatsby，向您介绍 Jamstack 的 A 部分，即 API，以及如何在编译 Web 页面的动态部分中使用它们，以重现 Web 开发人员在传统基于服务器的 Web 开发中熟悉的功能。




第 12 章，API – Alexa 技能，将教你如何构建 Jumpstart Jamstack Alexa 技能。作为一个示例，你可以询问即将到来的五个事件。通过这个示例，你可以通过检索过去的事件或所有事件来扩展当前的技能，你还可以为任何其他应用程序创建一个新的技能。

第 13 章，整合所有部分，将教你如何修改 CSS 以及网站的最终元素，以及如何配置和发布 GatsbyJS 生产构建。

第 14 章，使用 Netlify 和 Azure 部署，将介绍 Netlify 无服务器持续部署托管服务。你将学习如何将网站的仓库连接到 Netlify，以及 Netlify 如何准备和组合各种文件，以形成一个快速、高效执行的单一静态文件。

第 15 章，结论，将总结全书所学的概念，讨论各个部分如何结合在一起，并探讨 Jamstack 社区如何发展。本章将讨论各种外部项目以及如何为它们做贡献。

## 如何充分利用本书

| 本书中涵盖的软件/硬件 | 操作系统要求 |
|---|---|
| React JavaScript 框架（版本 16.14.0） | Windows、macOS X 或 Linux（任意） |
| Gatsby 前端框架（版本 2.9.2 或更高） | Windows、macOS X 或 Linux（任意） |
| Sanity.io 统一内容平台（版本 2.9.1） | Windows、macOS X 或 Linux（任意） |
| Netlify（版本 3.18.3） | Windows、macOS X 或 Linux（任意） |
| Alexa 技能 | Windows、macOS X 或 Linux（任意） |

> 注意：  
> 在撰写本书时，使用了 Gatsby、React、Netlify 和 Sanity 的最新版本。未来的任何更新都将推送并更新到 GitHub 仓库中。

如果你使用的是本书的数字版本，我们建议你自行输入代码或通过 GitHub 仓库（链接将在下一节中提供）访问代码。这样做有助于你避免任何与代码复制/粘贴相关的潜在错误。

## 下载示例代码文件

你可以从 www.packt.com 的账户下载本书的示例代码文件。如果你是在其他地方购买本书，你可以访问 www.packtpub.com/support 并注册，以便直接通过电子邮件接收文件。

你可以按照以下步骤下载代码文件：

- 1. 登录或注册 www.packt.com。
- 2. 选择**支持**选项卡。
- 3. 点击**代码下载**。
- 4. 在**搜索**框中输入书名，并按照屏幕上的说明操作。

文件下载后，请确保使用以下最新版本解压或提取文件夹：

- Windows 使用 WinRAR/7-Zip
- Mac 使用 Ziipeg/iZip/UnRarX
- Linux 使用 7-Zip/PeaZip

本书的代码包也托管在 GitHub 上，地址为 `https://github.com/PacktPublishing/Jumpstart-Jamstack-Development`。如果代码有更新，将在现有的 GitHub 仓库中更新。

我们还有来自我们丰富的书籍和视频目录的其他代码包，位于 `https://github.com/PacktPublishing/`。欢迎查看！

## 下载彩色图像

我们还提供了一份包含本书中使用的截图/示意图彩色图像的 PDF 文件。你可以从这里下载：`https://static.packt-cdn.com/downloads/9781800203495_ColorImages.pdf`。

## 使用的约定

本书中使用了多种文本约定。

**文本中的代码：** 表示文本中的代码单词、数据库表名、文件夹名称、文件名、文件扩展名、路径名、虚拟 URL、用户输入以及 Twitter 账号。这里是一个示例：“将下载的 `WebStorm-10*.dmg` 磁盘映像文件作为系统上的另一个磁盘挂载。”

代码块设置如下：

```javascript
{
    resolve: "gatsby-source-sanity",
    options: {
      ...clientConfig.sanity,
      token: process.env.SANITY_READ_TOKEN,
      watchMode: !isProd,
      overlayDrafts: !isProd
    }
  }
```

当我们希望引起你对代码块中某一部分的注意时，相关行或条目将以**粗体**显示：

```text
[default]
  exten => s,1,Dial (Zap/1|30)
  exten => s,2,Voice mail (u100)
**exten => s,102,Voice mail (b100)**
  exten => i,1,Voice mail (s0)
```

任何命令行输入或输出都按如下方式书写：

```bash
npm install -g gatsby
```

**粗体：** 表示新术语、重要单词或你在屏幕上看到的单词。例如，菜单或对话框中的单词在文本中是这样显示的。这里是一个示例：“从**管理**面板中选择**系统信息**。”

> 提示或重要说明  
> 看起来像这样。

## 联系我们

我们随时欢迎来自读者的反馈。

**一般反馈：** 如果你对本书的任何方面有问题，请在邮件主题中提到书名，并发送邮件至 customer.care@packtpub.com。

**勘误：** 尽管我们已尽一切努力确保内容的准确性，但仍可能发生错误。如果你发现本书中有错误，我们将感激不尽，请你向我们报告。请访问 www.packtpub.com/support/errata，选择你的书籍，点击“勘误提交表单”链接，并输入详细信息。

**盗版：** 如果你在互联网上以任何形式发现我们作品的非法副本，我们将感激不尽，请你提供位置地址或网站名称。请联系 `copyright@packt.com`，并附上材料链接。

**如果你有兴趣成为作者：** 如果你拥有专业知识领域，并有兴趣编写或参与编写一本书，请访问 authors.packtpub.com。

## 评价

请留下评价。阅读并使用本书后，何不就在你购买本书的网站留下评价？潜在的读者可以查看并利用你的客观意见做出购买决定，我们 Packt 可以了解你对我们产品的看法，我们的作者也可以看到你对他们书籍的反馈。谢谢你！

有关 Packt 的更多信息，请访问 packt.com。
