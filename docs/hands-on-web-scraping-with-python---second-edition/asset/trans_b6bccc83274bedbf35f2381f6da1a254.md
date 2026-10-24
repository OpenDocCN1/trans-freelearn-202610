

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_43_0.png)

Figure 1.12: Security panel (details about certificate, connection, and resources)

在探索了 HTML 页面源代码和 DevTools 之后，我们现在对数据和请求/响应相关信息存储的位置以及如何访问它有了一些概念。总的来说，抓取过程涉及从网页中提取数据，我们需要识别或定位具有数据或可以承载数据的资源。在进行数据探索和内容识别之前，识别页面 URL、DevTools 资源、XHR、JavaScript 以及浏览器活动概览是有益的。

最后，还有更多与链接、子页面相关的话题。我们将在第 3 章中深入使用 sitemaps.xml 和 robots.txt 等技术。

> **Important note**
> 有关 sitemaps.xml 和 robots.txt 的基本概念，请访问 Sitemaps 网站 (https://www.sitemaps.org) 和 Robots Exclusion Protocol 网站 (http://www.robotstxt.org)。

在本章中，您学习了网络抓取、涉及的网络技术，以及如何使用数据查找技术。

## Summary

网站本质上是动态的，因此本章介绍的基本活动在大多数情况下都适用。我们还解释并探索了一些与万维网 (WWW) 和网络抓取相关的核心技术。使用 DevTools 和页面源来识别或查找目标内容是本章的重点。这些信息将指导您在网络抓取中采取初步和专业的步骤。

在下一章中，我们将使用 Python 编程语言与网络或数据源交互，并探索我们为数据提取而选择的几个主要库。

## Further reading

- *HTML:* https://developer.mozilla.org/en-US/docs/Web/HTML
- *CSV:* https://docs.python.org/3/library/csv.html
- *JSON:*
  - https://www.json.org/json-en.html
  - https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON
- *XML:*
  - https://aws.amazon.com/what-is/xml/
  - https://www.w3.org/XML/
- *HTTP:*
  - https://developer.mozilla.org/en-US/docs/Web/HTTP
  - https://http.dev/methods
  - https://www.rfc-editor.org/rfc/rfc9110.html
- *JavaScript:*
  - https://www.javascript.com/
  - https://developer.mozilla.org/en-US/docs/Web/JavaScript
- *jQuery:*
  - https://jquery.com/
  - https://www.w3schools.com/jquery/default.asp
- *Browser developer tools:*
  - https://developer.chrome.com/docs/devtools/
  - https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Tools_and_setup/What_are_browser_developer_tools
- *Web technology:* https://developer.mozilla.org/en-US/docs/Web
- *Reverse engineering:* https://insights.globalspec.com/article/7367/how-does-reverse-engineering-work

## Technical requirements

我们将使用安装在 Windows 上的 Python 3.11.1。代码编辑器的选择很多；选择一个方便您使用且能处理本章代码示例中使用的库的编辑器。我们将并行使用来自 Jupyter 的 Notebooks 和 JupyterLab (https://jupyter.org/)、集成开发和学习环境 (IDLE)（来自 Python 的默认编辑器）以及 Windows 的命令提示符 (cmd)。

要跟上本章的学习，您需要安装以下应用程序：

- Python 3.11.* 或适用于您操作系统的最新版本，来自 https://www.python.org/downloads/
- pip Python 包管理工具 (https://pypi.org/project/pip/)
- JupyterLab (https://jupyter.org/install) 用于访问 .ipynb 扩展名的笔记本或文件
- Google Chrome、Mozilla Firefox 或任何其他配备开发者工具 (DevTools) 的网络浏览器
- JetBrains PyCharm (https://www.jetbrains.com/pycharm/)、Visual Studio Code (https://code.visualstudio.com/) 或您习惯使用的任何编辑器

本章所需的 Python 库如下：

- requests
- urllib

本章的代码文件可在 GitHub 上在线获取：https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter02。

## Why Python (for web scraping)?

Python 是一种流行的编程语言，用于编写各种类型的应用程序，从简单的脚本到软件开发、ML/AI 算法和 CLI/GUI 应用，以及创建 Web 应用程序。Python 简单的面向对象编程 (OOP) 语法非常易读，这使得开发人员可以用不多行代码编写代码并在不同平台上工作（这意味着 Python 是平台独立的），使其成为编程语言中的首选。

Python 是当今编程领域的热门词汇。相当多的工具和 Web 应用程序都是使用 Python 生产和管理的，它兼容小型和大型应用，并由其全球开发者社区提供充足且最新的库。

网络抓取需要各种工具和技术来从网站收集数据，以满足个人或业务需求，但同时要考虑到任何法律限制。使用 Python，我们可以使用许多有效的库来访问和管理项目，例如 `requests`、`http`、`urllib`、`json`、`csv` 和 `cookielib`。

Python 开发以及 Python 语言本身都在不断增长并定期更新。它解决了大多数市场需求，帮助用户提高生产力，同时为开发人员提供有效高效的库。在撰写本书时，Python 的版本 3.11.1 可在 `https://www.python.org` 获取，如 *Figure 2.3* 所示：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_48_0.png)

Figure 2.3: Python website (latest versions, jobs, and documentation)

Python 还拥有在全球备受推崇的用于各种任务和领域的库。这些库在 Python 社区中很有名：

- `requests`、`http`、`urllib`、`cookielib` 和 Starlette：用于 **HTTP** 通信
- `lxml`、PyQuery 和 BeautifulSoup：用于 **HTML**、**XML** 和 **DOM** 解析
- `json` 和 `csv`：用于文件管理和内容管理
- `numpy` 和 `pandas`：用于处理和分析数值和表格数据
- `scipy`：用于科学计算
- `nltk`、textblob 和 vader：用于 NLP
- PyTorch：一个开源 ML 框架
- `scikit-learn`：用于预测性数据分析的高效工具
- `matplotlib`、seaborn 和 plotly：可视化库
- PySpark：用于处理大数据、流式传输和块管理
- FastAPI、Flask 和 Django：基于 Web 的框架
- Pydantic 和 Pandera：数据验证和数据测试工具包
- PyArrow：用于互操作性和集成 Python 对象

有关这些库的更详细信息，请访问 https://www.python.org、https://pypi.org 和 https://pymotw.com/3/。

在本节中，您学习了 Python、其流行度、一些库以及 Python 在顶级公司中的使用。在下一节中，我们将设置 Python、安装所需的库并使用 Python 与 Web 通信。

## Accessing the WWW with Python

正如我们在 Why Python (for web scraping)? 部分所看到的，有许多 Python 库可用于与 HTTP 交互。requests 和 urllib 是我们感兴趣使用的两个库，因为它们具有深入的功能、处理 HTTP 通信的各种函数、易于阅读的文档和流行度。

为了开始使用这些库通过 Python 访问万维网，让我们验证是否已安装所有必需资源。在以下小节中，我们将开始设置，例如安装 Python、创建虚拟环境、在创建的环境中安装库以及使用 Python 库访问 Web。

### Setting things up

假设您系统上已安装了最新版本的 Python。如果没有，请访问 https://www.python.org/downloads/ 获取适用于您操作系统的最新 Python 版本。关于通用设置和安装程序，请访问 https://docs.python.org/3/using/windows.html 并按照详细步骤操作。

> **Important note**
> 在安装期间，Python 用户可以选择 **Customize Installation** 并选择诸如 **Add Python... to PATH** 之类的功能。此外，对于初学者和中级用户，建议使用易于定位的位置或根文件夹来设置 Python，例如 C:\Python311 或 D:\Python\Py311。

我们将在 Windows 上使用 Python 3.11.1。安装完成后，我们可以使用以下步骤确认 Python 在操作系统中的可用性：

1. 按 Windows + R 打开运行窗口并输入 cmd 以加载命令行界面 (CLI)。
2. 移动到您的根目录或所需路径。
3. 输入 python -h。这将加载 Python 选项，如 Figure 2.4 所示：

