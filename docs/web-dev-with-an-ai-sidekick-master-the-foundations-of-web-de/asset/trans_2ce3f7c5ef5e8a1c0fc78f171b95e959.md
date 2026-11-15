
# 前端 Web 开发

## [第 2 章：HTML – 构建您的网页](#02.xhtml#_idTextAnchor098)

涵盖 HTML（超文本标记语言）的基础知识。您将使用 AI 生成一个简单的页面，并在此过程中了解元素、标签和属性。您将了解 HTML 在定义网页内容结构（从标题到链接和图片）中的作用。您将创建一个联系表单，引入表单元素。

## [第 3 章：CSS – 为网页设置样式](#03.xhtml#_idTextAnchor154)

展示如何控制表现和布局。您将让 AI 解释选择器、属性和值。您将把样式应用到来自[第 2 章：HTML – 构建您的网页](#02.xhtml#_idTextAnchor098)的网页上。您将学习响应式设计，并使用 CSS 媒体查询制作适合移动设备的网页。

## [第 4 章：SVG – 绘制可缩放图形](#04.xhtml#_idTextAnchor212)

介绍 SVG 作为一种使用代码生成图形的方式，这将用于绘制调查结果图表。AI 将解释 SVG 是一种基于 XML 的二维图形标记语言，以及它与 canvas/位图的区别。通过 AI 引导的示例，您将在 SVG 中创建基本形状（圆、矩形、线条）并将它们嵌入 HTML。

## [第 5 章：JavaScript – 让网页具有交互性](#05.xhtml#_idTextAnchor266)

涵盖变量、函数、事件和 DOM 等基本 JavaScript 概念。我们首先描述什么是 JavaScript（一种在浏览器中运行的轻量级解释型语言）以及在 HTML 中哪里放置 `<script>` 块。

## [第 6 章：TypeScript – 更安全的 JavaScript](#06.xhtml#_idTextAnchor316)

介绍 TypeScript 作为 JavaScript 的超集，它增加了静态类型并解释了为什么它们很有用。

# 后端 Web 开发

## [第 7 章：SQL – 管理数据](#07.xhtml#_idTextAnchor374)

揭开 SQL 的神秘面纱（使用 SQLite 和一个简单的数据库进行练习）。AI 解释什么是数据库以及如何使用 SQL 查询数据。您将通过 AI 提示练习基本 SQL 命令：`SELECT` 语句、过滤（`WHERE`）和排序（`ORDER BY`）。AI 可以生成示例数据集（如 `users` 表）并用查询挑战您（“如何获取本月注册的所有用户？”）。

## [第 8 章：Python – 后端编程](#08.xhtml#_idTextAnchor494)

涵盖基本的 Python 语法和概念，为读者进行后端 Web 开发做准备。您将安装 Python，然后学习变量、循环和函数等 Python 基础编码。您将请求 AI 逐行解释代码，以加深理解。

## [第 9 章：Django – 后端 Web 开发](#09.xhtml#_idTextAnchor598)

专注于 Django，这是 Python 最常用的 Web 框架。您的 AI 将解释 Django 如何通过其 `MVT`（模型-视图-模板）模式工作，该模式与传统的 `MVC`（模型-视图-控制器）模式相关但有所不同，以及为什么它被用于快速 Web 开发。在 AI 的指导下，您将启动一个 Django 项目（本书提供分步说明，AI 可以帮助排查故障）。您将创建一个新的 Django 项目，为本书 *调查项目* 部分的完整项目奠定基础。AI 帮助您编写一个简单的视图和模板。您将了解 Django 如何提供 HTML 以及如何使用模板变量。

## [第 10 章：Bash/Shell – 开发者的命令行技能](#10.xhtml#_idTextAnchor689)

简要离开 Web 编程来教授 Bash/Shell 的使用，因为许多开发任务涉及命令行，尤其是自动化部署。您将通过让 AI 描述每个命令的作用并在终端或模拟器中尝试，来练习常用命令（`ls`、`cd`、`mkdir`、`rm` 等）。AI 将引导您创建一个小的 Bash/Shell 脚本来自动化任务，如备份文件或启动您的 Django 服务器。

# 调查项目

## [第 11 章：设置项目环境](#11.xhtml#_idTextAnchor764)

启动 TallyApp 调查项目并配置工具。您将看到您将构建的调查应用及其功能的概览，如创建调查、参与调查和查看结果。提供了设置 `VS Code` 与 `GitHub Copilot` 的分步说明（包含屏幕截图或代码片段）。您将了解基于 `Docker` 的开发环境，以及如何使用 `Git` 将代码置于版本控制之下。

## [第 12 章：单元、集成和端到端测试](#12.xhtml#_idTextAnchor809)

教您如何为 TallyApp 设计和实施分层测试策略，在 Django 中编写有意义的单元测试和集成测试，使用 `Playwright` 自动化真实浏览器交互进行端到端测试，并使用 AI 工具自动生成测试。

## [第 13 章：规范与构建初始项目](#13.xhtml#_idTextAnchor872)

为 TallyApp 定义清晰的项目简介，将其转化为可执行的开发计划，指导您搭建带有适当应用和模型的 `Django 6` 项目，并实现核心身份验证和调查功能。

## [第 14 章：使用规范驱动开发添加项目功能](#14.xhtml#_idTextAnchor922)

涵盖如何实施结构化的功能规范，使用 AI 编码助手在现有 Django 项目中实现它，并验证实现是否满足明确定义的验收标准。

## [第 15 章：部署到公共服务器](#15.xhtml#_idTextAnchor977)

回顾将 TallyApp 部署到连接广域网的服务器的选项，以便公众可以使用该网站。您将把 TallyApp 部署到公共可访问的服务器，了解请求如何通过生产 Django 技术栈流转，知道如何在部署之间保存和备份用户数据，以及随着流量和期望的增加需要改变什么。

## 后记

回顾您 Web 开发学习旅程的下一步。

## 附录

包含每章末尾问题的答案。

# 为什么选择这些语言和技能？

您可能会想知道为什么我选择涵盖这些特定的语言、框架和技能。一个主要原因是 `Stack Overflow` 每年都会对数千名开发者进行调查，其中一个问题是：*在过去一年中，您在哪些编程、脚本和标记语言中进行了大量开发工作？*

2025 年调查中对此问题的 31,771 份回复显示，开发者报告在过去一年中大量使用的前六种语言是 JavaScript、HTML/CSS、SQL、Python、Bash/Shell 和 TypeScript，如 *图 1.1* 所示：

![](Images/B34553_01_01.png)

*图 1.1：2025 年最广泛使用的编程语言调查*

您可以在以下链接的调查中看到这些结果：[https://survey.stackoverflow.co/2025/technology#most-popular-technologies-language](https://survey.stackoverflow.co/2025/technology#most-popular-technologies-language)

当您意识到所有这些语言都是 Web 开发所需的，而且如今大多数编程工作都是 Web 开发时，这些结果就讲得通了。前六名中缺少用于绘制可缩放矢量图形的 SVG，以及 Python 最流行的 Web 开发框架 Django。我甚至一度考虑将书命名为 *Top Six Programming Languages*，但最终决定 Web 开发是涵盖所有这些语言的书籍更好的连接主题。

您可以在 *图 1.2* 中看到每种语言和框架在 Web 开发中所扮演角色的摘要：

![](Images/B34553_01_02.png)
