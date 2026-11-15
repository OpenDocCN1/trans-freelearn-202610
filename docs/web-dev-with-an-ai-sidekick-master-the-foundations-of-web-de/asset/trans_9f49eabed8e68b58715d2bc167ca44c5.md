
# Bash 资源

前面的命令将为您提供关于各个命令的详细、权威信息。这些 `man` 页面并不总是友好的，但它们准确且无处不在。

## Bash 初学者指南

本指南专注于基础知识和常见模式，是参考手册的良好补充：

[https://tldp.org/LDP/Bash-Beginners-Guide/html/](https://tldp.org/LDP/Bash-Beginners-Guide/html/)

## 高级 Bash 脚本编程指南

尽管名为高级指南，但它被广泛用作实际脚本模式的参考。部分内容带有主观性，但极其详尽：

[https://tldp.org/LDP/abs/html/](https://tldp.org/LDP/abs/html/)

## Explainshell

该工具允许您粘贴一条 Shell 命令，并内联查看每个部分的解释。在阅读文档或 CI 日志中不熟悉的命令时，它特别有用：

[https://explainshell.com/](https://explainshell.com/)

## ShellCheck

ShellCheck 是一款用于 Shell 脚本的静态分析工具。它会高亮显示常见错误、不安全模式和可移植性问题。即使是经验丰富的 Shell 用户也依赖它：

[https://www.shellcheck.net/](https://www.shellcheck.net/)

# 总结

在本章中，您学习了 Bash 如何融入开发者的日常工作。具体来说：

- Bash 是什么，以及它如何作为 Unix Shell 和命令语言发挥作用
- 如何使用 `cd`、`ls` 和 `pwd` 等核心命令从命令行导航文件系统
- 如何从终端安全地创建、复制、移动和删除文件和目录
- 如何使用 `cat` 和 `echo` 等命令检查文件内容并生成输出
- 如何编写简单的 Bash 脚本来自动化一系列命令
- 如何在 Bash 脚本中使用变量和命令行参数
- 如何使用 AI 工具理解 Shell 命令并调试脚本，而不是盲目复制它们

主要教训是 Bash 为您提供了一种控制开发环境的方式。命令很重要，但更重要的是在运行它们之前知道它们会做什么。

在下一章中，您将使用这些命令行技能来设置一个完整的 Web 开发项目以及所需的工作环境。

# 获取本书的 PDF 版本及更多内容

扫描二维码（或访问 packtpub.com/unlock）。按名称搜索本书，确认版本，然后按照页面上的步骤操作。

![](Images/Unlock.png)![](Images/Unlock-01.png)

> **注意：** 请保留好您的发票。直接从 Packt 购买的不需要发票。

# 第 3 部分：调查项目

在本书的第三部分中，您将把所有内容整合到一个完整的项目中。您将构建一个包含前端界面、后端逻辑和工作数据库的调查应用程序。目标是超越孤立的示例，了解当所有部分连接在一起时，真实系统的行为方式。

AI 在整个构建过程中被使用，帮助生成代码、建议方法并加速重复性任务。同时，您需要阅读、质疑并改进 AI 产生的内容。在本部分结束时，您将拥有一个可工作的应用程序，并获得更多实践来判断 AI 输出何时有用、错误或需要更改。

本部分包含以下章节：

- [第 11 章](#11.xhtml#_idTextAnchor764)，设置项目环境
- [第 12 章](#12.xhtml#_idTextAnchor809)，单元、集成和端到端测试
- [第 13 章](#13.xhtml#_idTextAnchor872)，指定并构建初始项目
- [第 14 章](#14.xhtml#_idTextAnchor922)，使用规格驱动开发添加项目功能
- [第 15 章](#15.xhtml#_idTextAnchor977)，部署到公共服务器

## 第 11 章：设置项目环境

本书的最后一部分从学习前端和后端 Web 开发的单个技术转向在一个实际项目中应用它们。您将设计并构建一个名为 `TallyApp` 的完整 Web 应用程序，这是一个小型的调查和投票应用程序，而不是孤立的示例。

该项目的范围被刻意限定为足够小以便端到端理解，但又足够丰富，让人感觉像是一个您可以合理扩展或部署的东西。在本章中，我们将通过安装和配置开发工具来启动 TallyApp 项目。

作为名词，*tally* 是对项目、事件或投票的持续计数或记录。它通常指通过逐步增加计数达到的总数。例如，*the final tally of votes* 意味着完成的计数。作为动词，*tally* 意为计数或将项目添加到运行总计中。它也可以意味着一致或对应。例如，*the numbers do not tally* 意味着它们不匹配。

在本章结束时，您将已配置好带有 GitHub Copilot 的开发环境，使用 Docker 设置一致且可重复的环境，并学会如何使用 Git 将代码置于版本控制之下，以便您有信心开始构建功能。

本章关注于了解哪个工具解决哪个问题。Copilot 在您编写和审查代码时提供帮助。Docker 为每个人提供相同的运行时。Git 记录项目历史，以便您可以无畏地尝试更改。在添加 TallyApp 功能之前，您将使用所有这三种工具。

在本章中，我们将涵盖以下主题：

- TallyApp 简介
- 将 GitHub Copilot 配置为您的编码助手
- 用于一致服务器设置的 Docker
- 使用 Git 进行源代码版本控制

本章的建议聊天机器人提示词及示例响应可在以下链接找到：

[https://github.com/markjprice/web-dev-ai/blob/main/prompts/ch11-env/readme.md](https://github.com/markjprice/web-dev-ai/blob/main/prompts/ch11-env/readme.md)

### TallyApp 简介

TallyApp 是一个具有三个明确功能领域的调查和投票应用程序。

1. 首先，它允许经过身份验证的用户设计和发布调查。这包括定义不同类型的问题，例如多项选择和自由文本回答，并生成可与受访者共享的公共链接。
2. 其次，它为参与调查的人提供简洁、专注的体验，他们可以点击链接、回答问题并提交回答，而无需账户。
3. 第三，它为调查创建者提供分析结果的工具，使用图表和结构化数据视图来探索趋势和个别答案。

在接下来的章节中，您将逐步使用 AI 编码助手设置开发环境，然后指定、构建和改进 TallyApp。同样重要的是，这些章节展示了真实 Web 项目是如何开发的。您将做出设计决策，重新审视早期选择，并了解后端逻辑、前端展示和数据分析如何结合在一起。

在 TallyApp 项目结束时，您不仅将学习 Web 框架和语言的个别特性。您将构建并理解一个完整、可工作的 Web 应用程序，并将处于更有利的位置来设计属于您自己的应用程序。

### 关于过度依赖 AI 的警告

Anthropic 在其文章《AI 协助如何影响编码技能的形成》中发出了警告：[https://www.anthropic.com/research/AI-assistance-coding-skills](https://www.anthropic.com/research/AI-assistance-coding-skills)。

Anthropic 研究人员进行了一项涉及 52 名软件开发人员的随机对照试验。

所有参与者都有 Python 的常规经验，但不熟悉一个名为 Trio 的特定异步库。他们被随机分配到两组：
