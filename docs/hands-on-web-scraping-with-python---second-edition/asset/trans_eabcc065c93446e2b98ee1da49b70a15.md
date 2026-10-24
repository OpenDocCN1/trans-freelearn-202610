

根据 Scrapy 官方网站，https://scrapy.org/，它 *简单、快速、协作且可扩展*。Scrapy 之前由 **Scrapinghub** 维护，现在由 **Zyte**（https://www.zyte.com/）和其他一些贡献者维护。

以下是一些重要的特性，使 Scrapy 流行并使其在 Python 网页爬取框架中脱颖而出的列表：

- 内置支持 *解析、遍历、XPath、CSS 选择器和正则表达式*
- 使用内置库处理 HTTP 请求和响应
- 模块化结构和组件允许开发者专注于特定任务并协作管理编码
- 提供 **命令行界面 (CLI)** 来处理项目、数据导出、管理数据库等
- 可用的中间件和扩展非常丰富，可以轻松处理 *cookie、会话、身份验证、robots.txt、项目日志、用量统计、电子邮件等*

为了开始使用 Scrapy 尝试一些抓取任务，让我们在预设环境中使用 pip 安装它，如图 5.5 所示，或按照 https://docs.scrapy.org/en/latest/intro/install.html 中的说明进行操作：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_143_0.png)

图 5.5：Scrapy 安装

在对 Scrapy 进行本次基本介绍并在系统中成功安装后，接下来我们将设置一个项目。

## 设置项目

Scrapy，如前一节所述，提供了一个命令行工具 scrapy。该工具包含大量命令，可帮助设置项目和与数据提取相关的活动。代码可在 GitHub 上获取：https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter05/bookScrapy。

scrapy genspider booklist books.toscrape.com 命令在 bookScrapy\books\spiders 子文件夹中创建名为 booklist.py 的爬虫，并将 books.toscrape.com 设置为 allowed_domains 参数：

```
import scrapy
class BooklistSpider(scrapy.Spider):
    name = "booklist"
    allowed_domains = ["books.toscrape.com"]
    start_urls = ["http://books.toscrape.com/"]
    def parse(self, response):
```

如前述代码所示，刚创建的 booklist.py 爬虫包含默认代码。要开发爬虫，我们必须在 booklist.py 文件中使用 parse() 实现必要的代码和逻辑。bookScrapy\books 文件夹还包含一些额外的文件：items.py、middlewares.py、pipelines.py 和 settings.py，以及 spiders 文件夹。

> **重要说明**
> Spider 是一个包含用于抓取和数据收集逻辑的代码的 Python 类文件。可以存在多个针对特定抓取活动的 spider 类。spider 类在爬取过程中使用。诸如 scrapy list 和 scrapy list spider list 等命令可列出项目的爬虫。请访问 https://docs.scrapy.org/en/latest/intro/tutorial.html?highlight=Spider#our-first-spider 了解更多信息。

这些文件各自对最终爬虫具有控制和实现。以下是这些文件的简要说明：

- items.py：Item 就像一个 Python 字典，保存键和值（列名和值）。有关更多详细信息，请访问 https://docs.scrapy.org/en/latest/topics/items.html。
- pipelines.py：在收集数据后，抓取的项目会发送到管道以执行其他操作，例如清理和丢弃。有关更多详细信息，请访问 https://docs.scrapy.org/en/latest/topics/item-pipeline.html。
- settings.py：可以控制和添加项目相关设置。有关更多详细信息，请访问 https://docs.scrapy.org/en/latest/topics/settings.html。
- middlewares.py：我们可以指定一些钩子或扩展，可在爬虫中执行其他任务（处理输入和输出）。请访问 https://docs.scrapy.org/en/latest/topics/spider-middleware.html 了解更多详细信息。

> **重要说明**
> 处理或更新我们刚刚提到的文件不是强制性的。这些文件用于实现功能完善的爬取项目，通过允许用户使用 Scrapy 提供的深入功能和变体。

我们现在已准备好项目和默认爬虫文件以开始编码或进一步处理。在下一节中，我们将使用 `items.py` 文件处理 `scrapy.Item`。

## 创建项目

