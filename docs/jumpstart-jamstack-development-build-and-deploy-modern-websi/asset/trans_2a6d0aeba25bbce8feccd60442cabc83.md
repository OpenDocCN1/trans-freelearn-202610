

## 3 探索 Sanity Studio
- 技术要求 27
- 修改示例作者 28
  - 修改现有作者 28
  - 创建额外的作者 30
- 删除和添加文章 32
  - 删除示例文章 32
- 创建新文章 33
- 修改网站设置并重新部署网站 35
  - 设置网站名称 36
  - 通过 Netlify 重新部署 37
- 总结 38

## 4 Sanity 配置与模式
- 技术要求 40
- 使用模式建模内容 40
  - 克隆项目 40
  - 探索项目文件夹 42
- 模式关系 46
  - 一对一关系 46
  - 一对多关系 48
- 扩展、修改和部署模式 49
  - 活动模式 49
  - 场地模式 50
  - 模式文件 52
  - 部署模式 52
- 总结 56

## 5 Sanity 的 GROQ 语言
- 技术要求 58
- 为什么是 GROQ？ 58
  - 安装 Vision 59
- GROQ 与 SQL 的比较 60
- 使用 GROQ 进行基本查询 62
  - 选择所有活动 62
  - 选择所有即将举行的活动 63
  - 选择所有过往活动 64
  - 选择即将举行的虚拟会议和非虚拟会议 64
  - 选择特定活动字段 64
  - 从关系中选择特定字段 65
- 高级 GROQ 66
  - 按场地获取活动 66
  - 格式化响应 67
  - 计数结果 67
- 总结 68

## 6 Sanity 的 GraphQL 游乐场
- 技术要求 69
- GraphQL 简介 70
- 部署 GraphQL API 71
- 基本 GraphQL 语法 71
- GROQ 与 GraphQL 的比较 73
- GraphQL 游乐场基础 76
- 查询参数 78
- 总结 80

## 7 Gatsby – 简介
- 技术要求 82
- 基于 React 构建的 Gatsby 82
- Gatsby 基本项目结构 82
- gatsby-config.js 83
- dotenv 86
- 关键 Gatsby 文件 87
- Gatsby 文件夹 88
- gatsby develop 命令 89
- 总结 90

## 8 Gatsby 与 GraphQL
- 技术要求 91
- GatsbyJS 中的 GraphQL 92
- GraphiQL，一个 GraphQL 导航器 94
- 总结 100

## 9 Gatsby 源插件
- 技术要求 101
- 理解插件 102
- Node 包管理器 102
- 语义化版本控制 103
- 从 npm 安装包 103
- 搜索、安装和配置插件 106
- Gatsby 插件库 106
- 搜索插件 108
- 安装和配置 gatsby-source-filesystem 插件 109
- 安装和配置 gatsby-transformer-remark 插件 112
- 安装和配置 gatsby-source-drupal 插件 116
- 总结 120
- 延伸阅读 121

切换场地可见性 190

总结 191

## 14 使用 Netlify 和 Azure 进行部署
- 技术要求 194
- Netlify 简介 194
- Netlify 插件 196
- HTML 压缩 196
- 通过 netlify.toml 文件进行高级配置 198
- 通过命令行进行 Netlify 部署 200
- Azure 静态 Web 应用部署 201
- 创建静态 Web 应用 202
- 静态 Web 应用持续部署 206
- 总结 209

## 15 结语
- 概念总结 211
- Jamstack 的未来 212
- Jamstack, JAMstack 和 JAM Stack 212
- Jamstack 社区资源 214
- 为 Jamstack 项目做贡献 216
- 为 Sanity 做贡献 216
- 为 Gatsby 做贡献 219
- 为 Netlify 做贡献 220
- 最后的想法 220
- 为什么要订阅？ 221

## 您可能喜欢的其他书籍

## 索引

## 前言

Jamstack（JavaScript、API 和标记）通过利用 Gatsby、Sanity 和 Netlify 等工具，使 Web 开发人员能够创建和发布以速度、安全性和可访问性为核心的现代且易于维护的网站和 Web 应用。使用 Jamstack 工作的开发人员将通过这份关于静态站点生成和内容管理的实用指南，很好地利用他们的知识。这本 Jamstack 书籍采用实践方法，介绍实施和相关方法论，让您迅速掌握现代 Web 开发。

通过逐步解释核心概念、实用示例和自测问题，您将从构建活动和场地模式结构开始，然后扩展功能，探索 Jamstack 所提供的所有内容。您将学习如何构建一个示例 Jamstack，如何使用模式构建结构化内容，如何使用 GraphQL 公开内容，以及如何使用 Gatsby 通过页面和模板组件构建活动网站。最后，您将把网站部署到 Netlify 服务器。

读完本书后，您将获得使用 Jamstack 安装、配置、构建、扩展和部署简单活动网站所需的知识和技能。

### 本书面向读者

本书面向希望实际实施 Jamstack 的 Web 开发人员。希望构建现代、快速且安全的 Web 应用的 JavaScript 开发人员也会发现本书很有用。假设读者熟悉 JavaScript 和数据库编程。

### 本书内容

*第 1 章，Jamstack 的历史*，为您提供 Web 设计和开发历史的概览，以及后端驱动网站面临的问题，即安全性、速度、可访问性、托管和部署问题。

*第 2 章，Sanity 简介*，教您如何使用基于网络的示例项目创建工具创建 Sanity.io 网站。

第 3 章，探索 Sanity Studio，向您介绍 Sanity Studio，一个结构化内容管理工具。然后，您将学习如何配置网站以及创建、修改和删除内容。

第 4 章，Sanity 配置与模式，帮助您理解 Sanity.io 内容模式的术语和语法，以及它们如何定义内容结构。您还将学习如何定义实体之间的一对一关系和一对多关系。

第 5 章，Sanity 的 GROQ 语言，向您介绍 GROQ，即 Sanity.io 查询其数据的专有方式。将展示它与标准 SQL 的相似之处和不同之处，并详细说明各种示例。

第 6 章，Sanity 的 GraphQL 游乐场，向您介绍 Sanity.io 的 GraphQL 游乐场，如何启动它，以及它与标准 GraphQL 有何不同。您将学习如何获取单条记录类型或多条记录。

第 7 章，Gatsby – 简介，向您介绍 Jamstack 的 J 部分。您将了解 GatsbyJS，一个基于 ReactJS 的静态生成器，它被创建的原因，它是如何构建的，以及它解决的问题。最后，您将了解涉及的相关配置文件和关键文件（如 ssr 和 node）。

第 8 章，Gatsby 与 GraphQL，向您介绍 Gatsby 的 GraphQL 工具，以及它与标准 GraphQL 有何不同。本章将帮助您理解如何通过 Gatsby 源代码使用 GraphQL 将源内容连接并服务于静态站点生成器。

第 9 章，Gatsby 源插件，将向您介绍由 Jamstack 社区构建的各种插件，这些插件允许 GatsbyJS 连接到各种类型的知名后端系统，从文件系统到现有的 CMS 框架，如 Drupal。

第 10 章，构建 Gatsby 组件，将向您介绍 Jamstack 的 M 部分，即标记，以及 Gatsby 的构建块：组件，同时学习页面、模板和部分的结构以及它们之间的差异。您将学习如何创建、编辑和扩展组件。

第 11 章，API – 扩展 Gatsby，向您介绍 Jamstack 的 A 部分，即 API，以及如何在编译 Web 页面的动态部分中使用它们，以重现 Web 开发人员在传统基于服务器的 Web 开发中熟悉的功能。

