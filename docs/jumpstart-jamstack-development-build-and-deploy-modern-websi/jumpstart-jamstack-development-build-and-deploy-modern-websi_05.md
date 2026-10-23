# 2 Sanity 简介

在本书中，我们将构建一个新闻和活动网站，并使用 Sanity.io 作为结构化内容框架。我们将探讨如何通过添加自定义字段、为字段定义验证规则以及自定义 Sanity Studio 来创建和管理内容和新型内容。我们还将学习如何通过 Graph-Relational Object Queries（GROQ）Sanity.io 查询语言和 GraphQL 将内容源到前端框架。

我们将使用基于 JavaScript React 的 Gatsby 作为前端框架，并将介绍该框架的组件，展示如何管理路由、页面以及图像、日历、表单、列表等单个组件。

我们将在 Netlify 上托管应用程序，Netlify 是一个用于自动化 Web 项目部署的平台。我们将通过 GitHub 分布式源代码版本控制系统管理代码并将其连接到 Netlify，以便在每次向代码仓库添加新功能时触发自动部署。

本章将涵盖以下主要主题：

- Account setup
- Introduction to Sanity.io
- Creating a project
- Introducing Sanity Manage
- Sanity Studio overview

## Technical requirements

要理解本章，您需要以下内容：

- A GitHub account
- A Sanity Studio account
- A Netlify account

本章的代码可在 https://github.com/PacktPublishing/Jumpstart-Jamstack-Development/tree/chapter_two 获取。

## Account setup

首先需要做的步骤是在 Sanity.io 网站（https://www.sanity.io）上创建账户。Sanity 账户可在项目创建开始时设置。创建账户的三种不同方法是通过 Google 账户、GitHub 账户，或仅使用电子邮件和密码。接下来，我们在 Netlify 网站（https://app.netlify.com/signup）上创建账户。可用于账户创建的方法有四种：GitHub、GitLab、Bitbucket 账户，或电子邮件和密码。我们将使用 GitHub，因为它是最高效的选择。最后，如果尚未创建，请在 GitHub 网站（https://github.com/join）上创建账户。需要用户名、电子邮件地址和密码。

截至撰写时，全部三个服务均采用 Freemium 模式，因此为小型项目提供慷慨的免费计划。

## Introduction to Sanity.io

> "Sanity.io is the platform for structured content."
> - https://www.sanity.io/docs

Sanity.io 是一个数据存储服务，您可以在其中管理内容，并使用 Sanity 的 **Application Programming Interfaces (APIs)**、工具和库。您可以轻松为项目构建集中式内容存储库。

Sanity 的核心是数据存储，其查询语言为 GROQ。

由于 Sanity Studio 是一个开源单页应用程序，因此可以自由修改和主题化。使用 *JavaScript language* 构建，您可以轻松定义内容结构。然后，使用 ReactJS 库，您可以使用自己的 ReactJS 组件扩展 Sanity Studio 的功能。此外，Sanity Studio 还为项目的內容编辑人员提供工作流个性化功能。由于 Sanity Studio 使用 npm，您可以通过创建托管在 `https://www.npmjs.com/` 上的公共包来贡献插件，以扩展其功能。

Sanity 的内容存储在数据存储中并通过其访问，该数据存储可使用 Sanity.io 客户端库或通过 **HyperText Transfer Protocol (HTTP)** API 调用访问。Sanity.io 管理数据存储，该数据存储托管在云端。

### What is GROQ?

**GROQ** 是 Sanity 创建的查询语言。它用于从数据存储中检索信息。GROQ 易于学习且功能强大——例如，可以将不同集合的文档查询到单个响应中。

现在我们对 Sanity.io 有了高层次的概述，我们将创建我们的第一个项目。

## Creating a project

在您成功在 Sanity.io、GitHub 和 Netlify 上创建账户后，我们就可以开始使用 Sanity.io **Blog with Gatsby** 起始项目进行项目创建。

Sanity.io 项目创建的 **Uniform Resource Locator (URL)** 为 `https://www.sanity.io/create`。

点击 **Create project** 链接，如以下屏幕截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_36_0.png)

创建我们第一个项目需要完成三个步骤，如下所示：

1. 使用上述方法之一登录您的 Sanity.io 账户，并添加 **Project title**。Sanity 需要此部分为您创建一个新的 Sanity 项目。我们可以将 **Project title** 设置为 `<Your Name> hands-on Jamstack`。以下屏幕截图显示了此示例：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_37_0.png)

Figure 2.2 – Connect to Sanity.io

