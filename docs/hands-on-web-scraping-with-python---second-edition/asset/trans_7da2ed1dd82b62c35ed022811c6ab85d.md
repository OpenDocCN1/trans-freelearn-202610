

## 使用 Selenium 进行网络爬取

到目前为止，我们在第 1 章到第 7 章中学习了一些 Python 库、Web 和基于 API 的技术、数据查找与元素定位、提取技术以及大量与数据相关的服务。

Selenium 可以自动化浏览器——这句话引自 https://www.selenium.dev/，它主要是一组工具的集合，也被称为测试框架。Selenium 用于自动化 Web（应用程序、网站表单等）以达到测试目的。除了使用自动化进行测试外，还有许多潜在的服务和基于任务的场景可以通过 Selenium 来执行和处理。Selenium 框架由各种模块或组件组成。我们将使用 Selenium WebDriver。

总的来说，我们将安装并学习 Selenium WebDriver，使用 WebDriver 自动化网站，并使用 Selenium 从 Web 上抓取数据。

在本章中，我们将涵盖以下主题：
- Selenium 简介
- 使用 Selenium WebDriver
- 使用 Selenium 进行抓取

### 技术要求

你需要一个网页浏览器（Google Chrome 或 Mozilla Firefox），我们将使用 JupyterLab 来运行 Python 代码。

请参阅第 2 章的“环境设置”和“创建虚拟环境”部分，继续设置和使用所创建的环境。我们将使用 Google Chrome 和 Selenium WebDriver v4.10.0。

要安装 `selenium` Python 库，以下链接会很有帮助：
- `https://selenium-python.readthedocs.io/installation.html`
- `https://www.selenium.dev/documentation/webdriver/getting_started/install_library/`
- `https://pypi.org/project/selenium/`

本章需要以下 Python 库：
- `requests`
- `selenium`
- `re`
- `csv`
- `json`

本章的代码文件可在本书的 GitHub 仓库中在线获取：`https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter08`。

### Selenium 简介

根据**系统开发生命周期（SDLC）**，对基于 Web 的应用程序或系统进行测试是强制性的，这一步骤在应用程序于 Web 上发布之前完成。Selenium 是一个开源项目，它以网页浏览器作为自动化的接口，可用于与 Web 相关或基于 Web 的活动。

使用 JavaScript（JS）、Cookie/会话、其他 JS 脚本以及许多其他 Web 组件的动态和安全 Web 应用程序，都可以借助 Selenium 进行处理、加工、自动化和爬取。*Selenium 是一个包含多种工具和库的综合性项目，用于支持和实现网页浏览器的自动化。*（`https://www.selenium.dev/documentation/overview/`）。虽然它主要用于基于浏览器的自动化，但凡是可以在浏览器中管理或使用的不同场景，都可以通过 Selenium 来解决。这使得 Selenium 成为最受欢迎且备受赞誉的基于浏览器的自动化工具。

Selenium 主要用于 Web 测试。在接下来的章节中，我们将探讨 Selenium 的优点和用法，以及如何将其用于网络爬取。

### Selenium 的优点与缺点

Selenium 通过其组件之一 WebDriver 支持不同的网页浏览器。有许多优点使 Selenium 广受欢迎，其中包括：
- 易于实现
- 支持跨浏览器
- 开源且免费
- 支持并行测试
- 支持多种**操作系统（OS）**
- 支持多种语言（Java、Python、Ruby、PHP 等）
- 拥有大量可用的文档和资源
- 支持远程服务器和云设备

在应用程序测试中使用和实现 Selenium 有很多优势，但也存在一些限制或缺点：
- 处理多个标签页和框架较为复杂
- 执行速度较慢（取决于机器）

尽管 Selenium 提供了大量的优势和可扩展性，但与其一些竞争对手（**Playwright**、**Puppeteer**）相比，它在处理速度上较慢，而且其巨大的内存消耗仍然是一个值得争论的问题，这一话题常在 Web 相关的小组和社区中讨论。

> **重要提示**
> 有关一些精选自动化库和框架的更多详细信息，请参阅 Selenium https://www.selenium.dev/about/、Puppeteer https://pptr.dev/ 和 Playwright https://playwright.dev/。

### Selenium 的使用场景

Selenium 在项目中有许多使用场景。从网络爬取的角度来看，Selenium 既可用于普通场景，也可用于复杂场景。对于普通的抓取场景，我们完全可以使用前面在第 *2 章到第 7 章* 中学到的其他库和技术。

