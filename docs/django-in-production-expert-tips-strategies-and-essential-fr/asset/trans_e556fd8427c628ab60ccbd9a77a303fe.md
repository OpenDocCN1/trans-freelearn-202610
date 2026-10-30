

2. 搞定！我们会直接把免费的 PDF 和其他福利发送到你的邮箱

# 第一部分 – 使用 Django 和 DRF 构建现代 Web 应用

在本书的第一部分，我们将概述如何使用 Django 和 `Django Rest Framework` (`DRF`) 创建现代 Web 应用。我们可以预期学习与 Django ORM 和 DRF 序列化器相关的所有概念，这些概念对于构建任何现代 Web 应用至关重要。Django 管理后台和身份验证是 Django 最受赞赏的功能之一。我们将学习在生产环境中使用 Django 和 DRF 之前，开发者应该知道的所有最佳实践。

本部分包含以下章节：

*   [*第 1 章*](#B18867_01.xhtml#_idTextAnchor014)，*使用 DRF 设置 Django*
*   [*第 2 章*](#B18867_02.xhtml#_idTextAnchor033)，*探索 Django ORM、模型和迁移*
*   [*第 3 章*](#B18867_03.xhtml#_idTextAnchor067)，*使用 DRF 序列化数据*
*   [*第 4 章*](#B18867_04.xhtml#_idTextAnchor102)，*探索 Django 管理后台和管理命令*
*   [*第 5 章*](#B18867_05.xhtml#_idTextAnchor126)，*掌握 Django 身份验证和授权*

# 使用 DRF 设置 Django

在 2003 年，`Django` 项目由来自报纸网络运营公司 *World Online* 的开发者 *Adrian Holovaty* 和 *Simon Willison* 启动，并在 2005 年夏季开源并首次发布。当 Django 最初构建时，世界仍在使用拨号调制解调器互联网连接，移动设备尚未普及，智能手机尚未面世，人们通过台式机和笔记本电脑访问网页。Django 是那个时代构建 Web 应用所需所有功能的完美框架。

在过去二十年里，技术发生了极大演变：

*   我们已从拨号互联网连接转向 4G/5G 互联网连接
*   2022 年，55% 的全球互联网流量来自移动设备（[https://radar.cloudflare.com/](https://radar.cloudflare.com/)）

在本書中，我们将看到如何使用 Django 构建现代 Web 应用，并深入探讨开发者创建可扩展的初创公司 Web 应用所需了解的核心概念。为初创公司构建产品的开发者不仅仅是编写 Django 代码的普通开发者；他们需要开发代码、为业务逻辑编写测试、将应用部署到网络，并最终持续监控已部署的服务。在这里，我们将学习使用 Django 开发 Web 应用是多么容易，以及行业开发者，特别是初创公司开发者遵循的最佳实践，以使他们的开发周期更简单、更快。

在本章中，我们将学习 Django 的基础知识以及如何设置 Django 项目并组织项目文件夹。由于我们将在本书的大部分内容中主要使用 RESTful API，因此我们将学习 REST API 的约定以及使用 `Django Rest Framework` (`DRF`) 设置 Django 项目以创建 RESTful API 的核心。我们还将重点关注 API 版本控制以及如何使用 DRF 实现版本控制。DRF 为我们提供了创建函数式视图和基于类的视图的灵活性；我们将在本章中学习它们，以及它们的优缺点。

我们将涵盖以下主题：

*   为什么选择 Django？
*   使用 Django 和 DRF 创建“Hello World” Web 应用
*   使用 DRF 创建 RESTful 端点
*   使用 DRF 处理视图
*   介绍 API 开发工具

## 技术要求

在本章中，我们将进行基础项目设置并编写我们的第一个 Hello World 应用。虽然本书面向已经知道如何编写基础 Web 应用的开发者，但任何具备良好编程技能的人都可以拿起本书学习如何创建可扩展的 Django Web 应用。以下是你应具备的技能，以便跟上本章内容：

*   良好的 Python 编程知识，并熟悉包以及在 Python 中编写循环、条件语句、函数和类。
*   对 Web 应用的工作原理有基本了解，并且之前编写过某种形式的 API 或 Web 应用。
*   虽然我们将尝试从头开始涵盖大部分概念，但具备 Django 的基础知识会更好。Django Girls 教程是学习基础的好资源：[https://tutorial.djangogirls.org/en/](https://tutorial.djangogirls.org/en/)。

你可以在本书的 GitHub 仓库中找到本章的代码：[https://github.com/PacktPublishing/Django-in-Production/tree/main/Chapter01](https://github.com/PacktPublishing/Django-in-Production/tree/main/Chapter01)。

重要提示

如果你对本章或其他章节中提到的任何主题有任何疑问，欢迎创建 GitHub issue 并说明所有相关信息（[https://github.com/PacktPublishing/Django-in-Production/issues](https://github.com/PacktPublishing/Django-in-Production/issues)），或加入我们的 *Django in Production* Discord 频道提问。以下是 Discord 服务器的邀请链接，你可以直接联系我：[https://discord.gg/FCrGUfmDyP](https://discord.gg/FCrGUfmDyP)。

## 为什么选择 Django？

Django 是一个基于 Python 的 Web 框架，Python 是目前最受欢迎且易于学习的编程语言之一。由于 Python 是数据科学和人工智能/机器学习的首选语言，开发者可以轻松学习 Django，而无需为构建 Web 应用再学习一种额外语言。

Django 的口号“*Django – The web framework for perfectionists with deadlines*”证明了其对更快、更高效开发的承诺，这一点进一步强调了其自带电池原则，即所有基础和广泛使用的功能都随框架一起提供，而不是我们必须安装额外的包。这使 Django 相对于其他框架，如 Flask 具有额外优势。

### Django 自带了什么？

Django 在过去十年中不断发展，目前已是 5.x 版本，拥有一些令人期待的新功能，例如异步支持。然而，Django 的核心模块仍然相同，遵循相同的原则。当新开发者想要学习 Django、组织想要为新项目选择 Django，或资源有限的初创公司想要为其技术栈选择完美框架时，他们都想知道为什么应该学习 Django。为回答这个问题，我们将了解更多关于 Django 的功能。

让我们看看 Django 开箱即提供的显著功能：

*   在任何组织中，执行速度对于产品的成功非常重要。Django 的设计旨在帮助开发者以惊人的速度将应用从概念阶段带到产品阶段。
*   Django 负责用户身份验证、内容管理、站点地图、RSS 订阅以及开发者在任何框架中寻找的许多其他基础 Web 任务。
*   安全是任何组织都非常关注的问题，Django 帮助开发者避免常见的<|start|>安全陷阱。
*   Mozilla、Instagram、Disqus 和 Pinterest 等网站都使用 Django 构建，这使得 Django 成为经实战检验且可扩展的框架。
*   Django 的多功能框架可用于不同的目的，从内容管理系统到社交网络再到科学计算平台。

