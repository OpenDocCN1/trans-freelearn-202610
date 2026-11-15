
# 第 10 章：Bash/Shell – 开发者的命令行技能

## 获取本书的 PDF 版本及更多内容

扫描二维码（或访问 `packtpub.com/unlock`）。按名称搜索本书，确认版本，然后按照页面上的步骤操作。

![](Images/Unlock.png) ![](Images/Unlock-01.png)

> **注意：** 请保留好发票。直接从 Packt 购买的不需要发票。

在下一章中，你将从 Django 的 Web 框架结构转向支持日常开发工作的命令行环境。你将学习 `Bash` 和其他 Shell 如何提供一种通过命令与操作系统交互的方式，让你能够运行程序、自动化重复任务，并理解 `Python`、`Git` 和 `Django` 等工具在运行命令时到底在做什么。

使用 `Bash` 或类似的 Shell 进行脚本编写似乎与 Web 开发无关，但其实从未远离。无论你是创建新项目、安装依赖、运行开发服务器、管理文件还是部署代码，你都会反复发现自己在终端中输入命令。

像 `Bourne Again Shell` (`Bash`) 和 `Z Shell` (`Zsh`) 这样的 Shell 让你能将一次性命令转化为可保存、可编辑、可再次运行的可重复步骤。

请记住，在 Stack Overflow 2025 年对 31,771 名开发者的调查中，48.7% 的人表示他们曾用 `Bash`/`Shell` 进行过大量开发工作。这使得学习 Shell 脚本变得值得，即使你主要认为自己是一名 Web 开发者。

`Bash` 是大多数 Linux 系统的默认 Shell，长期以来一直是 macOS 和服务器环境的核心。即使在 Windows 上，现代开发环境也通过 Windows Subsystem for Linux 和 `Git Bash` 等工具提供 `Bash`。因此，`Bash` 已成为跨平台的通用语言。

自 2019 年 macOS Catalina 起，Mac 的默认 Shell 是 `Zsh`，而非 `Bash`。本章的基本命令在两种 Shell 中完全相同。macOS 自带的 `Bash` 版本相当旧。要在 macOS 上安装更新的 `Bash`，请使用以下 Homebrew 命令：`brew install bash`。或者，你可以直接使用 `Zsh`，因为对于本章内容来说，差异并不重要。

本章刻意将 `Bash` 扎根于日常开发任务，而不是将其视为一种需要孤立掌握的完整编程语言。你将了解什么是 `Bash` 以及它如何融入 Shell 的更广泛概念，练习用于导航和操作文件的核心命令，然后编写一个自动化真实任务的小脚本。读完本章后，你应该能轻松阅读和调整 Shell 命令，即使你从未完全独立编写过复杂脚本。

因为命令行可能让人感到无情，我们还将探讨 AI 工具如何帮助你检查理解。善用它们可以解释陌生命令、建议更安全的替代方案，并帮助你理解失败的原因，而不仅仅是粘贴一个你不理解的修复方案。

读完本章，你将理解 `Bash` 的作用、导航文件系统、运行常用命令、编写简单的 `Bash` 脚本，并在运行前使用 AI 工具检查陌生命令。

本章将涵盖以下主题：

* 介绍 `Bash`
* 在 Windows 上设置 `Bash`
* 基本命令
* 编写简单脚本
* 自动化你的 Django 开发
* 使用 AI 获取 Shell 帮助

本章建议的聊天机器人提示词及示例回复可在以下链接找到：[https://github.com/markjprice/web-dev-ai/blob/main/prompts/ch10-bash/readme.md](https://github.com/markjprice/web-dev-ai/blob/main/prompts/ch10-bash/readme.md)

## 介绍 `Bash`

`Bash` 代表 `Bourne Again Shell`，它既是一个 Unix Shell，也是一种命令语言。简单来说，`Bash` 是位于你和操作系统之间的程序，读取你输入的命令并将其转化为行动。

当你打开终端窗口并输入类似 `python manage.py runserver` 或 `npm install` 的内容时，你并不是在直接与 `Python` 或 `Node` 对话。你是在与 `Bash` 对话，`Bash` 负责启动那些程序、向它们传递参数并处理它们的输出。

> **提示：** 请结合具体例子解释 `Bash`、Shell 和终端的区别。

### Shell 脚本编写

Shell 是操作系统的用户界面，只是不是图形化的。你不需要点击图标或菜单，而是发出文本命令。Unix 系统一直将这种基于文本的界面视为正常的工作方式，这也是为什么像 `Bash` 这样的 Shell 变得如此可编写脚本且富有表现力。随着时间的推移，`Bash` 也成了一种编程语言，拥有变量、条件判断、循环和函数。这让你能超越单个命令，开始自动化任务序列。

`Bash` 在开发中特别有用，因为许多工具都是围绕它设计的。运行本地开发服务器、启动和停止后台进程、清理构建目录，或将多个工具链接在一起，在 `Bash` 中都非常简单。

部署脚本几乎总是作为 Shell 脚本编写。持续集成系统执行 `Bash` 命令来构建、测试和部署应用程序。`Dockerfile` 严重依赖 Shell 命令来组装镜像。即使你在使用其他开发工具，`Bash` 往往也是将所有东西粘合在一起的胶水。

### 脚本可移植性

`Bash` 重要的另一个原因是可移植性。在你机器上编写的 `Bash` 脚本通常可以在云端的 Linux 服务器上原样运行。这种共享环境是云基础设施、DevOps 实践和容器化迅速普及的原因之一。当文档说“运行以下命令”时，几乎总是假设使用类 `Bash` 的 Shell，即使没有明确说明。

### 终端与 Shell

将 `Bash` 与终端本身区分开也很重要。终端只是显示文本和接受输入的窗口或应用程序。`Bash` 是运行在该终端内的程序。还有其他 Shell，如 `zsh` 或 `fish`，但 `Bash` 仍然是通用基准。即使你的系统使用不同的默认 Shell，`Bash` 通常也已安装并广泛支持，这也是为什么它仍被频繁教授和引用的原因。

最外层是终端窗口。这只是一个用户界面。在 Windows 上可能是 `Windows Terminal`，在 macOS 上是 `Terminal.app`，在 Linux 上则是众多终端模拟器之一。它的工作很简单：显示文本并发送你的按键。终端不理解命令。

运行在终端内部的是 Shell，这里是 `Bash`。`Bash` 是一个程序。它读取你输入的文本，解释它，跟踪当前目录和环境变量等状态，并决定下一步做什么。当你按下回车键，`Bash` 解析命令行、展开变量、应用引用规则，并确定要执行的内容。

当 `Bash` 识别出 `ls`、`python` 或 `docker` 等命令时，它会启动一个外部程序。这些程序是由操作系统或安装的工具提供的独立可执行文件。`Bash` 将它们作为子进程启动，传递参数，并等待它们完成。它们的输出通过 `Bash` 回流并由终端显示。

因此，流程始终是：

```
You type text → the terminal sends it to Bash → Bash interprets it → Bash runs programs → output flows back up to the terminal.
```

这个流程解释了初学者的几个常见困惑：

* 为什么更换 Shell 不会改变终端窗口