Selenium 用于 Web 上的自动化和测试。在特定情况下，或者当其他库和技术无法完成抓取时，通常会优先选择使用 Selenium（很多时候甚至是最后的选择）。以下是可能需要使用 Selenium 完成抓取任务的一些情况：
- 处理警报、iframe 和弹窗（有时间限制的）
- 收集和使用 Cookie 与会话
- 处理滚动和点击活动（以应对网站上的反机器人措施）
- 处理基于 JavaScript 的网站（具有动态值或动态元素的网站）
- 获取屏幕截图
- 使用无头模式（减少资源消耗）
- 绕过基本身份验证（隐藏或动态值）
- 处理 HTML 表单
- 执行和注入 JavaScript 代码
- 在页面上模拟人类操作

Selenium 是一个框架，或者说是一组基于任务的模块（库）的集合。我们将在下一节介绍这些模块。

### Selenium 的组件

基于 Web 的应用程序测试在开发周期的各个阶段都会进行，甚至会进行多次。这种类型的测试确保开发符合需求或按计划推进。它还有助于发现任何可能的缺陷或错误，并克服或记录它们。

通常，测试是手动（由用户）和/或使用自动化工具（如 Selenium）完成的。自动化测试和验证任务是应用程序生命周期的核心组成部分之一。Selenium 由以下三个主要组件或项目组成（https://www.selenium.dev/projects/）：
- **Selenium WebDriver**：这是一个面向对象（OO）的 API，也是用于自动化浏览器的主要组件之一。浏览器自动化是通过借助内置 API 和其他语言（Java、PHP 和 Python）向第三方浏览器驱动程序（如 Google Chrome、Mozilla 和 Opera）提供命令来实现的。**WebDriver** 允许程序或脚本以原生方式访问浏览器，就像用户操作一样。对于与抓取相关的任务，我们通常会与 WebDriver 打交道，我们将在“使用 Selenium WebDriver”部分探讨其功能。有关 WebDriver 的更多详细信息，请访问此链接：https://www.selenium.dev/documentation/webdriver/。
- **Selenium IDE**：这是一个即装即用的浏览器扩展，也被称为 Selenium 的**用户界面（UI）**，用于记录用户在浏览器中的操作。IDE 还提供了一个功能，可以回放已记录的操作，同时包含所部署的命令和带值的已定义参数。它还具有调试功能，如设置断点、通过 IDE 处理异常、创建可运行命令的脚本以及支持控制流结构。此扩展适用于 Google Chrome 和 Mozilla Firefox。有关 Selenium IDE 的更多详细信息，请访问此链接：https://www.selenium.dev/documentation/ide/。
- **Selenium Grid**：这允许跨多台机器、多种浏览器以及跨平台执行测试或自动化操作。Grid 还支持基于组件的配置，可部署在多台机器或平台上。基本上，Selenium Grid 支持分布式测试。这有助于缩短测试时间，并行识别不同系统的性能问题。有关 Selenium Grid 的更多详细信息，请访问此链接：https://www.selenium.dev/documentation/grid/。

通过以上对 Selenium 的简要概述和介绍，我们将在下一节中安装 Selenium WebDriver，并通过代码和示例进行探索。

### 使用 Selenium WebDriver

Selenium 用于浏览器自动化，其重要组件之一 WebDriver 是访问浏览器的核心工具。WebDriver 为选定的浏览器实现了自动化过程中所需的代码逻辑。它也是将 Selenium 框架与浏览器绑定在一起的核心系统，通常被称为 **Selenium 驱动** 或简称为 **driver**。更详细的信息，请访问此链接：https://www.selenium.dev/documentation/webdriver/getting_started/。

在深入了解自动化或使用该框架之前，让我们先在下一节安装所需的库。

### 环境设置

要使用 Python 和 Selenium WebDriver 探索浏览器自动化，首先需要安装 `selenium` 库（一个 Python 库）以及与浏览器相关的驱动程序。

> **重要提示**
> Selenium 是一个包含 WebDriver 等各类组件的框架，而 `selenium` 是一个 Python 库（https://www.selenium.dev/documentation/webdriver/getting_started/install_library/），我们使用它来编写和维护绑定 WebDriver 与所选浏览器的代码和逻辑。

