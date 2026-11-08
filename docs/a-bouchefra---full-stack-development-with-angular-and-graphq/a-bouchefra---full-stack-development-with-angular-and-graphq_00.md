# A. Bouchefra - Angular 和 GraphQL 全栈开发. 学习使用 Apollo、Lerna 和 GraphQL 构建可扩展的 monorepo 和完整的 Angular 应用 (2022)





致我的母亲，以及我父亲的记忆。
- Ahmed Bouchefra

## 贡献者

## 关于作者

**Ahmed Bouchefra** 是软件开发人员和技术作者，拥有软件开发工程学位。他是使用现代技术（如 Angular 和 Node.js）进行 Web 开发的专家，同时也使用 Python 和 Django 等传统技术。他是开源贡献者、开发者以及多个开源 Angular 库（如 ngx-qrcode2）的维护者。他目前还在 techiediaries.com 及其他行业领先网站上撰写关于现代 Web 开发的教程。

## 关于审阅者

**Peter Eijgermans** 是一名长期从事软件开发的开发者，也是 Ordina Netherlands 的一名充满冒险精神和热情的 Codesmith 前端开发者。他喜欢骑自行车环游世界，始终在寻找意想不到和未知的事物。作为工作的一部分，他尝试最新的技术和框架。他喜欢通过在世界各地的会议上演讲以及为荷兰 Java 杂志和 DZone 撰稿来分享自己的经验。他认为前端开发者是网络中的蜘蛛，将用户、项目团队和产品联系在一起。

_我谨感谢以下同事在审阅本书时提供的帮助：_

- _Corneel Eijsbouts_
- _Jeroen Verhoeven_
- _Michael Awad_

## 前言

Angular 是现代 Web 应用开发中最流行的 JavaScript 框架之一，它使开发人员不仅可以构建应用并重用代码，还可以为任何部署目标开发应用。对于 Web、移动 Web、原生移动和原生桌面，GraphQL 是查询 Web API 的现代 REST 替代方案。使用 Angular、TypeScript 和 GraphQL 将为您提供一个面向未来且可扩展的技术栈，您可以围绕它开始构建应用。

本书将向您展示如何使用前沿技术构建应用。您将学习如何使用 GraphQL 和 Apollo 解决常见的 Web 开发问题，例如数据库访问、身份验证和图像上传。

本书首先向您介绍如何使用 Angular 和 GraphQL 构建全栈应用。之后，您将学习如何使用 Lerna 和 NPM 工作区创建 monorepo 项目，并配置 Node.js 应用以使用 Express 和 Apollo Server 的 GraphQL。您还将了解 Angular 架构和路由的基础知识。然后，本书演示如何使用 Angular Material 构建专业外观的 UI，并使用 Apollo Client 与服务器交互，从内置的 GraphQL API 获取数据。

您将了解 Apollo Client 的类型和字段策略，以及各种获取策略。除此之外，您还将学习 Apollo Client 中的本地状态管理和响应式变量，以及如何使用 GraphQL Code Generator 为您的 GraphQL 模式和查询生成 TypeScript 类型甚至 Angular Apollo 服务，以实现可扩展的代码库。

读完本书后，您将具备构建自己的全栈应用所需的技能。

## 本书适合谁

这本 Angular GraphQL 书籍适合想要学习如何使用 GraphQL 和 Apollo 与 Angular 构建全栈应用的 Angular 开发人员。本书不假设您具备使用 Angular 和 GraphQL 进行全栈开发的先验知识。

## 本书涵盖内容

*第 1 章，应用架构和开发环境，* 教您了解项目的结构以及开发应用所需的工具。之后，您的机器将准备好进行开发。具体来说，您将安装 Node.js 以及 npm 和 MySQL。

*第 2 章，使用 Node.js、Express.js 和 Apollo 设置 GraphQL，* 帮助您构建后端应用，以提供 Angular 应用将使用的 API。

*第 3 章，使用 TypeORM 连接数据库，* 教您如何使用 TypeORM 将 MySQL 数据库连接到应用，以及如何创建解析器从数据库获取和保存数据。

*第 4 章，使用 Apollo Server 实现身份验证和图像上传，* 帮助您了解如何使用 Apollo Server 为 GraphQL API 添加身份验证和图像上传，实现更多解析器。您将学习使用 Node.js、Express 和 Apollo Server 添加身份验证所需的概念，然后学习如何处理图像上传。

