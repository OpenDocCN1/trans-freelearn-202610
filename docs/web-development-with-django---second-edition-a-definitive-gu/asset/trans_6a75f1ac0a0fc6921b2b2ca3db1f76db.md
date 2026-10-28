

# 如何充分利用本书

| 书中涵盖的软件/硬件 | 操作系统要求 |
|---|---|
| Python 3.8 | Windows, macOS, 或 Linux |
| Django 4.0 |  |
| React 16 |  |

如果你使用的是本书的数字版，我们建议你自己输入代码或从本书的 GitHub 仓库访问代码（链接在下一节提供）。这样做将有助于避免与复制和粘贴代码相关的任何潜在错误。

## 下载示例代码文件

你可以从 GitHub 在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition 下载本书的示例代码文件。如果代码有更新，将在 GitHub 仓库中更新。

我们还有来自我们丰富的图书和视频目录的其他代码包，可在 https://github.com/PacktPublishing/ 获取。快来看看吧！

## 下载彩色图片

我们还提供了一份 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。你可以在这里下载：https://packt.link/5pZtF。

## 使用的约定

本书通篇使用了若干文本约定。

正文中的代码：表示正文中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。举例说明：“首先，在 filter_demo/templatetags 目录下创建一个名为 inclusion_tag.py 的新文件。”

代码块设置如下：

```
from django.http import HttpResponse
from django.views import View

class IndexView(View):
    def get(self, request):
        return HttpResponse("Hey there!")
```

当我们希望引起你对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
from django.shortcuts import render

def index(request):
    names = "john,doe,mark,swain"
    return render(request, "index.html", {'names': names})
```

任何命令行输入或输出写成如下形式：

```
POST /form-submit HTTP/1.1
Host: www.example.com
Content-Length: 31
Content-Type: application/x-www-form-urlencoded

first_name=Joe&last_name=Bloggs
```

**粗体**：表示新术语、重要词语或你在屏幕上看到的词语。例如，菜单或对话框中的词语以**粗体**显示。举例说明：“填写数据并点击 **Save record** 后，Django 将数据保存到数据库。”

**提示或重要说明**
显示如下。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈**：如果您对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中提及书名。

**勘误**：尽管我们已尽一切努力确保内容的准确性，但错误难免。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 www.packtpub.com/support/errata 并填写表格。

## 下载本书的免费 PDF 副本

感谢您购买本书！

您喜欢随时随地阅读，但又无法随身携带印刷书籍吗？  
您购买的电子书与您选择的设备不兼容？

不用担心，现在每本 Packt 书籍都免费提供一份无 DRM 的 PDF 版本。

在任何地方、任何地点、在任何设备上阅读。直接从您喜爱的专业书籍中搜索、复制和粘贴代码到您的应用程序中。

福利还不止于此，您还可以每天通过电子邮件获得独家折扣、新闻通讯和精彩免费内容

按照以下简单步骤获取福利：

- 1. 扫描二维码或访问以下链接

![](img/4840a43efd335abd10b93708225692d6_22_0.png)

https://packt.link/free-ebook/9781803230603

- 2. 提交您的购买证明

- 3. 就这样！我们会直接将免费 PDF 和其他福利发送到您的电子邮件

# Django 简介

“完美主义者的截止日期之选的 Web 框架。” 这句标语恰如其分地描述了 Django，这是一个已经存在十多年之久的框架。它经过实战检验并被广泛使用，每天都有越来越多的人使用它。所有这些可能会让你认为 Django 已经过时且不再相关。恰恰相反——它的长寿证明了其 API 的可靠性和一致性，甚至那些在 2007 年学习 Django v1.0 的人今天大多仍可为 Django 4 编写相同的代码。Django 仍在积极开发中，每月都会发布错误修复和安全补丁。

与编写它的语言 Python 一样，Django 易于学习，同时又足够强大和灵活，能够随着你的需求而成长。它是一个“开箱即用”的框架——换句话说，你不需要查找和安装许多其他库或组件即可让你的应用程序运行起来。其他框架，如 Flask 或 Pylons，需要手动安装第三方框架用于数据库连接或模板渲染。相比之下，Django 内置了数据库查询、URL 映射和模板渲染的支持（我们很快会详细介绍这些含义）。然而，Django 易于使用并不意味着它功能有限。Django 被许多大型网站使用，包括 Disqus (https://disqus.com/)、Instagram (https://www.instagram.com/)、Mozilla (https://www.mozilla.org/)、Pinterest (https://www.pinterest.com/)、OpenStack (https://www.openstack.org/) 和国家地理 (http://www.nationalgeographic.com/)。

Django 在 Web 中处于什么位置？谈到 Web 框架时，你可能会想到 ReactJS、Angular 或 Vue 等前端 JavaScript 框架。这些框架用于增强或为已生成的网页添加交互性。Django 位于这些工具之下的一层，负责路由 URL、从数据库获取数据、渲染模板以及处理用户的表单输入。然而，这并不意味着你必须二选一；JavaScript 框架可用于增强 Django 的输出或与 Django 生成的 REST API 交互。

在本書中，我们将使用专业 Django 开发者每天使用的相同方法构建一个 Django 项目。该应用程序名为 Bookr，允许你浏览和添加书籍及书评。本工作坊分为四个部分。在第一部分，我们将从搭建 Django 应用的基础开始，快速构建一些页面，并使用 Django 开发服务器提供服务。你将能够使用 Django 管理站点向数据库添加数据。

本章向你介绍 Django 及其在 Web 开发中的作用。你将首先了解 **model-view-template** 范式的工作原理以及 Django 如何处理 HTTP **请求**和**响应**。掌握基本概念后，你将创建你的第一个 Django 项目 Bookr，这是一个用于添加、查看和管理书评的应用程序。这是一个你将在本书中不断增强和添加功能的应用程序。你将然后学习 `manage.py` 命令（用于编排 Django 操作）。你将使用此命令启动 Django 开发服务器并测试迄今为止编写的代码是否按预期工作。你还将学习如何使用 **PyCharm**，这是一款流行的 Python IDE，你将在本书中全程使用它。你将使用它编写代码以向 Web 浏览器返回**响应**。最后，你将学习如何使用 PyCharm 的调试器来排查代码中的问题。到本章结束时，你将具备使用 Django 开始创建项目的必要技能。

本章将涵盖以下主题：

- 搭建 Django 项目和应用
- 理解 model-view-template 范式
- 探索 Django 项目结构
- 介绍 Django 视图
- 探索 URL 映射细节
- 探索 Django 设置
- 在应用目录中找到 HTML 模板
- 调试和处理错误
- 活动 1.01 – 创建站点欢迎屏幕
- 活动 1.02 – 书籍搜索脚手架

到本书结束时，你将拥有足够的经验来从头到尾设计和构建自己的 Django 项目。

## 技术要求

在本書中，你将编写代码。如果需要参考本章的完整代码，你可以在此 GitHub 仓库中找到：https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter01。

## 搭建 Django 项目和应用

在深入探讨 Django 范式和 HTTP 请求背后的理论之前，我们将向你展示启动一个 Django 项目是多么容易。在本节和练习之后，你将创建一个 Django 项目，使用浏览器向其发出请求，并看到响应。

