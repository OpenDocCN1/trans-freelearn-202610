# Beautiful Soup 入门指南

## 关于作者

**Vineeth G. Nair** 毕业于喀拉拉邦科钦的 Model Engineering College，获得计算机科学与工程学士学位。目前他担任 Oracle India Pvt. Ltd. 的高级应用工程师。

他在大学期间对 Python 产生了兴趣，并开始做自由程序员。这使他参与了一些使用 Beautiful Soup 的网页抓取项目，帮助他在该技术上达到相当熟练的程度，并在自由职业领域赢得了良好声誉。他可通过 `vineethgnair.mec@gmail.com` 联系，也可访问他的网站 `www.kochi-coders.com`。

我衷心感谢 Beautiful Soup 的主要作者 Leonard Richardson。感谢我的朋友和家人对编写本书的大力支持和鼓励。特别感谢 Vijitha S. Menon，她总是让我保持高昂的情绪，提供宝贵的意见，并指导我以最佳方式完善本书。也衷心感谢所有评审人员的建议、纠正和改进意见。

我对 Packt Publishing 团队的帮助表示感激，正是他们促成了本书的出版。

## 关于评审人员

**John J. Czaplewski** 是威斯康星州麦迪逊市的一名制图师和 Web 开发人员，专门从事基于 Web 的制图、GIS、数据处理与可视化。他曾就读于威斯康星大学麦迪逊分校，获得政治学文学学士学位和 GIS 研究生证书。目前他是 UW-Madison 地球科学系的程序员分析师，致力于数据可视化、数据库和 Web 应用程序开发。在不坐在电脑前时，他喜欢攀岩、骑行、徒步、旅行、制图、语言学习以及几乎所有与技术相关的事物。

**Christian S. Perone** 是一位经验丰富的 Python 开发者、开源协作贡献者，同时也是 PyEvolve 的项目负责人。PyEvolve 是一个非常流行的进化计算框架，被选为 OpenMDAO 的一部分，而 OpenMDAO 是 NASA 格伦研究中心的项目。他做程序员已有 12 年，使用过包括 C、C++、Java 和 Python 在内的多种语言。他为许多开源项目做出了贡献，热爱网页抓取、开放数据、Web 开发、机器学习和进化计算。目前，他居住在巴西阿雷格里港。

**张翔** 是新浪公司的工程师。

> 我要感谢我的女朋友，她一直支持我。

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
