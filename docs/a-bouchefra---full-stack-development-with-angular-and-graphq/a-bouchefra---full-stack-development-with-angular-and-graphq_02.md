## 1 应用程序架构和开发环境

在本书中，我们将开发一个全栈 Web 应用程序，使用 **Angular** 前端、**Node.js** 后端和 **MySQL** 数据库。

因此，在本章中，我们将了解项目的结构以及开发应用程序所需的工具。之后，我们的机器将准备好进行开发。具体来说，我们将安装 Node.js 以及 npm 和 MySQL。

Node.js 对于后端应用程序是必需的，该应用程序运行一个服务器，使用 Express.js（最受欢迎的 Node 框架之一）公开 GraphQL API。Node.js 对于前端应用程序的开发也是必要的。这是因为 Angular 使用官方命令行界面来初始化项目，并在应用程序开发期间生成所需的任何构件。

MySQL 将用于存储我们的数据，但它在代码中不会直接访问。相反，我们将使用名为 **TypeORM** 的 ORM。

在本章中，我们将涵盖以下主题：

- 项目的架构和开发技术
- 安装 MySQL
- 安装和配置 Node.js

### 技术要求

本章需要一台配备操作系统和互联网连接的计算机。由于我使用的是 Ubuntu 系统，本书中的说明将特定于该系统。它们也应该在基于 Debian 的任何其他系统上运行。

### 架构和技术

在本节中，我们将了解应用程序的架构以及在本书中用于开发应用程序前端和后端的各种技术。

让我们先熟悉 **full stack** 架构和 **monorepo** 仓库（也称为 **mono-repositories**）的概念。

### 全栈架构

我们将开发一个包含前端和后端的 Web 应用程序，也称为 **full stack application**。这意味着我们将承担全栈开发者的工作。

全栈开发者是一名 Web 开发者，能够处理 Web 开发项目的前端（客户端）和后端（服务器端）部分的开发。

他们必须精通 HTML、JavaScript 和 CSS 等基本技术，并对 TypeScript、Node.js 以及 MySQL 等数据库管理系统等技术有所了解。这些技术大致可分为两类：

- 用于开发基于浏览器的应用程序的客户端语言、框架和工具，包括 HTML、CSS、JavaScript、TypeScript 和 Angular。
- 服务器端语言和工具，如 Node.js、Python 或 PHP，以及数据库语言，如 SQL。

要构建我们的全栈应用程序，我们需要两个部分：

- **前端**：这是运行在客户端机器的 Web 浏览器中的应用程序。历史上，这通过在服务器上渲染 HTML 页面然后返回给客户端来实现。然而，浏览器现在可以运行功能齐全的 JavaScript 应用程序（也称为 **client-side apps**），它们在客户端浏览器上完成大部分处理，仅依赖服务器提供应用程序的初始文件和数据。简单来说，前端是向用户展示用户界面的部分，用户通过它进行交互。
- **后端**：这是服务器端应用程序，将处理 HTTP 请求，执行一些处理逻辑，并向浏览器返回响应（HTML 和/或 JSON 数据）。

我们将使用 Lerna 等工具，采用 monorepo 方法来组织应用程序的代码。在软件开发的上下文中，monorepo 仅指使用单个源代码仓库（通常使用 Git 进行版本控制）来管理所有应用程序（服务器、客户端和任何共享库）。

让我们从高层次来看我们的应用程序将如何交付到客户端机器上：

1. 首先，客户端将在浏览器地址栏中输入应用程序域名以发出请求。
2. 服务器将拦截请求并处理包含 Angular 应用的 HTML 文档。
3. 客户端浏览器将开始下载运行 Angular 应用程序所需的所有 JavaScript 和 CSS 文件。
4. Angular 应用程序将发送对服务器的任何初始数据请求，例如帖子，并在用户界面中渲染。
5. 用户开始与应用程序交互后，将向服务器发出其他请求。

此过程提供了应用程序运行方式的高层次概述。我们将在下一章中更深入地探讨这些阶段。

在快速讨论了全栈和 monorepo 概念之后，让我们看看我们将用于构建应用程序的技术和工具。

### 开发技术

