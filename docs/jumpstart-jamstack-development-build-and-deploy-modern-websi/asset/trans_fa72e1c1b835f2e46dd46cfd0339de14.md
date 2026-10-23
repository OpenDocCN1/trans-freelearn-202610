
> **重要提示**  
> 在开始编写自定义插件之前，请确保该插件在 Gatsby 插件库中尚不存在。

使用 Gatsby 插件库中的插件有诸多好处。这里列出其中的一些：

- **节省时间：** 如果一个插件能完全实现你的任务，你可以直接安装、配置并开始使用。如果一个插件只能部分实现你的任务，你也可以在其基础上添加自定义功能进行扩展。
- **修复 Bug：** 你或其他开发者可以修复 Bug 并将信息发布到 npm，然后通过更新应用中的插件来获取这些修复。
- **改进功能：** 与修复 Bug 类似——你或其他开发者可以为插件添加新功能并发布到 npm。
- **安全性：** 你或其他开发者可能会发现插件中的代码或依赖存在安全问题；更新到最新的安全版本并发布到 npm。

要查找 Gatsby 插件，请访问位于 `https://www.gatsbyjs.org/plugins/` 的 Gatsby 插件库。

下面的页面提供了一个 **Search Gatsby Library** 文本框，让你可以在 2000 多个插件中导航搜索！文本框位于页面左上方，如下图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_127_0.png)

*图 9.1 – Gatsby 插件库页面*

每个 Gatsby 插件都附带了如何安装、配置和使用的详细说明。Gatsby Source 插件允许 Gatsby 从数据库、文件、RESTful API（其中 **REST** 代表 **Representational State Transfer**）等来源获取内容。

我们在*第 2 章，Sanity 简介*中安装的 `Starter` 项目已经预装并配置了 `gatsby-source-sanity` 插件。通过 `gatsby-source-sanity` 插件，我们能够从 Sanity 应用中获取内容。

在下一节中，我们将说明如何在 Gatsby 插件库中搜索插件。作为示例，我们将安装并配置两个源插件：`gatsby-source-filesystem` 和 `gatsby-source-drupal` 插件。

## 搜索插件

所有 Gatsby 插件都可以在位于 https://www.gatsbyjs.org/plugins/ 的 Gatsby 插件库中找到。

在页面左侧有一个插件搜索工具。开始输入要查找的插件的潜在关键词。

例如，如果你正在查找 Google Analytics 插件，可以输入 `google analytics`。

在我们的示例中，我们将查找 `gatsby-source-filesystem` 插件，因此可以输入 `source filesystem`，如下面的截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_128_0.png)

图 9.2 – Gatsby 插件库页面

在页面左侧，会返回包含该关键词的所有结果。每个结果包含插件名称、描述、下载次数和插件类型。当你点击插件结果时，你将看到插件的描述以及如何安装的说明。

## 安装插件

插件通过命令行使用 npm 安装。当你找到所需的插件后，请在插件页面的 Install 部分查看安装命令，如下面的截图所示：

### 安装

```
npm install gatsby-source-filesystem
```

图 9.4 – gatsby-source-filesystem 插件页面的 Install 部分

在项目 `web` 文件夹内的终端中，输入上面截图所示的命令，然后按 Enter 键。

该命令会将插件及其依赖项下载到 `node_modules` 文件夹中，并将 gatsby-source-filesystem 插件及要下载的版本添加到 `package.json` 文件的 Dependencies 部分。

## 配置插件

现在，插件文件已经在自己的应用中了，但为了启用插件，我们需要将其添加到位于 Gatsby 项目根目录下的 gatsby-config.js 文件中——在我们的示例中，该文件位于 `web` 文件夹内。

gatsby-config.js 文件是应用中最重要的文件之一。它包含以下配置：

- siteMetadata（对象）
- plugins（数组）
- pathPrefix（字符串）
- polyfill（布尔值）
- mapping（对象）
- proxy（对象）
- developMiddleware（函数）

要启用插件，只需在 gatsby-config.js 文件的 plugins 数组中添加插件名称，如下所示：

```
module.exports = {
  plugins: [
    ...
    'gatsby-plugin-name',
    ...
  ]
}
```

一些其他插件可能还需要配置选项，这些选项也可以添加到 gatsby-config.js 文件的 plugins 数组中，如下面的代码片段所示：

```
module.exports = {
  plugins: [
    ...
    {
      resolve: 'plugin-name',
      options: {
        // Options list, if any
      },
    },
    ...
  ]
}
```

既然我们知道了如何启用插件，现在让我们启用 gatsby-source-filesystem 插件，并告诉 Gatsby 在哪里可以找到我们的内容。在 Gatsby 插件页面的 How to use 部分，有如何在应用中配置该插件的详细说明。

gatsby-source-filesystem 插件可以接受三个选项，如下所示：

- name：GraphQL 查询使用的名称（必填）
- path：实际文件所在的文件夹（必填）
- ignore：需要忽略的文件扩展名列表（可选）

在我们的示例中，可以这样添加配置：

```
module.exports = {
  plugins: [
    ...
    {
      resolve: 'gatsby-source-filesystem',
      options: {
        path: '${__dirname}/src/data',
        name: 'md-files',
        ignore: ['**/\.*'],
      },
    },
    ...
  ]
}
```

通过上述代码，我们启用了 gatsby-source-filesystem 插件，并告诉 Gatsby 在哪里找到文件、内容的名称，以及忽略所有以点开头的文件。现在，重新启动你的应用。在命令行中按 Ctrl + C，然后运行以下命令：

```
npm run dev
```

作为示例，我们将从一个 Markdown 文件中查询数据。我们将在 `web/src` 应用文件夹下创建一个名为 `data` 的文件夹。在 `data` 文件夹内，我们创建所有需要的 Markdown 文件。文件按子文件夹组织，系统会自动找到它们。为此，我们需要 gatsby-transformer-remark 插件，将在下一节中安装。

## 安装和配置 gatsby-transformer-remark 插件

该插件解析并编译 Markdown 文件，并将其转换为 HTML。它还支持 frontmatter，这样你就可以定义 title、slug、date、image、excerpt 等字段。

让我们安装并配置该插件。

### 安装插件

由于这是一个 Gatsby 插件，因此可以在插件库中找到它，并附带安装和配置说明。

在你的 Gatsby 文件夹中，运行以下命令来安装插件：

```
npm install --save gatsby-transformer-remark
```

### 配置插件

插件下载完成后，我们只需在 gatsby-config.js 文件中添加插件名称即可启用它，如下面的代码块所示：

```
module.exports = {
  plugins: [
    ...
    'gatsby-transformer-remark',
    ...
  ]
}
```

就这么简单！现在，重新启动你的应用，我们已准备好创建第一个内容！

## 创建内容文件

在本节中，我们将在一个 Markdown 文件中创建内容。如上所述，gatsby-transformer-remark 允许你在 Markdown 文件中添加字段。让我们开始添加 frontmatter，如下所示：

1. 在 `web/src/data` 中创建一个名为 `hello-world.md` 的文件。在文件的开头和结尾添加三条短横线。在这两行之间，你可以定义你的 frontmatter，如下所示：

```
---
// Define your frontmatter.
---
```

2. frontmatter 中的字段可以定义为键/值对。我们可以添加想要的信息。在本例中，我们将按以下方式添加 title、date 和 excerpt：

```
---
title: Hello World
date: "2020-07-12T09:01:00"
excerpt: Hi! This is my first content from a markdown file! I will fill with James Joyce's poem.
---
```

