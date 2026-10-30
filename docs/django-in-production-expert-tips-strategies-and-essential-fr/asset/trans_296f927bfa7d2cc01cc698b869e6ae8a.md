

但 Django 是否仍然相关的问题非常主观。最终，这取决于使用场景。我们知道 Django 是一个优秀的 Web 开发框架，然而，由于全球超过 55% 的互联网流量来自使用 Android 或 IOS 应用的移动设备，你可能会想知道 Django 是否适用于为移动用户构建功能？Django 作为一个独立的框架可能不足以应对当今的现代 Web 开发，因为越来越多的组织正转向 API First 开发，但在与 `Django Rest Framework (DRF)`、`Tastypie` 等框架集成时，Django 就成为开发者的首选框架。

对于时间和资源有限的初创公司来说，选择一个框架变得更加至关重要，这样他们就不必从零开始构建每个功能，而是利用框架来完成大部分繁重的工作。

让我们快速了解一下 Django 使用的框架原则：MVT 框架。

## 什么是 MVT 框架？

我们大多数人都听说过 `MVC` 框架（*Model-View-Controller*），它代表了现代 Web 框架的一种范式，其中我们有以下内容：

*   `Model` 代表数据和业务逻辑层
*   `View` 代表数据如何在 UI/设计布局中向用户呈现
*   `Controller` 根据用户的输入更新模型和/或视图

Django 认为标准名称存在争议，因此它对 MVC 有自己的解释。在这里，我们有以下内容：

*   `View` 代表向最终用户展示*哪些*数据，而不是数据如何表示
*   `Template` 代表数据如何向最终用户表示
*   `Model` 代表数据层

这就是为什么 Django 遵循 `MVT` 框架（*Model-View-Template*）。但现在，问题是，Django 中的控制器是什么？框架本身就是控制器，因为它使用其内置功能处理整个路由逻辑。

重要说明

你不需要深入研究 MVT 概念，因为随着你在 Django 中编写更多代码，这个概念会成为肌肉记忆。

MVT 是一个使用模板的概念，但在当今世界，大多数产品都是为多个领域构建的，例如移动、物联网和 SaaS 平台。为了为所有这些领域构建产品，开发者生态系统也发生了演变；现在，组织正转向 `API-first` 开发方法（[https://blog.postman.com/what-is-an-api-first-company/](https://blog.postman.com/what-is-an-api-first-company/)）。这意味着 API 是“第一类公民”；产品的每个功能都是以 API-first 模型构建的，这有助于创建更好的客户端（移动应用、前端应用等）和服务器集成。它涉及在客户端和服务器之间建立契约，以便每个团队可以并行工作而无需太多依赖。一旦两个团队完成工作，产品的集成和开发周期就会变得更快，并且开发体验更好。

移动设备使用量的增长意味着构建平台无关的后端 API 很重要，这些 API 可以被任何客户端使用，Android 应用、iOS 应用、浏览器框架等等。作为一个独立的 MVT 框架，Django 能满足所有这些需求吗？不能。使用 Django 的开箱即用功能创建 API 所需的额外工作量与重新发明轮子类似。这就是为什么大多数组织在使用 Django 的同时使用 Django 的 REST 框架来创建 API。

重要说明

在本书中，我们将不关注模板和仅使用 Django 构建的独立 Web 应用，而是专注于使用 Django 和 DRF 创建 API。有关仅使用 Django 入门的更多信息，我最喜欢的资源之一是 Django Girls 教程：[https://tutorial.djangogirls.org/en/](https://tutorial.djangogirls.org/en/)。

现在我们已经了解了 MVT 框架是什么以及 Django 如何成为 MVT 框架，让我们使用 Django 和 DRF 创建一个基本的 Hello World Web 应用，并设置我们的项目结构和开发环境。

# 使用 Django 和 DRF 创建“Hello World”Web 应用

如前所述，Django 是一个基于 Python 的 Web 框架，因此我们需要使用 Python 编程语言编写代码。如果你已经在使用 Linux 或 macOS 系统，则 Python 已预装。然而，对于 Windows 系统，你必须按照官方 Python 网站上的说明进行安装：[https://www.python.org/downloads/](https://www.python.org/downloads/)。

我们还将使用 `virtualenv` 作为我们首选的工具来管理多个项目的不同环境，使我们能够创建隔离的 Python 环境。

重要说明

我们不会深入研究 `virtualenv`，因为我们希望你知道如何以及为什么为不同项目使用 `virtualenv`。你可以在其官方网站上找到有关 `virtualenv` 的详细信息：[https://virtualenv.pypa.io/en/latest/index.html](https://virtualenv.pypa.io/en/latest/index.html)。

首先，让我们使用最新版本的 Python（最好 >3.12.0）创建一个虚拟环境。以下命令适用于 Linux/Unix/macOS；对于 Windows，请查看下一节：

```
pip install virtualenv
virtualenv -p python3 v_env
source /path to v_env/v_env/bin/activate
```

现在，我们将分解代码的含义：

*   `pip install virtualenv` 在系统上安装 `virtualenv`。`pip` 是随 Python 提供的内置包管理器，已在 Mac 和大多数 Linux 环境中预装。
*   `virtualenv -p python3 v_env` 使用名称 `v_env` 创建一个新的虚拟环境（这只是我们给虚拟环境起的名字；你可以取另一个相关名称）。`-p python3` 标志用于告诉我们应使用哪个解释器来创建虚拟环境。
*   `source /path to v_env/v_env/bin/activate` 执行 `activate` 脚本，该脚本将虚拟 Python 解释器加载为 shell 中的默认 Python 解释器。

现在 Python 虚拟环境已设置好，我们将专注于管理包依赖。要安装 Django 的最新版本，请运行以下命令：

```
pip install Django==5.0.2
```

对于 Windows 系统，从 [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/) 下载 Python 3.12 或更高版本并按照向导进行安装。请记住在安装步骤中勾选 `Add python.exe to PATH` 复选框。

要验证你的 Python 安装，请在终端中使用以下命令：

```
C:\Users\argo\> python --version
Python 3.12.0
```

一旦 Python 安装成功，你可以使用以下命令设置虚拟环境并安装 Django：

```
py -m pip install --user virtualenv
py -m venv venv
.\<path to venv created>\venv\Scripts\activate
// to install Django
pip install Django==5.0.2
```

Windows 特定命令的解释与我们对 Linux/MacOS 系统的解释相同。

重要说明

我们不使用 poetry、PDM、pipenv 或任何其他依赖和包管理工具，以避免使初始设置过于复杂。

此外，我们更喜欢使用 Docker 环境来创建更多隔离并提供更好的开发体验。我们将在 [*第 10 章*](#B18867_10.xhtml#_idTextAnchor204) 中了解更多关于 Docker 的内容。*

通过之前的命令，我们的本地 Python 和 Django 开发环境已准备就绪。现在，是时候创建我们的基本 Django 项目了。

## 创建我们的 Django hello_world 项目

