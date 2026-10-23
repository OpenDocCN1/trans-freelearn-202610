

# Jamstack 缩写词解析

Jamstack 缩写代表 **JavaScript, APIs, 和 Markup**。由于静态站点页面无法通过任何实时预处理生成，页面的所有动态方面都必须通过 JavaScript 实现，JavaScript 将执行所有页面交互。

JavaScript 将使用 **应用程序编程接口 (APIs)** 与外部系统（如搜索引擎）通信。使用 API 是一种现代方式，可以利用预先构建的外部 **软件即服务 (SAAS)**，例如 Algolia 的搜索 API，或使用另一个原本会与静态站点位于同一服务器上的软件应用程序。将通信限制在 API 中可使较大系统中的每个组件保持独立且可重用。

最后，Markup 代表页面本身。如前所述，HTML 是一种用于创建网页的语言。有时，Markdown 会被错误地用作 Markup 的替代。Markdown 实际上是用于生成标记的一种记法之一；然而，生成标记的方式有很多种。

# Jamstack 优势

现在，让我们来了解 Jamstack 的优势。

## 速度

如前所述，静态页面的优势之一是每个页面实际上都是文件系统上的一个文件。如网页设计的早期一样，每个文件都通过 Web 服务器加载。唯一的瓶颈是访问文件、读取它，然后通过 Web 服务器发送其内容的速度。

**内容分发网络 (CDN)** 是一个遍布全球的众多服务器网络。这意味着页面将从距离网站用户最近的服务器进行最优提供，从而降低网络延迟。这为终端用户提供了整体更快、更好的体验。

所有的 **层叠样式表 (CSS)** 和 JavaScript 通常都会被压缩或以一种方式进行最小化，该方式会删除不必要的字符、空格、制表符、换行符和其他非必要项。使用最少的字符并将其组合到一个或多个文件中，以减少涉及的文件数量。

未压缩的 JavaScript 看起来像这样：

```
<javascript>
function multiplyTwoNumbers(number1, number2) {
return number1 * number2;
}
var result = multiplyTwoNumbers(2, 3);
</javascript>
```

压缩后的 JavaScript 看起来像这样：

```
<javascript>
function a (b, c) {return b*c;} var d=a (2, 3);
</javascript>
```

## 更不易出错

大多数 Jamstack 系统都有一个构建过程，该过程会在构建页面的过程中检查所有 HTML、JavaScript 和 CSS 的正确性，因此严重的编码错误会导致构建失败，不会让错误进入生产环境。

## 安全

使用安全的 **超文本传输安全协议 (HTTPS)** 而不是旧的 **超文本传输协议 (HTTP)** 意味着所有传输的信息都是安全的，因为它从端到端都是加密的。

此外，由于静态页面通过 *无服务器* 主机服务提供，因此没有任何后端系统或数据库被入侵的可能。不再需要花费宝贵的时间来保持服务器、库、登录访问和邮件服务的安全并始终保持最新版本，甚至也不必担心各种版本之间的冲突。

## 开发者体验

在 Jamstack 中开发意味着使用流行的现代编程语言，例如 JavaScript、TypeScript 和 **ECMAScript 6 (ES6)** 变体。Jamstack 开发者还可以利用现代查询语言如 GraphQL，以及 ReactJS 和 VueJS 等库和框架。他们还可以使用 Hugo 和 GatsbyJS 等静态站点生成器。

# 我们的 Jamstack

到目前为止应该很清楚，Jamstack 并不是一个技术栈，而是一种 Web 开发范式。可以随意使用 Jamstack 生态系统中各种组件的任意组合。对于本书，我们将使用三种最流行的软件应用程序和服务来制作一个 Jamstack 网站：GatsbyJS、Sanity.io 和 Netlify。我们还将提供几个示例，使用其他 Jamstack 工具进行比较。

## GatsbyJS

Jamstack 中最受欢迎的部分之一，实际上是一个静态站点生成器，就是 GatsbyJS。

**GatsbyJS**（称为 Gatsby）是开源的，并使用 React JavaScript 库从各种来源编译和构建静态页面。这些来源可能简单到文件系统；一组 Markdown 文件；传统的 CMS 如 WordPress 或 Drupal；或无头 CMS 如 Contentful 或 Sanity。

## Sanity.io

由于 *Jamstack 方式* 的 Web 开发摒弃了单一的单体 CMS，网站所需的实际内容与将对其进行格式化和交付的工具是分开的。

**Sanity.io**（称为 Sanity）是一个结构化内容平台。有一个名为 Sanity Studio 的开源 CMS，它使内容生产者如作家、编辑和摄影师能够轻松创建灵活且模块化的丰富内容，而无需理解编程和标记语言。

## Netlify

网站的实际 *托管* 将在无服务器系统上完成。

**Netlify** 是一项服务，使开发者能够享受完全自动化的编译和部署到 Netlify 的 CDN 上，快速安全地将内容推送到世界各地的服务器。

# 开始使用 Jamstack

现在我们已经了解了 Jamstack 的历史及其优势，接下来我们将查看在本书记述的上下文中使用 Jamstack 所需的工具。

# 安装

为 Jamstack 开发环境安装所有组件并配置一切正常运行对新技术开发者来说会令人望而生畏，因此最好的方法是参考每个组件的网站以获取最新的安装和配置文档。

本书附带一个 GitHub 仓库，其中包含执行所有示例所需的所有代码。因此，首先需要安装的是 Git 版本控制系统。对 Git 不熟悉的用户会发现大量信息、速查表和帮助论坛，但大多数基本操作只需四五个命令即可完成。许多代码编辑器，如 WebStorm，已将命令内置到用户界面中。

为确保安装过程是最新的，提供仓库中的 README.md 文件将提供安装说明和指向每个项目当前文档的链接。

# 先决条件

为了使用本书中的示例，需要几种 Web 开发工具，列于此处：

- Terminal
- Node.js 和 **node package manager** (`npm`)
- Git
- 一个 **集成开发环境** (**IDE**) 或文本编辑器

需要 Terminal 环境来执行命令。应使用哪个 Terminal 应用程序将取决于所使用的操作系统。

Node.js 允许在 Terminal 中运行 JavaScript，而不仅仅是在浏览器中。

npm 对于 Sanity 和 Gatsby 的安装是必要的。这允许安装许多由 JavaScript 功能组成的包。

Git 命令行版本控制系统对于 Gatsby 的安装至关重要，也用于下载本书中的示例。

最后，需要一个提供代码语法高亮和代码检查的 IDE，该检查会持续分析代码的正确性。如果不可能，则需要一个文本编辑器来修改源代码文件。截至本文撰写时，Visual Studio Code 和 WebStorm 是两个流行的 IDE 选择。

# 可选工具

可选地，可以为本书中的示例使用 Yarn。Yarn 是 npm 的兼容替代品，虽然建议使用，但不是必需的。

# 总结

在本章中，我们了解了万维网如何从带有标记和链接的简单页面发展成为庞大、缓慢且不安全的 Web 应用程序。我们了解了 Jamstack 如何结合过去和现在的各个方面，形成一种真正现代、解耦且令人愉快的范式，供 Web 开发者和希望了解更多知识的 Web 设计师使用。

在下一章中，我们将开始构建一个真实、可工作的 Jamstack 应用程序之旅。我们将熟悉 Sanity 和一个基于 Web 的工具，以便使用其中一个模板快速创建示例应用程序。

我们将介绍 Sanity 及其功能，并展示如何修改其模式。我们将展示如何使用 Sanity 设计和构建结构化内容，以创建具有新闻和事件的网站。

