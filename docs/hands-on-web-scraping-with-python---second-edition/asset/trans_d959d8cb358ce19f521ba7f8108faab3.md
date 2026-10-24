

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_69_0.png)

图 2.31：带有表单数据的 Payload（代码中使用的一个实际 params 键值对）

如图 2.31 所示，表单数据与图 2.30 中的 params 类似。始终建议通过浏览器和 DevTools 学习、记录并检测任何站点的请求/响应序列。

我们可以从 DevTools 中获取大量信息，并使用这些信息来部署脚本和自动化流程。在当前场景中，图 2.32 显示了 Headers 中 General 和 Request Headers 部分所包含的最大信息量：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_69_1.png)

图 2.32：HTTP POST 请求中使用的请求 URL、方法和请求头

本部分中的示例和图片涉及 HTTP POST 方法的功能，以及 POST 及提供给 POST 的参数与 GET 方法相比有何不同。实现 HTTP 方法是 HTTP 活动的核心。有关 HTTP 方法的更多详细信息，请访问 https://requests.readthedocs.io/en/latest/api/ 和 https://www.w3schools.com/tags/ref_httpmethods.asp。

## 小结

在本章中，我们学习了 Python 编程、搭建虚拟环境，以及安装 Python 库来向 Web 资源发送请求并收集响应和其他一些信息。

本章的主要目标是展示 requests 库的核心功能。我们的主要目标是学习如何处理 HTTP 请求和响应循环、如何使用 HTTP 方法（带额外参数）、如何使用 DevTools，以及在这个领域中使用 Python 的好处。

在下一章中，我们将学习并使用一些基本技术，从 Web 内容中识别和提取数据。

## 延伸阅读

- **Python 编程：**
  - https://www.python.org/doc/
  - https://www.w3schools.com/python/default.asp
- **数据与数据分析：**
  - https://www.diesel-plus.com/the-importance-of-data-collection-10-reasons-why-data-is-so-important/
  - https://www.simplilearn.com/data-analysis-methods-process-types-article
  - https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8274472/
  - https://scientific-publishing.webshop.elsevier.com/research-process/when-data-speak-listen-importance-of-data-collection-and-analysis-methods/
- **requests、urllib、JSON：**
  - https://requests.readthedocs.io/en/latest/
  - https://pymotw.com/3/
- **虚拟环境：**
  - https://docs.python.org/3/library/venv.html
  - https://docs.python-guide.org/dev/virtualenvs/
  - https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html
  - https://docs.anaconda.com/free/navigator/tutorials/manage-environments/
- **Jupyter Notebooks：**
  - https://jupyter.org/
  - https://code.visualstudio.com/docs/datascience/jupyter-notebooks
- **Cookies：**
  - https://www.cloudflare.com/en-gb/learning/privacy/what-are-cookies/
  - https://www.cookieyes.com/blog/session-cookies/
- **URL 和 HTTP：**
  - https://blog.hubspot.com/marketing/parts-url
  - https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview

## 第 2 部分：
开始 Web 爬取

在本部分中，你将学习如何分析、规划和处理来自目标网站的所需或必需数据集合，将数据收集或写入所需的文件格式。你将使用 Python 及其选定的库来编写爬虫。本部分中的章节将介绍可以有效且高效完成的爬取实践的各个方面。

本部分包含以下章节：

- *第 3 章，搜索与处理 Web 文档*
- *第 4 章，使用 PyQuery（一个类似 jQuery 的 Python 库）进行爬取*
- *第 5 章，使用 Scrapy 和 Beautiful Soup 爬取 Web*

## 搜索与处理 Web 文档

到目前为止，我们已经了解了 Web 爬取、数据查找技术以及有助于我们进行爬取的相关技术，并且也确定了选择 Python 编程语言的一些原因。

