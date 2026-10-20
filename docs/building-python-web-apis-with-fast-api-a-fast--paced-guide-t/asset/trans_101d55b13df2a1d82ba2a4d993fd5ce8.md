

*第 2 章，FastAPI 中的路由*，详细介绍了使用 FastAPI 中的路由系统创建端点的过程。路由系统的组件，包括请求体和路径参数，也在讨论中，并与使用 pydantic 类验证它们一起讨论。

*第 3 章，响应模型和错误处理*，介绍了 FastAPI 中的响应、响应建模、错误处理和状态码。

*第 4 章，FastAPI 中的模板*，讨论了如何使用模板创建视图并从 API 渲染响应。

*第 5 章，FastAPI 应用的结构*，介绍了应用的结构化，并简要介绍了将在下一章中构建的应用。

*第 6 章，连接到数据库*，讨论了两类数据库（SQL 和 NoSQL），并演示了如何将 FastAPI 应用连接到其中任何一个。我们将介绍如何使用 SQLModel 连接和使用 SQL 数据库，以及如何使用对象文档映射器 Beanie 与 MongoDB 协作。

*第 7 章，保护 FastAPI 应用*，讨论了保护应用所包含的内容——授权和身份验证、实现身份验证以及限制对应用端点的访问。

*第 8 章，测试 FastAPI 应用*，解释了什么是测试以及如何测试我们的 API 端点。

*第 9 章，部署 FastAPI 应用*，讨论了部署 FastAPI 应用所涉及的步骤。

## 为了充分利用本书

您需要在计算机上安装最新版本的 Python。您还需要熟悉 Python 编程语言才能充分利用本书。

| 书中涵盖的软件/硬件 | 操作系统要求 |
|---|---|
| Python 3.10 | Windows、macOS 或 Linux |
| Git 2.36.0 | Windows、macOS 或 Linux |

如果您使用的是本书的数字版，我们建议您自己输入代码或从本书的 GitHub 仓库访问代码（链接在下一节中提供）。这样做将有助于您避免与复制和粘贴代码相关的任何潜在错误。

## 下载示例代码文件

您可以从 GitHub 下载本书的示例代码文件，网址为 https://github.com/PacktPublishing/Building-Python-Web-APIs-with-FastAPI。如果代码有更新，将在 GitHub 仓库中更新。

我们还有来自我们丰富的图书和视频目录的其他代码包，可在 https://github.com/PacktPublishing/ 获取。快来看看吧！

## 下载彩色图片

我们还提供了一份 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。您可以在此处下载：https://packt.link/qqhpc。

## 使用的约定

本书通篇使用了多种文本约定。

正文中的代码：表示正文中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。示例：“要切换回原始主分支，我们运行 git checkout main。”

代码块如下所示：

```
from fastapi import FastAPI
from routes.user import user_router

import uvicorn
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
from pydantic import BaseModel
from typing import List

class Event(BaseModel):
    id: int
    title: str
    image: str
    description: str
```

```
    tags: List[str]
    location: str
```

任何命令行输入或输出如下所示：

```
$ git add hello.txt
$ git commit -m "Initial commit"
```

**粗体**：表示新术语、重要词语或您在屏幕上看到的词语。例如，菜单或对话框中的词语以**粗体**显示。示例：“如前面的模型图所示，每个用户将拥有一个 **Events** 字段，这是一个他们拥有的事件列表。”

**提示或重要说明**
显示如下。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果您对本书的任何方面有疑问，请发送电子邮件至 customercare@packtpub.com，并在邮件主题中提及书名。

**勘误：** 尽管我们已尽一切努力确保内容的准确性，但错误难免。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 www.packtpub.com/support/errata 并填写表格。

**盗版：** 如果您在互联网上发现我们作品的任何形式的非法副本，我们将不胜感激您提供位置地址或网站名称。请发送邮件至 copyright@packt.com 并附上材料链接。

**如果您有兴趣成为作者：** 如果您在某个主题上具有专业知识，并且有兴趣撰写或为一本书做出贡献，请访问 authors.packtpub.com。

## 分享您的想法

一旦您阅读了 *Building Python Web APIs with FastAPI*，我们很想听听您的想法！请点击此处直接前往此书在 Amazon 的评论页面并分享您的反馈。

您的评论对我们和技术社区都很重要，并将帮助我们确保提供优质内容。

## 第 1 部分：
FastAPI 简介

完成本部分后，您将对 FastAPI 有实质性的理解，包括路由、文件处理、错误处理、构建响应模型和模板。本部分首先介绍本书中将使用的技术，然后再介绍 FastAPI 框架的基础知识。

本部分包含以下章节：

- 第 1 章，开始使用 FastAPI
- 第 2 章，FastAPI 中的路由
- 第 3 章，响应模型和错误处理
- 第 4 章，FastAPI 中的模板

## 1
开始使用
FastAPI

FastAPI 是我们将在本书中使用的 Python Web 框架。它是一个快速、轻量级的现代 API，与其他 Python Web 框架（如 Flask 和 Django）相比，具有更易于学习的曲线。FastAPI 相对较新，但拥有不断增长的社区。它被广泛用于构建 Web API 和部署机器学习模型。

在第一章中，您将学习如何设置开发环境并构建您的第一个 FastAPI 应用。您将首先学习 Git 的基础知识——一个版本控制系统——以便您具备存储、跟踪和检索文件更改的知识，因为您构建应用。您还将学习如何使用 pip 在 Python 中处理包，如何使用 Virtualenv 创建隔离的开发环境，以及 Docker 的基础知识。最后，您将通过构建一个简单的 Hello World 应用来了解 FastAPI 的基础知识。

理解前面提到的技术对于构建完整的 FastAPI 应用是必需的。它也作为您当前技能集的补充。

完成本章后，您将能够设置和使用 Git，使用 pip 安装和管理包，使用 Virtualenv 创建隔离的开发环境，使用 Docker，最重要的是，搭建一个 FastAPI 应用。

本章涵盖以下主题：

- Git 基础
- 使用 Virtualenv 创建隔离的开发环境
- 使用 pip 进行包管理
- 设置和学习 Docker 基础知识
- 构建简单的 FastAPI 应用

## 技术要求

您可以在 GitHub 上找到本章的代码文件，网址为 https://github.com/PacktPublishing/Building-Python-Web-APIs-with-FastAPI/tree/main/ch01

## Git 基础

Git 是一个版本控制系统，使开发人员能够记录、跟踪并恢复到文件的早期版本。它是一个去中心化且轻量级的工具，可以安装在任何操作系统上。

您将学习如何使用 Git 进行记录保存。随着应用的每一层被构建，都会进行更改，重要的是要记录这些更改。

## 安装 Git

要安装 Git，请访问下载页面 https://git-scm.com/downloads 并为当前操作系统选择下载选项。您将被重定向到有关如何在机器上安装 Git 的说明页面。

