

- *浏览器自动化：*
  - https://www.browserstack.com/guide/what-is-browser-automation
  - https://community.dataquest.io/t/web-scraping-without-selenium/456297
- *自动化练习站点：*
  - https://www.automationexercise.com/
  - https://practicetestautomation.com/practice-test-login/
- *Chromium:* https://chromedriver.chromium.org/documentation
- *Puppeteer, Playwright:*
  - https://pptr.dev/
  - https://playwright.dev/

# 使用正则表达式和 PDF

到目前为止，我们已经学习并探索了一些核心 Python 库在网络通信、内容读取和浏览器自动化方面的应用，用于数据查找和提取。

**正则表达式**（也称为 **Regex**、**regex** 或 **RegEx** —— 在本章剩余部分我们将使用 **regex**）是使用预定义的一组字符构建的，用于搜索和类似活动的一种模式。在 *第 3 章和第 4 章* 中，在进行网页抓取时，我们测试并应用了各种可用功能，例如 *CSS 选择器*、*XPath* 和 *PyQuery*，以查找和定位特定类型的活动。Regex 帮助我们进行模式匹配 —— 在处理文档或任何文本内容时，我们有意识或无意识地大部分时间都在使用 regex。

在数据相关上下文中，很难避免查找、搜索和匹配等活动。Regex 为处理此类活动提供了一种简单而优雅的方法。数据以结构化或非结构化格式存在于各种形式和类型的文档中。**便携式文档格式（PDF）** 是一种安全、功能齐全、压缩良好的文档格式，可轻松通过互联网传输。与其他文本文档格式相比，PDF 不同。我们将在本章学习从 PDF 中提取数据。

在本章中，我们将学习以下主题：

- regex 概述
- 使用 Python 的 regex
- 使用 regex 提取数据
- 从 PDF 提取数据

# 技术要求

需要一个网络浏览器（*Google Chrome* 或 *Mozilla Firefox*），并且我们将使用 *JupyterLab* 进行 Python 代码。

请参考 *第 2 章* 中的 *设置事项* 和 *创建虚拟环境* 部分以继续设置和使用创建的环境。请参考 https://pypdf2.readthedocs.io/en/3.0.0/user/installation.html 安装 **PyPDF2。**

本章所需的 Python 库如下：

- `requests`
- `re`
- `pypdf2`

本章的代码文件可在本书的 GitHub 仓库在线获取：https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter09。

# regex 概述

在很多情况下，使用 XPath 和 CSS 选择器定位某些基于网络的内容或元素是相当困难的，甚至是不可能的。幸运的是，我们可以使用 regex 克服这种情况。regex 是一个使用字符串构建的表达式，用于通过识别现有模式来查找或搜索内容。

在网页抓取和提取相关活动中，regex 也被用作最终或首选的模式匹配选项。模式可以使用各种步骤定义，通常伴随表示预定义规则的特殊符号。regex 就像对纯文本进行分组和书写一样，许多库和文本相关功能都使用这些表达式，为我们提供方便易用的函数。

最新的代码编辑器、文档阅读器和写作程序都提供诸如在文件、多页和项目文件夹内搜索，以及查找和替换等功能。要使用这些选项，我们需要输入文本或 regex；无论如何，模式匹配或 regex 都将用于搜索内容。

regex 通常在其他选项不起作用时使用，有时甚至是首选方法。范围可以扩展到一些重要功能，例如以下内容：

- 将内容拆分为块，也称为爆炸或将内容切片成块
- 查找所有或多个匹配项
- 基于模式替换或替换内容

以下是数据提取过程中 regex 被广泛使用的一些常见用例：

- 验证电子邮件地址
- 从地址中匹配或收集邮政编码或邮编
- 从地理地址中匹配纬度和经度
- 查找页面上列出的项目数量
- 验证电话/手机号码
- 验证日期和时间
- 使用生成的（用户定义）模式提取内容

regex 可以在大多数支持文本的应用程序或服务中定义和运行。外观相当简单、优雅的表达式可用于处理任何类型的用例。其可用性和可扩展性取决于内容，使其成为强大的文本功能。请访问 https://www.regular-expressions.info/ 和 https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions 获取有关 regex 的详细信息。

**重要说明**
基于 Unix/Linux 的 grep（global regular expression print，全球正则表达式打印）命令非常强大，在 Unix/Linux 用户和开发者中很著名。grep 用于匹配和过滤模式，甚至被认为是一种模式匹配引擎。请访问 https://www.gnu.org/software/grep/ 获取更详细信息。

在下一节中，我们将更详细地探索 regex 并在实际示例中使用 re Python 库。

# 使用 Python 的 Regex

Python 编程以其简单、可读、可重用和简短的代码而闻名。Python 也因其科学计算和文本计算功能而流行（**自然语言处理（NLP）**、**情感分析（SA）** 等等）。Regex 也是 Python 的核心能力之一，因为 re（regex 库）作为系统或内置库提供，随 Python 安装即可使用。

让我们深入研究 re 并通过代码查看其中的一些功能。对于此示例，我们将使用 Sadhguru 的以下著名名言（可在 https://isha.sadhguru.org/us/en/wisdom/type/quotes 获取）：

> *如果你不与自己为敌，人类的潜能是无限的。*

我们定义了一个名为 quote 的 Python 变量，其值包含前述引语：

```
quote="If you do not turn against yourself, the Human Potential is limitless"
```

在接下来的部分中，我们将使用 re 方法查找至少三个字符长的单词，或字符串长度为三的单词，这些是数据相关任务中常用的方法。

# re（search、match 和 findall）

Python 的 re 库提供了各种类型的搜索相关方法。一些通常用于在任何代码中搜索模式的常见方法列于此处：

- search(): 在字符串中查找与给定模式匹配的位置并返回第一个匹配对象（re.Match）。如前述代码所示，只返回您或在 quote 字符串中找到的第一个匹配项。
- match(): 此方法在字符串开头查找匹配项。在前述代码的情况下，它不匹配任何内容。re.match() 在成功时也返回各种函数（start、end、span、group、groups 等）和属性（lastindex 和 lastgroup），这在任何情况下都有信息性，但在与 re.finditer() 一起使用时更有用。请访问 https://docs.python.org/3/library/re.html?highlight=regular%20expressions#functions 获取更详细信息。
- findall(): 此方法接受一个模式以在字符串中搜索所有匹配项。它类似于从头到尾迭代 search() 方法。

在即将到来的小节中，我们将学习如何生成 regex 并通过示例应用它。

# 使用字符集的 Regex

在 regex 中，我们可以使用 [] 查找一组字符。如以下代码所示，[a-z] 查找由 a 到 z 字符组成的任何单词组合：

```
re.search(r"([a-z]{3})",quote) # <re.Match object; span=(3, 6), match='you'>
```

在前述代码中，为了固定长度或检查出现次数，我们使用 {}；因此表达式 [a-z]{3} 中的 {3} 正在查找由最多三个字母字符组成的单词的匹配项（例如 you、not 和 the）。

通常，regex 模式以 r 字符开头的字符串提供。r"[a-z]" 是一个字符串，但也是 regex，也称为原始字符串。此外，我们需要小心字符大小写（小写或大写），因为 regex 模式区分大小写。在一般情况下，我们