*第 5 章，使用 Apollo Server 添加实时支持，* 帮助您为服务器应用添加实时支持，从而允许服务器在数据可用时立即将最新数据传达给客户端。为此，您将使用 Apollo Server 的 GraphQL 订阅。

*第 6 章，Angular 应用架构和路由，* 通过安装 Angular CLI 并使用最新版本的 Angular 创建新项目来开始。之后，您将利用 Angular CLI 创建构成应用 UI 的模块、服务和组件，并介绍依赖注入。

*第 7 章，添加用户搜索功能，* 具体介绍如何使用 Apollo Client 将前端与后端集成，Apollo Client 旨在向服务器发送 GraphQL 查询和变更以获取和写入数据。然后，您将开始实现身份验证。

*第 8 章，保护路由和测试身份验证，* 通过保护必要的路由免受未经授权的访问、向 API 请求发送 JWT 以及对代码进行单元测试，继续实现我们的身份验证系统。

*第 9 章，上传图像和添加帖子，* 实现个人资料组件的功能。您将添加必要的代码来获取与个人资料 URL 对应的用户并在页面上渲染其信息，包括上传用户照片和封面图像以及添加个人简介的能力。

第 10 章，获取帖子并添加评论和点赞，首先处理身份验证令牌过期问题，然后您将继续完善个人资料组件，通过发送查询接收分页的帖子和评论数据，并通过变更向帖子添加评论和点赞。

第 11 章，实现 GraphQL 订阅，在学习如何为应用添加实时支持以便无需不断刷新应用即可从服务器检索和显示新数据之前，继续构建用户的个人资料组件。您将使用 Apollo Client 和 Angular 的 GraphQL 订阅来实现此功能。

## 如何充分利用本书

![](img/c798729e4c42765fcb1c98950164d0ba_15_0.png)

本书涵盖了安装 Node.js 和 MySQL 的内容。

进一步的前提条件是熟悉 JavaScript/TypeScript、HTML 和 CSS。

如果您使用的是本书的数字版，我们建议您自己输入代码或从本书的 GitHub 仓库访问代码（链接在下一节提供）。这样做有助于避免与复制和粘贴代码相关的潜在错误。

## 下载示例代码文件

您可以从 GitHub 下载本书的示例代码文件，地址为 https://github.com/PacktPublishing/Full-Stack-App-Development-with-Angular-and-GraphQL。如果代码有更新，将在 GitHub 仓库中更新。

我们还有其他来自我们丰富图书和视频目录的代码包，可在 https://github.com/PacktPublishing/ 获取。快去看看吧！

## 下载彩色图片

我们还提供了一份 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。您可以在此处下载：https://static.packt-cdn.com/downloads/9781800202467_ColorImages.pdf。

## 使用的约定

本书通篇使用了多种文本约定。

代码中的文本：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。示例：“Open the lerna.json file and set version to "0.0.1":”

代码块如下所示：

```
{
  "packages": [
    "packages/*"
  ],
  "version": "0.0.1"
}
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
import express, { Application } from 'express';
import { ApolloServer } from 'apollo-server-express';
import schema from './graphql/schema';
```

任何命令行输入或输出均如下所示：

```
curl -sL https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh
```

粗体：表示新术语、重要词语或屏幕上显示的词语。例如，菜单或对话框中的词语以粗体显示。示例：“You can click on the Query your server button to access Apollo Studio, where you can send requests to your API endpoint.”

提示或重要说明
如下所示显示。



## 联系我们

我们热烈欢迎读者的反馈。

**一般反馈：** 如果您对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中注明书名。

**勘误：** 尽管我们已尽一切努力确保内容的准确性，但错误难免。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表格。

**盗版：** 如果您在互联网上发现我们作品的任何形式的非法复制品，我们将不胜感激您提供其位置地址或网站名称。请通过 `copyright@packt.com` 联系我们，并附上相关材料的链接。

**如果您有兴趣成为作者：** 如果您在某个领域有专业知识，并且有兴趣撰写或为一本书做出贡献，请访问 `authors.packtpub.com`。

## 分享您的想法

在您阅读完 *Full-Stack Development with Angular and GraphQL* 后，我们很想听听您的想法！请为本书选择 `https://www.amazon.in/review/create-review/error?asin=1800202466` 并分享您的反馈。

您的评论对我们和技术社区都很重要，将帮助我们确保提供优质的内容。