Item 通常被理解为用于收集值以在爬虫中实现的值的字典对象的列名或键。`items.py` 文件包含一个默认类，该类实现 `scrapy.Item`。该类（`BooksItem`）是在处理 `scrapy startproject` 命令期间生成的。

以下代码块探索了 BooksItem 项目类，并与之关联了 `scrapy.Field()` 类型的对象：

```
import scrapy
class BooksItem(scrapy.Item):
    # define the fields for your item here like:
    title = scrapy.Field()
    no_review = scrapy.Field()
    ...
    url = scrapy.Field()
    image = scrapy.Field()
```

根据我们的爬虫计划，`items.py` 将包含 `scrapy.Field()` 的默认对象。这些字段将从爬虫或 `spiders\booklist.py` 获取其值，如我们在下一节中将发现的那样。

## 实现爬虫

在规划和收集项目准备就绪后，我们将实现爬虫以使用 `items.py` 并进一步进行代码实现。代码可在 GitHub 上获取：https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter05/bookScrapy/books/spiders/booklist.py。

与爬取相关的逻辑和代码由 `parse()` 函数提供，该函数存在于 `booklist.py` 中，如下所示：

```
def parse(self, response):
    listings = response.xpath("//article[@class='product_pod']")
    #root
    nextPage = response.xpath("//ul[@class='pager']/li[@class='next']/a/@href")
    for listing in listings:
```

前述代码从 http://books.toscrape.com 识别 listings 并将使用 XPath 进行迭代。下一页的链接 nextPage 也已被解析。CSS 选择器和 XPath 表达式都可与 response（对 start_urls 的 HTTP 响应）一起使用。

如以下代码所示，已从 items.py 创建了 Python BooksItem 类，该类具有列对象，已识别或计划的字段正在被解析并添加到列中：

```
column = BooksItem() # from books.items import BooksItem
column['title'] = listing.xpath("./h3/a/@title[normalize-space()]").extract_first()
column['url'] = listing.xpath("./h3/a/@href").extract_first().strip()
...
yield response.follow('http://books.toscrape.com/catalogue/'+column['url'],
    callback=self.parse_page, meta={'column':column})
```

为了使用 column['url'] 提供的链接解析详细列表页，Scrapy 提供了 follow() 函数，该函数链接到 parse 函数以作为回调处理（parse_page），并且收集的项目被添加到 BooksItem 类的 column 对象中。

以下代码实现了基于循环的逻辑，并在每次有下一页链接可用时调用 parse()：

```
if nextPage:
    nextPage = 'http://books.toscrape.com/catalogue/'+nextPage
    yield scrapy.Request(nextPage, callback=self.parse)
```

此爬虫（booklist）可使用 scrapy crawl booklist 命令执行。它列出了大量日志，例如经过时间和日期。

爬虫准备好处理或爬取后，我们将在下一节中导出数据。

## 导出数据

scrapy 命令使导出数据到文件变得非常容易。我们可以使用以下命令：

- scrapy crawl booklist -o bookRecords.csv
- scrapy crawl booklist -o bookRecords.json

4. 项目创建后，Zyte 将引导您进入 **Deploy** 部分，或您可以从菜单中选择该选项。在这里，我们必须指定如何将项目加载到 app.zyte.com。多个选项可用，如 *图 5.8* 所示：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_149_0.png)

图 5.8：代码与部署 – Scrapy 选项

5. 根据 *步骤 4* 下 **SCRAPY** 中的说明，我们将使用 `pip install shub` 安装 shub，登录并使用 *图 5.9* 中显示的代码进行部署。成功安装和登录将创建 API 密钥，提供部署 ID（638593），并向项目文件夹添加一些文件，例如 `scrapinghub.yml` 和 `setup.py`：

```text
(seconded) C:\HOWScraping2E\Chapter05\book\scrapy>shub deploy 638593
C:\HOWScraping2E\seconded\Lib\site-packages\distutils\hack\__init__.py:33: UserWarning: Setuptools is replacing distutils.
distutils.warn("Setuptools is replacing distutils.")
Packing version 1.0
Deploying to Scrapy Cloud project "638593"
Run your spiders at: https://app.scrapinghub.com/p/638593/
{"status": "ok", "project": 638593, "version": "1.0", "spiders": 1}
```

图 5.9：shub（安装、部署）