每个项目都需要使用一套技术和工具。这包括编程语言、命令行界面、库和框架。在我们的全栈项目中，我们将为前端和后端开发使用多种技术。

如今，现代开发涉及在前端和后端使用相同的工具。让我们更详细地了解这些技术和工具。

我们将使用 Angular 的最新版本（撰写时为版本 12）作为前端，Node.js 作为后端，以及一套支持库，包括 Express.js 用于启动 Web 服务器和 TypeORM 用于抽象数据库操作。

本书的主要目标之一是构建一个 GraphQL API，该 API 将由我们的 Express.js 服务器提供，并被我们的 Angular 前端消费。为了抽象运行 Web 服务器所需的所有低级 API，我们将在 Node.js 之上使用 Express.js。

Web 服务器将监听来自客户端浏览器的传入 HTTP 请求，这些请求主要是用于获取数据的 GraphQL 查询和用于创建和更新数据的 mutation。

接下来，我们将了解 Node.js 及其用途。

### 使用 Node.js 在服务器上运行 JavaScript

Node.js 到底是什么？Node.js 是一个免费的开源平台和运行时环境，允许您在服务器上运行 JavaScript，并采用事件驱动、非阻塞输入/输出（I/O）架构。Ryan Dahl 于 2009 年在 Google Chrome 的 JavaScript 引擎（V8 引擎）之上创建了 Node.js。

如果您是一名经验丰富的 JavaScript 开发者，您可能熟悉 JavaScript 作为一种用于创建只能在客户端浏览器中执行的动态网页的编程语言。然而，得益于 Node.js，我们现在可以使用 JavaScript 在客户端（通过浏览器）和服务器上构建 Web 应用。

正如您所期望的，开发者现在可以使用单一编程语言创建完整全栈 Web 应用程序，而无需为服务器使用不同的语言，例如 PHP 或 Python，它们只是众多可用选项中的两个。Node.js 与这些语言一样，可用于开发 Web 应用程序的后端。

这使得前端 JavaScript 开发者无需学习新语言即可开始开发后端应用。



Node.js 的用途远不止于服务端应用；它是一种通用工具，可用于构建各种网络应用，以及在开发者的机器上构建和运行前端桌面工具。例如，Angular CLI 就是一个基于 Node.js 的工具，我们将使用它来开发和操作我们的前端 Angular 项目。

> **重要提示**
> 因为 Node.js 是一个 JavaScript 运行时，因此它也可以与 TypeScript 配合使用，TypeScript 是包含面向对象编程原则和静态类型检查的 JavaScript 超集。在本书中，我们将利用 Node.js 和 TypeScript 来开发后端。

## 使用 npm 安装包

Node.js 拥有庞大的生态系统以及一个名为节点包管理器（npm）的包管理工具。它可用于从一个中央注册表中快速安装包，该注册表包含成千上万由其他组织和开发者构建并发布以解决常见开发问题的包。

得益于庞大的生态系统和 npm 注册表，在尝试解决其他开发者之前已经遇到过的相同开发难题时，你无需重复造轮子。npm 注册表还使使用单个命令安装任何包或库变得简单，包括 Angular 和 Express.js 等知名库。

在我们前端和后端项目中，我们都将使用 npm 来安装必要的工具和库，例如 Angular、Apollo 和 Express.js 等。

## 使用 Express.js 运行 Web 服务器

如前所述，Node.js 是一个平台和运行时环境，它公开了一组我们在构建 Web 应用时很少直接使用的低级 API。与其开发大量复杂的代码或重复造轮子，我们将利用其他开发者创建的某些库。Express.js，如我们之前所述，就是其中之一。那么它到底是什么？

Express.js 是一个流行的 Node.js Web 应用框架，用于开发服务器应用。它无主观偏见、轻量级，并且包含开发 Web 应用和 Web API 所需的所有基本功能。

Express.js 让你无需处理低级 Node.js API 即可创建接收 HTTP 请求并以 HTTP 响应响应的服务器。它还使实现路由、管理静态文件和提供资源变得简单。

> **提示**
> 与 Python 中被认为有主见的流行框架 Django 不同，Express.js 是一个灵活的框架，不强制规定你应如何组织项目。