Sanity 将为项目创建一个存储库。点击 Sign In 按钮连接到您的 GitHub 账户并将存储库重命名为 your-name-hands-on-Jamstack。

Note
名称中不能有空格。

可选地，您可以将其设置为私有存储库。每次向远程存储库推送 git 时，网站将自动重新部署，如以下屏幕截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_37_1.png)

Figure 2.3 – Connect to GitHub

Sanity 将自动将您的应用程序部署到 Netlify 并使其可供网络访问。

2. 点击 **Connect** 按钮连接到您的 Netlify 账户，我们就可以开始了！您应该会看到以下消息：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_38_0.png)

Figure 2.4 – Connect to Netlify

3. 现在，按 **Create project** 按钮，如以下屏幕截图所示，项目将在短时间内设置完成：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_38_1.png)

Figure 2.5 – Create project

项目创建后，Sanity 将向 Netlify 部署两个 Web 应用程序。

第一个 Web 应用程序是由 Gatsby 驱动的前端网站，连接到 Sanity 主持的后端。

URL 结构格式如下：

```
`https://website-name.netlify.com`
```

您可以看到它采用项目名称加上 Netlify 域名的形式。

另一个 Web 应用程序是 Sanity Studio，我们可以在其中管理内容。URL 结构格式如下：

```
`https://website-name-studio.netlify.com`
```

您可以看到它采用项目名称、连字符、单词 studio，然后是 Netlify 域名的形式。

让我们开始探索 Sanity。

## Introducing Sanity Manage

让我们先看看我们刚刚创建的 Sanity.io 项目。通过点击 Sanity.io 网站上的 Log in 链接登录您的 Sanity.io 账户，如以下屏幕截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_39_0.png)

Figure 2.6 – Log in link

登录后，您应该能够看到您的项目列表。从顶部菜单，您还将有创建新团队的选项以及创建新项目的选项，如以下屏幕截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_39_1.png)

Figure 2.7 – Sanity.io Manage landing page

点击项目的矩形块，让我们来看看项目页面。

## Sanity.io project pages

从本节，您可以管理您的 Sanity.io 项目。在顶部菜单下，您可以找到项目的 Sanity Studio 详细信息部分，例如 URL、PROJECT ID 和 PLAN，这些信息将在项目的各个页面上始终可见，如以下屏幕截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_39_2.png)

Figure 2.8 – Sanity menu header

在此顶部菜单下，有一个二级菜单，其中包含项目的各个方面，如以下屏幕截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_39_3.png)

Figure 2.9 – Second-level menu

有五个部分可供探索。让我们逐一查看。

## Usage

在本页部分，显示了该项目的资源使用情况，以 **kilobytes (KB)**、**megabytes (MB)** 或 **gigabytes (GB)** 为单位的带宽。

目前有七个图表可用，数据如下：

- **Resource Usage:**
  **Document used**（甜甜圈图）
  **Assets used**（甜甜圈图）
- **Usage this month:**
  **Bandwidth**（面积图）
  **API Requests**（面积图）
  **API CDN Requests**（面积图）
- **Asset and document history:**
  **Documents**（面积图）
  **Assets**（面积图）

带宽、文档和请求统计数据每小时更新一次。资产统计数据每天更新一次。

## Members

在 **Members** 页面上，显示当前项目的所有成员。在这里，用户可以根据其角色邀请和删除成员。添加新成员时，他们可以选择此成员的访问级别类型。

可以设置的权限有三种，如下所示：

- **Administrator**：具有此角色的用户可以完全管理当前项目及其各方面。
- **Read + Write**：具有此角色的用户可以读取和写入当前项目的所有数据集。
- **Read**：具有此角色的用户只能读取当前项目的所有数据集。





## 数据集

在本部分中，你可以查看和删除数据集。数据集是数据的存储位置。一个项目可以根据需要拥有多个数据集。*开发者*（Developer）账户仅限于两个账户。

### 设置

在本部分中，你可以管理当前项目的**常规设置**和**API**。

**常规设置**包括以下内容：

- **组织**
- **项目名称**
- **项目颜色**
- **自定义 Studio URL**

你还可以禁用或删除项目。当你禁用项目时，可以随时重新启用，不会丢失数据。

当你删除项目时，与该项目相关的所有文档、资源和用户都将被移除。**请注意，此操作无法撤销。**

在本部分中，你还可以查看和修改当前套餐。你可以根据自己的需求选择其他套餐。不过，对于本项目来说，免费套餐*开发者*（Developer）已绰绰有余，最多支持三个用户。

接下来，让我们在以下小节中探索**API**设置。

### CORS Origins

在本小节中，你可以添加和移除能够连接项目 API 的主机。`localhost:3333` 主机是自动添加的。`localhost:3333` 是本地开发环境的 URL。托管在 Netlify 上的 Gatsby 应用 URL 和 Sanity Studio 应用 URL 也会自动添加到 **CORS Origins** 列表中。

这样，我们就允许本地开发环境和托管在 Netlify 上的应用访问我们的 Sanity API。

每当我们想让一个新应用与 Sanity API 通信时，只需将该应用的 URL 添加到我们的 **CORS Origins** 列表中，如下所示：

- **Token**：令牌用于机器人查询项目 API。
- **Webhooks**：当某个操作被触发时，就会调用 Webhooks。例如，内容发生变化时，我们可以调用给定的 URL 执行操作，例如发送通知。

既然我们已经知道如何更改应用程序的配置，接下来让我们看看如何通过 Sanity Studio 添加内容。

## Sanity Studio 概述

入门项目是一个博客网站，用户可以添加以下内容：

- 博客文章
- 作者
- 分类

在本书中，我们将通过创建一个新闻和活动网站来扩展此功能，添加新的内容类型和新的字段类型。

我们将通过 Sanity Studio 管理应用程序设置和内容。

在顶级菜单下方的 Sanity Studio 详细信息部分中，单击 **Studio** 链接。

第一个屏幕包含 Sanity Studio 的 **Dashboard**（仪表板），其中有一些有用的小部件，我们现在就来探索它们。

### 导航

从 **导航**菜单中，可以访问 Sanity Studio 的所有部分，如下面的屏幕截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_42_0.png)

单击铅笔标志将打开一个弹出窗口，你可以在其中选择添加**博客文章**、**作者**或**分类**。

从 **搜索**字段中，你可以查找任何**博客文章**、**作者**和**分类**。

**Dashboard** 和 **Desk** 是应用程序的两个主要部分。让我们详细讨论这些内容。

### Dashboard

在 **Dashboard** 部分中，我们有分块显示的快捷方式，链接到 Sanity Studio 中包含的所有部分。

### 编辑你的内容

通过此快捷方式，用户可以访问 **Desk** 部分的子页面，例如：

- **设置**
- **博客文章**
- **作者**
- **分类**

### Netlify 站点

此块列出了为项目创建的 Netlify 站点。我们可以单击**查看**或**管理**来对我们的应用执行操作。如果任何网站内容发生更改，则可以单击**部署**按钮触发部署。

**Sanity Studio** 是我们当前使用的应用程序，**博客网站**（Blog Website）是我们的博客应用程序。

### 项目信息

此块列出了与 Sanity 项目相关的所有信息，如下所示：

- 项目 ID
- 数据集
- 前端
- GROQ
- GraphQL
- GitHub 仓库

### 项目用户

与 **Sanity Manage** 页面的 **团队**部分类似，**项目用户**列出了当前项目的所有用户。

### 最近博客文章

如块标题所示，这里将显示最近五篇博客文章的列表。单击每个博客文章标题将带你进入该特定博客文章的**编辑**页面。

### Desk

在 Sanity Studio 的 **Desk** 部分中，可以管理设置和内容。

### 设置

应用程序带有一些基本设置，我们将在本书中扩展这些设置。

基本设置包括以下内容：

- **标题**
- **描述**
- **关键字**
- **作者**

### 博客文章

**博客文章**是由作者创建的一段内容。它带有一些预配置的字段，例如：

- **标题**
- **Slug**
- **发布于**
- **主图**
- **摘要**
- **作者**
- **分类**
- **正文**

### 作者

**作者**是内容（例如博客文章）的创建者。与**博客文章**类似，此部分具有预配置的字段，例如：

- **姓名**
- **Slug**
- **图像**
- **个人简介**

### 分类

**分类**用于对内容进行分类。分类也可以用于在搜索时过滤内容。

与**博客文章**和**作者**部分一样，它有一些预定义字段，即**标题**和**描述**。

## 总结

在本章中，我们了解了 Sanity.io。通过 Sanity 的 **Create** 工具，我们使用 Sanity 用户界面创建了一个托管在 Netlify 上的新应用程序，并将代码库托管在 GitHub 上。我们探索了如何管理 Sanity 应用程序以及 Sanity Studio 的 **Dashboard** 功能。

在下一章中，我们将探索示例内容，并学习如何使用 Sanity Studio 的 **Desk** 删除、修改和创建新内容。
