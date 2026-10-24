

6. *Step 5* 将部署的代码或项目加载为作业，这些作业将在 **Jobs Dashboard** 下可用，可执行、运行、调度，以及更多选项，如 *Figure 5.10* 所示：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_150_0.png)

Figure 5.10: Listing jobs and Spider links

7. *Step 6* 显示存在 **1 spider**。点击该蜘蛛，将带你进入 **Spider Details**，并提供有关蜘蛛的一些信息。我们现在可以通过从 **Jobs Dashboard** 点击 **Run** 选项，然后在 **Spider Details** 上运行蜘蛛。

8. 你可以通过在 **Completed, Deleted, Running** 和 **Next** 部分点击蜘蛛名称（**booklist**）来验证运行是否成功，如 *Figure 5.11* 所示：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_150_1.png)

Figure 5.11: Spider jobs completed

9. 如 *Figure 5.11* 所示，详细信息如 **Items, Requests, Runtime** 和 **Errors** 也可用，可点击以进行更详细的调查。*Figure 5.12* 向我们展示了 **Items** 下的信息：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_151_0.png)

Figure 5.12: Job Items (loading Item 0 or the first item)

10. 如 Figure 5.12 所示，在 Jobs Items 部分下有多个选项，例如 Download、Exporting to Files（CSV、JSON、XML）、Filtering、Dataset Publishing 和 Samples。

通过上述步骤，我们成功将 Scrapy 项目部署到 app.zyte.com 并根据蜘蛛（booklist）中可用的代码提取了详细信息。

Important note
最好你在本地机器上测试爬虫，以防出现任何异常和错误，测试所有功能（例如运行爬虫和导出数据）并使用最新的库（如果可能）。关于部署或 Zyte 中的任何问题，请访问 https://support.zyte.com。

在本节中，我们专注于在服务器上部署我们的爬虫。

## Summary

在本章中，我们探索并学习了使用 *Beautiful Soup* 和 *Scrapy* 从网页解析和提取数据。

到目前为止，在本书中，我们已经确定了许多有效且适合网页抓取的库和技术。Beautiful Soup 为开发人员提供了一些用于解析、遍历和创建爬虫的功能。Scrapy 提供了与 Beautiful Soup 相同的功能，可用于数据提取，但它更像是一个基于项目的框架，在幕后使用大量库，使你只需专注于自己的任务。由于 Scrapy 易于实现且架构协作性强，它在初学者、专业人士甚至像 Zyte、*ScrapeOps* 和 *Apify* 这样的基于网络的服务提供商中都很受欢迎。

在下一章中，我们将学习并探索更多抓取技术和与安全相关的问题。

## Further reading

- Beautiful Soup: https://www.crummy.com/software/BeautifulSoup/bs4/doc/
- lxml – XML and HTML with Python: https://lxml.de/
- Web crawlers:
  - https://www.cloudflare.com/en-gb/learning/bots/what-is-a-web-crawler/
  - https://research.aimultiple.com/web-crawler/
- Web scraping: https://www.zyte.com/learn/
- robots.txt: https://www.robotstxt.org/robotstxt.html
- Scrapy Cloud: https://www.zyte.com/scrapy-cloud/
- Scrapy: https://docs.scrapy.org/en/latest/index.html

## Part 3: Advanced Scraping Concepts

在这一部分，你将学习如何处理安全或启用安全的网站。你将学习如何从 Web API 抓取数据并使用浏览器自动化工具如 Selenium 来绕过某些强制的安全措施。本部分各章节中有实用示例，将帮助你使用正则表达式从 PDF 文件甚至非结构化格式中提取数据。

本部分包含以下章节：

- *Chapter 6, Working with the Secure Web*
- *Chapter 7, Data Extraction Using Web APIs*
- *Chapter 8, Using Selenium to Scrape the Web*
- *Chapter 9, Using Regular Expressions and PDFs*

## Working with the Secure Web

到目前为止，我们已经学习了有关网络、网络内容、逆向工程技术、数据查找技术、一些 Python 库以及我们可以用来访问和抓取所需网络内容的框架。

如今网络平台上存在大量与安全相关的问题，以及确保安全的措施。存在大量网络应用程序、扩展程序，甚至网络服务提供商来保护我们和我们的网络系统免受未经身份验证的使用和未经授权的访问。

互联网应用程序和电子商务活动的不断增长要求将安全网络（或网络启用安全功能）作为处理有害甚至非法的行为的高优先级。我们经常收到不相关的电子邮件或垃圾邮件，其中包含我们没有要求的信息。

从网页抓取的角度来看，处理此类问题相当具有挑战性，但道德黑客的概念使其更具可行性，即使从应用程序和安全角度来看也是如此。我们将介绍一些处理网络安全的基本概念，然后继续查看网页抓取。

在本章中，我们将学习以下主题：

- Exploring secure web content
- HTML <form> processing using Python
- User authentication and cookies
- Using proxies

### Technical requirements

需要一个网络浏览器（*Google Chrome* 或 *Mozilla Firefox*），我们还将使用 *JupyterLab* 进行 Python 代码。

请参阅 *Chapter 2* 中的 *Setting things up* 和 *Creating a virtual environment* 部分以继续使用创建的环境。

本章所需的 Python 库如下：

- `requests`
- `pyquery`

本章的代码文件可在本书的 GitHub 仓库在线获取：https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter06。

### Exploring secure web content

当今的网络和互联网技术在网络安全方面相当脆弱（内容、授权、非法访问等）。我们希望网络是安全的，我们浏览、搜索或查看的内容是真实的，不违反任何法律或道德标准，也不影响人们的人权。

网络必须对寻求信息的所有人可访问且可用，并且有效且遵循道德实践。我们经常遇到网络内容并不完全是我们所寻找的，或者听说网络内容已被篡改或被黑客入侵，或者私人敏感信息被非法泄露等。虽然很多这些情况超出了我们的控制范围，但我们可以减少漏洞，使网络成为更安全的地方。

在许多新技术中，已经识别出与安全相关的问题并实施了解决方案。甚至还有关注基于网络安全的应用程序和组织，它们的需求正在增长并使用启用安全的技术。在政府部门，开发人员使用最新技术使网络更安全。随着技术的改进，抓取和爬取变得更具挑战性。网页抓取或提取网络上可用的信息在考虑道德问题时并无害处。

在接下来的部分中，我们将介绍一些可在网络上实现且易于获得的基本安全相关概念，然后使用 Python 探索、实现和访问它们。

### Form processing

表单或 HTML `<form>` 处理，也称为表单处理，在大多数情况下用于将收集或用户提供的数据传递或提交到后端或服务器。有大量网络表单（例如登录、联系我们、注册和订阅表单）被冒充他人提交的案例。

从用户角度来看，表单只是一个数据收集器，它有一些 `<form>` 元素，例如 `<select>`、`<option>`、`<textarea>` 和 `<input>`，要求提供一些输入或值。当提供数据时，它会在某个级别得到验证并由服务器或相关系统处理，并进行任何必要的进一步操作（例如用户登录、用户注册、重置忘记的密码、QR 处理和图像验证）。