## GraphQL

现在我们知道了 Node 是什么，并且将使用 Express.js 运行后端服务器，GraphQL 是什么，它在这个技术栈中处于什么位置？

GraphQL 与 SQL 类似；然而，它用于查询 Web API 而非数据库。它是一种 API 规范和查询语言。它还充当运行时，用于响应查询以获取、创建和更新数据。

> **重要提示**
> GraphQL 是 REST 的一种更新替代方案，REST 被开发人员广泛用于创建可被桌面和移动客户端消费的 API。

Facebook 于 2015 年推出 GraphQL，以解决 REST 和其他 API 开发方法的一些不足。

例如，使用 GraphQL，前端应用可以仅向服务器请求其所需的数据。这是因为 GraphQL 允许你使用由字符串和整数等基本内置类型构成的用户定义类型来描述数据的形式。

GraphQL 可与多种网络协议一起使用，其中最常见的是 WebSocket 和 HTTP。

如果你熟悉 SQL，你会认识到它与使用表来定义数据形式的方式相似。如果你熟悉面向对象语言，它也与你构建接口和类来描述现实世界对象的方式相似。

你可以发送类似 JSON 的对象查询，指定你希望服务器通过 HTTP 响应返回的字段。以下是一个查询示例：

```
post {
  id
  content
  date
}
```

如果将此查询提交给定义了带有 ID、content 和 date 属性的 post 类型的 GraphQL 服务器，则相关帖子数据将使用 **解析器函数** 返回。

解析器函数处理数据的解析，执行从数据库检索数据所需的逻辑，并将其返回给请求客户端。

如前所述，GraphQL 是一个标准，不受任何编程语言或框架的约束。许多流行的编程语言，如 Python 和 JavaScript，都有实现。

它也不受任何数据库系统的约束，可以与包含 MySQL 或任何数据库管理系统作为数据库的任何技术栈一起使用。在我们的情况下，我们将与 MySQL 结合使用它。

**Apollo** 是 GraphQL 实现之一。那么它在我们的技术栈中处于什么位置？

## 使用 Apollo 集成前端和后端

我们将使用 Apollo，这是一种面向 JavaScript 的行业标准 GraphQL 实现。它由客户端和服务器部分组成，分别称为 Apollo Client 和 Apollo Server。

Apollo Client 与纯 JavaScript 以及最新的 UI 库和框架（如 React、Angular 和 Vue.js）兼容，而 Apollo Server 与常见的 Node.js 框架（如 Express.js 和 Hapi）兼容。

得益于 Apollo Client 和 Apollo Server，我们可以在应用程序的前端和后端之间简单轻松地交互，从而无需复杂的获取数据逻辑。

因此，在我们的 Angular 前端中，Apollo Client 将提供一个用于发送查询以获取数据以及用于向数据库添加、修改和删除数据的 mutation 的层。Apollo Client 不直接与数据库交互，而是与运行在 Node.js 之上的 Apollo Server 和 Express.js 交互。

## 使用 TypeORM 和 MySQL 保存数据

数据将保存在 MySQL 数据库中。在开发环境中，我们将使用本地安装的 MySQL 服务器，但在生产环境中，你可以使用云托管的关系数据库，例如 **亚马逊关系数据库服务 (Amazon RDS)** 或你选择的任何其他服务。

我们选择 MySQL 作为数据库管理系统，因为它是世界上使用最广泛的开源关系数据库，这意味着大多数开发者都熟悉它，并且可能在之前的某个 Web 项目中使用过它。

它在所有受支持的操作系统上本地安装非常容易。借助 Amazon RDS 和 DigitalOcean 等云服务，在生产环境中设置和扩展也非常简单。

Amazon RDS 提供免费套餐，超出后你将按需付费。它将帮助你专注于应用开发，而非数据库管理操作，如备份、监控、可扩展性和复制。

我们将不会使用 SQL 直接构建表或查询数据。相反，我们将使用对象关系映射器（ORM）来创建数据库表并使用高级编程语言而不是 SQL 来查询、插入和删除数据。在我们的案例中，将使用基于 TypeScript 的 ORM TypeORM。

