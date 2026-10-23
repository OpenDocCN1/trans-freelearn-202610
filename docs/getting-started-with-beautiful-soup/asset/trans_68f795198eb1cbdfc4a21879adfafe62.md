

第 3 章《使用 Beautiful Soup 进行搜索》详细讨论了 Beautiful Soup 中的不同搜索方法，即 find()、find_all()、find_next() 和 find_parents()；使用搜索方法从网站获取信息的抓取器代码示例；以及理解搜索方法组合的应用。

第 4 章《使用 Beautiful Soup 进行导航》详细讨论了 Beautiful Soup 提供的不同导航方法，以及专门用于向下、向上和向侧面导航到 HTML 树的前一个和后一个元素的方法。

第 5 章《使用 Beautiful Soup 修改内容》讨论了使用 Beautiful Soup 修改 HTML 树，以及 HTML 标签的创建和删除。还通过简单示例介绍了更改 HTML 标签属性。

第 6 章《Beautiful Soup 中的编码支持》讨论了 Beautiful Soup 中的编码支持，为具有特定编码的页面创建 BeautifulSoup 对象，以及输出的编码支持。

第 7 章《Beautiful Soup 中的输出》讨论了 Beautiful Soup 中的格式化和非格式化打印支持，不同格式化程序的规范以格式化输出，以及从 HTML 页面获取纯文本。

第 8 章《创建网络抓取器》讨论了为三个网站创建网络抓取器，即 Amazon、Barnes and Noble 和 PacktPub，以根据 ISBN 获取图书销售价格。用于创建解析器的搜索和导航方法、使用开发者工具以识别创建解析器所需的模式，以及抓取上述网站的完整代码示例也在本章中进行了说明。

## 本书所需条件

本书需要 Python 版本 2.7.5 或更高版本以及 Beautiful Soup 版本 4。

对于第 3 章《使用 Beautiful Soup 进行搜索》和第 8 章《创建网络抓取器》，你必须拥有互联网连接，以便使用提供的代码示例抓取不同的网站。

## 本书适合人群

本书适合使用 Beautiful Soup 进行网络抓取的初学者。了解 Python 编程的基础知识（如函数、变量和值），以及 HTML 和 CSS 的基础知识，对于跟随本书中的所有步骤非常重要。尽管不是强制性的，但在学习第 3 章和第 8 章中的抓取示例时，了解如何在浏览器（如 Google Chrome 和 Firefox）中使用开发者工具将是一个优势。

## 惯例

在本書中，你会找到多种文本样式，用以区分不同类型的信息。以下是这些样式的示例，以及它们的含义。

文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号显示如下：

“`prettify()` 方法可以调用在 Beautiful Soup 对象或任何 Tag 对象上。”

代码块设置如下：

```
html_markup = """<html>
    <body>&  &amp;  ampersand
        ¢  &cent;  cent
        ©  &copy;  copyright
        ÷  &divide;  divide
        >  &gt;  greater than
    </body>
</html>
"""
soup = BeautifulSoup(html_markup,"lxml")
print(soup.prettify())
```

当我们希望引起你对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
UserWarning: "http://www.packtpub.com/books" looks like a URL.
Beautiful Soup is not an HTTP client. You should probably use
an HTTP client to get the document behind the URL, and feed
that document to Beautiful Soup
```

任何命令行输入或输出写成如下形式：

```
sudo easy_install beautifulsoup4
```

**新术语和重要词语**以粗体显示。你在屏幕上看到的词语，例如菜单或对话框中出现的词语，在文本中如下所示：“Beautiful Soup 中的输出方法仅转义 >, < 和 & 的 HTML 实体，分别转义为 &gt;、&lt; 和 **&amp;。**”

> 警告或重要说明以如下框显示。

> 提示和技巧如下所示。

## 读者反馈

我们始终欢迎读者的反馈。请让我们知道你对本书的看法——你喜欢什么或可能不喜欢什么。读者反馈对我们开发你真正受益的书名非常重要。

如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中提及书名即可。

如果你在某个主题方面有专业知识，并且有兴趣撰写或为一本书做出贡献，请参阅我们位于 www.packtpub.com/authors 的作者指南。

## 客户支持

现在你已成为 Packt 书籍的自豪拥有者，我们有一系列资源可帮助你充分利用你的购买。

### 下载示例代码

你可以从 http://www.packtpub.com 下载所有 Packt 书籍的示例代码文件。如果你是在其他地方购买本书，可以访问 http://www.packtpub.com/support 并注册，以便将文件直接发送到你的电子邮件。

### 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果你发现我们的书籍中存在错误——可能是文本或代码中的错误——我们将非常感激你向我们报告。通过这样做，你可以节省其他读者的挫败感，并帮助我们改进本书的后续版本。如果你发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择你的书籍，点击勘误提交表单链接，并输入勘误详情。一旦你的勘误得到验证，你的提交将被接受，勘误将被上传到我们的网站，或添加到该标题现有勘误列表中，在该标题的勘误部分下。任何现有勘误均可通过 http://www.packtpub.com/support 选择你的标题进行查看。

### 盗版

互联网上版权材料的盗版是一个持续存在的问题，涉及所有媒体。在 Packt，我们非常重视对版权和许可的保护。如果你发现我们在互联网上以任何形式存在非法复制品，请立即提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并附上疑似盗版材料的链接。

我们感谢你的帮助，保护我们的作者，并使我们能够为你提供有价值的内容。

### 问题

如果你在本书的任何方面遇到问题，可以联系我们 questions@packtpub.com，我们会尽力解决。

## 1 安装 Beautiful Soup

在开始使用 Beautiful Soup 之前，我们应确保它已正确安装在我们的机器上。所需步骤非常简单，任何用户都可以迅速完成安装。在本章中，我们将涵盖以下主题：

- 安装 Beautiful Soup
- 验证 Beautiful Soup 的安装

### 安装 Beautiful Soup

Python 支持安装第三方模块，如 Beautiful Soup。在最佳情况下，我们可以期望模块开发者可能已准备好平台特定的安装程序，例如在 Windows 的情况下为可执行安装程序；在 Red Hat 为基础的 Linux 操作系统（如 Red Hat、Open Suse 等）的情况下为 rpm 包；在 Debian 为基础的操作系统（如 Debian、Ubuntu 等）的情况下为 Debian 包。但这并不总是如此，如果平台特定的安装程序不可用，我们应了解替代方案。我们将讨论在不同操作系统（如 Linux、Windows 和 Mac OS X）中可用的不同 Beautiful Soup 安装选项。我们将在后面的示例中使用的 Python 版本为 Python 2.7.5，用于安装 Beautiful Soup 的说明可能与 Python 3 不同。你可以直接前往对应操作系统的安装部分。

### 在 Linux 中安装 Beautiful Soup

在 Linux 机器上安装 Beautiful Soup 非常简单直接。对于 Debian 或 Ubuntu 的最新版本，Beautiful Soup 可作为包提供，我们可以使用系统包管理器进行安装。对于 Debian 或 Ubuntu 的其他版本，如果 Beautiful Soup 不作为包提供，我们可以使用替代方法进行安装。

