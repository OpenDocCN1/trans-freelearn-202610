## Contributors

### About the authors

**Ben Shaw** is a software engineer based in Auckland, New Zealand. He has worked as a developer for over 16 years and has been building websites with Django since 2007. In that time, his experience has helped many different types of companies, ranging in size from start-ups to large enterprises. He is also interested in machine learning, data science, automating deployments, and DevOps. When not programming, Ben enjoys outdoor sports and spending time with his partner and son.

**Saurabh Badhwar** is an infrastructure engineer who works on building tools and frameworks that enhance developer productivity. A major part of his work involves using Python to develop services that scale to thousands of concurrent users. He is currently employed at LinkedIn and works on infrastructure performance tools and services.

**Chris Guest** is based in Melbourne and started programming in Python 24 years ago when it was an obscure academic language. He has since used his Python knowledge in the publishing, hospitality, medical, academic, and financial sectors. Throughout his career, he has worked with many Python web development frameworks, including Zope, TurboGears, web2py, and Flask, although he still prefers Django.

**Bharath Chandra K S** is a passionate software developer with over 14 years of experience in the industry, currently residing in Sydney, Australia. He specializes in Python stack development, including frameworks such as Flask and Django, and has extensive experience with both monolithic and micro-service architectures. Bharath has developed various public-facing applications and data processing backend systems. When not creating software, he enjoys cooking delicious food.

*I am grateful to my family for their constant support while working on this book. Thanks to my wife, Sneha, daughter, Sharanya, parents, Manjula and Dr. Srikanta Rao, brother, Dr. Sharath, in-laws, Sudha and Ramesh. Special thanks to my grandfather, Javagal Nagesh Bhat, whose inspiration and guidance have been instrumental in my academic journey.*

### About the reviewer

While in college in the mid ’80s, **Wayne Tooley** began programming as part of his program. In the early ’90s, he began a career in IT and wrote software utilities to assist with various tasks. After 10 years working in large corporations, Wayne began a freelancing career offering business solutions for business as well as multimedia production.

## 前言

你想在不花费数小时编写样板代码的情况下，开发出可靠且安全、脱颖而出的应用程序吗？你选择信任 Django 框架是正确的，而本书会告诉你原因。Django 常被称为“开箱即用”的 Web 开发框架，它自带构建独立应用程序所需的所有核心功能。《使用 Django 进行 Web 开发》将带你了解所有基本概念，并帮助你探索其构建使用 Python 的真实世界应用程序的能力。在整本书中，你将通过构建一个名为 Bookr 的网站来掌握 Django 的主要功能——一个用于书评的存储库。这个端到端的案例研究被拆分为一系列小项目，以练习和活动的形式呈现，让你以一种愉快且可实现的方式挑战自己。随着学习的深入，你将获得各种实用技能，包括如何提供静态文件以向应用程序添加 CSS、JavaScript 和图像，如何实现表单以接受用户输入，以及如何管理会话以确保可靠的用户体验。你将涵盖真实世界 Web 应用程序开发周期中的日常任务。到本书结束时，你将拥有创造性地开发和部署自己的项目的技能和信心。

### 本书适合谁

本书适合希望使用 Django 框架提升 Web 开发技能的程序员。为了充分理解本书中解释的概念，假设读者具备 Python 编程的基础知识，以及对 JavaScript、HTML 和 CSS 的熟悉程度。

### 本书涵盖内容

- *第 1 章，Django 简介*，首先几乎立即设置一个 Django 项目。你将学习如何启动 Django 项目、响应 Web 请求以及使用 HTML 模板。
- *第 2 章，模型和迁移*，介绍了 Django 数据模型，即将数据持久化到 SQL 数据库的方法。
- *第 3 章，URL 映射、视图和模板*，在 *第 1 章，Django 简介* 中介绍的技术基础上进行构建，并更深入地解释如何将 Web 请求路由到 Python 代码并渲染 HTML 模板。
- *第 4 章，Django 管理简介*，展示如何使用 Django 内置的管理 GUI 来创建、更新和删除模型存储的数据。
- *第 5 章，提供静态文件*，解释如何通过样式和图像增强你的网站，以及 Django 如何使管理这些文件变得更容易。
- *第 6 章，表单*，展示如何通过使用 Django 的 Forms 模块从网站收集用户输入。
- *第 7 章，高级表单验证和模型表单*，在 *第 6 章，表单* 的基础上，通过添加更高级的验证逻辑使你的表单更加强大。
- *第 8 章，媒体提供和文件上传*，展示如何通过允许用户上传文件并使用 Django 提供它们来进一步增强网站。
- *第 9 章，会话和身份验证*，介绍了 Django 会话并展示如何使用它来存储用户数据和验证用户。
- *第 10 章，高级 Django 管理和自定义*，从 *第 4 章，Django 管理简介* 继续。现在你对 Django 有了更多了解，可以使用高级功能自定义 Django 管理。
- *第 11 章，高级模板和基于类的视图*，让你了解如何使用 Django 的一些高级模板功能和类来减少需要编写的代码量。
- *第 12 章，构建 REST API*，介绍如何在 Django 中添加 REST API，以便从不同应用程序以编程方式访问你的数据。
- *第 13 章，生成 CSV、PDF 和其他二进制文件*，进一步扩展 Django 的功能，展示如何使用它生成不仅仅是 HTML 的内容。
- *第 14 章，测试你的 Django 应用*，是真实世界开发的重要组成部分。本章展示如何使用 Django 和 Python 测试框架来验证你的代码。
- *第 15 章，Django 第三方库*，让你接触到众多社区构建的 Django 库，展示如何使用现有的第三方代码快速为项目添加功能。
- *第 16 章，在 Django 中使用前端 JavaScript 库*，通过集成 React 和在 *第 12 章，构建 REST API* 中创建的 REST API 为你的网站带来交互性。
- *第 17 章，部署 Django 应用（第 1 部分 – 服务器设置）*，开始部署应用程序的过程，通过设置自己的服务器。这是一个附赠章节，可从本书的 GitHub 仓库下载。
- *第 18 章，部署 Django 应用（第 2 部分 – 配置和代码部署）*，通过展示如何将项目部署到虚拟服务器来完成项目。这也是一个附赠章节，可从本书的 GitHub 仓库下载。





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