现在我们已经介绍了后端最后一个主要组件——数据库，让我们来看看我们技术栈的另一个组件：Angular。

## 使用 TypeScript 和 Angular 构建前端

Google 的 Angular 是一个开源客户端框架。它从头开始用 TypeScript 创建，是基于纯 JavaScript 的 Angular.js 的替代品。Angular 与 React 和 Vue 一起，是三大最受欢迎的前端框架之一。它包含构建面向移动和桌面设备的现代前端 Web 应用所需的库。

我们将使用官方 Angular CLI 以基本的文件结构创建项目，并使用模块、组件和服务等抽象来组织我们的客户端 TypeScript 代码。

Angular 开箱即带客户端路由器，允许我们在应用中添加路由和导航。当我们开始采用这些技术时，我们将在接下来的章节中更深入地介绍它们。

我们还将通过利用 Apollo Client 与 Angular 集成，将前端与基于 Apollo Server 构建的 GraphQL 服务器集成。

通过将所有这些工具结合起来，我们将拥有一个包含 Node.js、MySQL、Express.js、TypeORM、Apollo 和 Angular 的技术栈。

现在我们已经介绍了应用架构和技术，让我们开始在开发环境中安装 MySQL 和 Node.js。



## 安装 MySQL

在本节中，我们将学习如何在开发机器上安装 MySQL。 在计算机上安装 MySQL 的说明取决于您的操作系统，但在这里，我们将重点介绍 Ubuntu 的说明。

MySQL 是一款流行的数据库管理系统，由于其安装和配置简单，也适用于本地开发。

安装过程很简单；只需更新系统的软件包索引，安装 mysql-server 软件包，然后执行随附的安全脚本即可。

MySQL 可能已安装在您的开发机器上。如果是这样，您可以跳过此步骤。

> **重要说明**
> 以下步骤仅用于在本地机器上为开发目的设置 MySQL。在生产环境中，您必须遵循适当的指南，尤其是在保护数据库免受攻击方面。您可以使用云服务轻松实现这一点，云服务提供托管数据库，您无需自行管理和保护。

现在，让我们通过运行安装 MySQL Server 的说明开始。打开一个新的命令行界面并运行以下命令：

```
sudo apt-get update
```

```
sudo apt-get upgrade -y
```

这些说明将把系统的软件包索引更新到最新版本。

然后，要安装 MySQL Server，请使用以下命令：

```
sudo apt-get install mysql-server
```

现在我们已经安装了 MySQL Server，让我们学习如何配置它。

## 配置 MySQL Server

安装 MySQL Server 后，您必须执行安全脚本。返回命令行界面并执行以下命令：

```
sudo mysql_secure_installation
```

系统会提示您输入 root 用户密码；输入密码并按 Enter。

接下来，您将看到一系列问题。第一个问题将是 Would you like to set up VALIDATE PASSWORD plugin? 这用于验证密码并提高安全性。它评估密码的强度并帮助用户创建足够安全的密码。由于我们使用的是开发机器，这不是关键，因此回答 N 表示否。下一个问题将要求您为 MySQL root 用户创建密码。输入您选择的密码并确认。

要为以下问题选择默认答案，只需输入 Y 然后按 Enter 即可。

- Remove anonymous users?
- Disallow root login remotely?
- Remove test database and access to it?
- Reload privilege tables now?

这将删除一些匿名用户以及测试数据库及其访问权限，禁用远程 root 登录，并加载权限表以确保所有先前的修改立即生效。

就是这样——完成了！您已成功在运行 Ubuntu 的本地开发机器上安装了 MySQL Server。接下来，您将学习如何验证 MySQL 是否正在运行，以及如果未运行如何启动它。

## 测试 MySQL Server

安装后 MySQL 应该已自动启动。返回命令行界面并执行以下命令：

```
systemctl status mysql.service
```

如果它正在运行，您会看到类似以下内容：

```
mysql.service - MySQL Community Server
  Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled
  Active: active (running) since Tue 2020-12-08 17:15:40 +01; 48min ago
Main PID: 20416 (mysqld)
  Tasks: 29
  CGroup: /system.slice/mysql.service
    └─20416 /usr/sbin/mysqld --daemonize --pid-file=/run/mysqld/mysqld.pid
```