基于 Web 或网站的内容以 HTML 元素、预定义文档或某种对象（JSON）的形式存在。为了提取，我们需要分析和识别此类内容、模式和对象。基于 HTML 的元素通常通过 XML 路径（XPath）和层叠样式表（CSS）选择器来识别，并通过爬取逻辑对所需内容进行遍历和处理。本章将使用 lxml 库来处理标记文档。我们将使用基于浏览器的开发者工具（DevTools）来查找内容和识别元素。

具体而言，在本章中我们将学习以下主题：

- 介绍用于处理标记文档的 XPath 和 CSS 选择器
- 使用 Web 浏览器 DevTools 访问 Web 内容
- 使用 lxml（一个 Python 库）进行爬取
- 解析 robots.txt 和 sitemap.xml

## 技术要求

需要 Google Chrome 或 Mozilla Firefox 浏览器，我们将使用 JupyterLab 和 Python Notebook。

请参阅第 2 章的“环境搭建”和“创建虚拟环境”部分，并继续使用我们创建的环境。

本章所需的 Python 库如下：

- `lxml`
- `urllib`

本章的代码文件可在 GitHub 上获取：`https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter03`。

> **重要提示**
> 
> 基于 Web 或网站的内容是指向 URL 发送请求后收到的响应或页面源代码。内容的类型可以多种多样，如 PDF、CSV、TXT、XML、HTML 和 JSON。一般来说，在本章中，除非另有说明，否则我们将 HTML、页面源代码或标记文档作为主要内容进行讨论。

## 介绍用于处理标记文档的 XPath 和 CSS 选择器

在 *第 1 章* 的 *理解 Web 页面中使用的最新 Web 技术和数据查找技术* 各节中，我们探讨并讨论了 **HTML** 和 **XML** 标记文档及其在整个 Web 中的可用性。

通常，标记是一种对文档中的部分、章节或任何实体进行标注或加标签的方式，有助于识别内容，甚至可以使用第三方应用程序处理内容。在 HTML 中，我们称它们为**标签**（`https://www.w3.org/html/`），在 XML 中称为**节点**（`https://www.w3.org/standards/xml/`）。因此，标记文档是一种树状结构，包含标签或节点（嵌套或单独的），这种结构也称为**元素树**。

> **重要提示**
> 
> 自互联网时代开始发展以来，XML 文档在 Web 上一直非常流行和普遍。可读性、编码支持、互操作性和数据可交换性是 XML 的一些核心优势。最新的 Web 技术仍然支持 XML，XML 也是标记文档最重要的支柱。

在了解了这里关于标记和 XML 的简要概述之后，接下来的章节将介绍**文档对象模型（DOM）**、XPath 和 CSS 选择器，并借助 Python 库将它们用于 Web 爬取。

## 文档对象模型（DOM）

**树型结构**或元素树是大多数标记语言的基础模型，通常被称为 DOM。请访问以下链接以了解有关 DOM 的更多详细信息：https://www.w3.org/TR/WD-DOM/introduction.html 和 https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model。

DOM 处理、DOM 解析、DOM 操作以及更多类似术语指的是与 DOM 相关的活动。借助 DOM 及其定义的约定，我们可以访问、遍历和操作标记文档。许多与 Web 相关的技术都支持 DOM 的使用，例如 **JavaScript（JS）**、模板引擎和网页开发工具。

DOM 元素（标签或节点）大多数时候是预定义的或用户自定义的。我们可以在标记文档中找到开放型和闭合型约定。元素在父元素内部以结构化或嵌套的形式存在，也可能同时是某些其他元素的父元素或子元素。这种由 DOM 元素组成的树状结构基于某种语言或约定，可用于构建标记文档，如图 3.1 所示：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_77_0.png)

图 3.1：HTML DOM 树结构

理解 DOM 或元素树（HTML 树或 XML 树）是理解文档作为元素容器并遵循提取过程的基本步骤。信息可能嵌套在树结构中，并可能带有表示内容的属性以及其他附加信息。有关更多详细信息，请参阅 *第 1 章的 HTML 部分*。

XPath 和 CSS 选择器用于沿 DOM 导航，并用于在找到的节点或元素中搜索所需内容。我们将在下一节中探讨 XPath。