如果 MySQL Server 未运行，请使用以下命令启动它：

```
sudo systemctl start mysql
```

在我们安装 MySQL 并验证其正在运行后，我们将学习如何安装 Node.js，我们的前端和后端应用程序都需要它。

## 安装和配置 Node.js

在安装 MySQL 数据库管理系统后，我们需要安装 Node.js，因为它对于执行服务器代码是必要的。

我们有几种在操作系统上安装 Node.js 的选项：

- Node Version Manager (NVM)，您可以使用它在开发系统上运行各种版本的 Node.js，获取有关可用版本的信息，并通过单个命令安装任何版本
- 操作系统的官方包管理器，例如 Ubuntu 的 APT、macOS 的 Homebrew 或 Windows 的 Chocolatey
- 来自官方网站 https://nodejs.org/en/download/ 的二进制文件，它不仅提供 Windows、macOS 和 Linux 二进制文件，还提供可下载和编译的源代码

如前所述，我们假设您正在运行基于 Debian 的系统，例如 Ubuntu。在本章中，我们将教您如何在 Ubuntu 系统上使用第一种方法。

> **重要说明**
> 如果您未使用 Ubuntu 或基于 Debian 的系统，请访问官方网站 https://nodejs.org/en/download/package-manager/ 并获取在您的操作系统上安装 Node.js 的必要说明。

## 使用 nvm 安装 Node.js

您可以使用 nvm 而不是系统的原生包管理器来安装 Node.js 和 npm。此实用程序不在系统级别运行。相反，它使用主目录中的一个单独文件夹。

这使您可以同时安装多个版本的 Node.js，并在需要时快速在它们之间切换。此外，在安装 nvm 后，您可以使用单个命令快速安装任何版本的 Node.js，无论新旧。

让我们回顾一下步骤：

1. 返回您的命令行界面并运行以下命令以下载 nvm 安装脚本：

```
curl -sL https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh
```

2. 然后，使用以下命令执行脚本：

```
bash install.sh
```

此命令将在 ~/.nvm 文件夹下克隆 nvm 仓库并根据需要更新 ~/.profile 文件。

3. 要开始使用 nvm，只需 source 以下文件或注销然后重新登录即可：

```
source ~/.profile
```

4. 使用此命令，您可以快速检索可安装的可用 Node.js 版本列表：

```
nvm ls-remote
```

5. 我们在本书中使用 v12.22.1，您可以使用以下命令安装它：

```
nvm install v12.22.1
```

这将下载与您的操作系统兼容的安装二进制文件并安装 Node.js 以及 npm。它还将使此版本成为默认版本。

6. 要验证已安装的版本，请使用以下命令：

```
node -v
```

在我们的例子中，终端上会打印 v12.22.1。

有关可用命令的更多信息以及如何使用它们，可以在官方仓库 https://github.com/nvm-sh/nvm 中找到。

nvm 实用程序兼容 Linux、macOS 和 **Windows 子系统 for Linux** (WSL)。您可以使用两个非官方的 Windows 替代方案：

- nvm-windows，可在 https://github.com/coreybutler/nvm-windows 找到
- nodist，可在 https://github.com/marcelklehr/nodist 找到

就是这些——您已将开发机器配置为运行 Node.js，从而可以安装和使用 Node.js 包，例如 Express.js 来构建后端应用程序，以及前端库和工具，例如 Angular CLI。

在整本书中，我们将使用 npm 来安装后端和前端应用程序的依赖项。

## 摘要

在本章中，我们了解了整本书中我们将构建的应用程序的全栈和 monorepo 架构，以及我们将用于构建它的技术。

在介绍架构和技术之后，我们研究了如何在开发机器上安装 MySQL。最后，我们学习了如何安装和设置 Node.js，我们的全栈应用程序的前端和后端都需要它。

本章现已完成！现在我们已经设置了开发环境并安装了 Node.js，我们需要构建一个具有 GraphQL 支持的 Node.js 服务器来实现后端。在下一章中，我们将首先安装和配置 Express.js，然后添加 Apollo Server 并查看如何测试和调试我们的 GraphQL 服务器。


