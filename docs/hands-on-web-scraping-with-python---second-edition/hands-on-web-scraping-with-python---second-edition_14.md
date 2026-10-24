## 第5部分：结论

在本部分中，你将了解一些精选的高级概念，这些概念可以结合抓取的数据或数据集实现，或在其之上实现，以完成高级处理和功能。你还将了解与网络抓取和数据科学相关的、在全球范围内需求很高的工作和职业发展方向。

本部分包含以下章节：

- *第12章，抓取之后——后续步骤与数据分析*

## 12

## 抓取之后——后续步骤与数据分析

到目前为止，我们已经学习了如何抓取网页、分析收集的数据，并使用 Python 编程应用**机器学习（ML）**算法。

本章将简要介绍一些在该领域日益重要的新兴概念和技术。从数据中获取洞察和知识的能力需求旺盛，甚至相关供给领域也在呈指数级增长。从研究领域到商业领域，数据的重要性迅速提升。由机器学习逻辑驱动的**人工智能（AI）**系统将继续消耗并生成更多数据。

在本章中，我们将学习以下主题：

- 抓取之后会发生什么？
- 网络请求
- 数据处理
- 工作与职业

## 技术要求

需要使用网络浏览器（*Google Chrome* 或 *Mozilla Firefox*）来浏览所提供的资源。

## 抓取之后会发生什么？

网络抓取是一种收集和提供高质量数据的方法，并应用各种依赖数据的技术。任何涉及数据的学科或领域都提供了学习和成长的机会，因为通过抓取获得的数据被用于分析、报告、数据集创建和数据挖掘等许多其他任务。随着基于网络的技术的发展，与网络抓取相关的技术一直是动态的且充满挑战。

基于数据的系统以图像和视频的形式返回原始数据、处理后的数据和可视化图表，正通过多种形式吸引全球受众而不断发展。**信息技术（IT）**驱动的系统是所有行业都在使用的核心应用，Python 编程在数据相关系统的开发和处理中发挥了重要作用。

本书前面的章节介绍了几个步骤，这些步骤整合了如下一些主要概念和操作：

- 数据需求
- 数据收集
- 数据分析（信息生成）
- 数据使用（ML/AI）

大量框架和编程语言正在被开发。与数据、信息和知识管理相关的应用在商业和研究领域中使用最为广泛。

虽然我们已经讨论了网络抓取和数据收集的各种特性和技术，但鉴于各种实体不断被创建和演变，能够调整我们的方法和技术非常重要。一些因素，例如系统的有效性、时间消耗、内容量以及适用技术的可用性，会影响整个网络抓取过程，甚至影响数据收集后执行的步骤。

在下一节中，我们将介绍一些处理与网络相关请求的高级技术和 Python 库。

## 网络请求

在本书的各个章节中，一直使用 `requests` Python 库在代码和网络之间建立通信。如果在 https://pypi.org/ 上搜索与 `requests` 类似的库，可以找到大量 Python 库。

以下小节列出了一些 Python 库和技术，并对其进行了简要介绍。

## pycurl

pycurl Python 库（http://pycurl.io/）是基于流行的 libcurl 库的封装。libcurl 是最早用于与互联网网站通信的 Python 库之一，它基于 **curl** 工具（也称为 **cURL**）。

`curl (https://curl.se/)` 是一个用于在网络上连接和传输数据的命令行工具。`curl` 是网络通信的基础；它是一个核心实现，借助封装器，可用于不同的**操作系统（OS）**、浏览器以及与互联网通信的机器。curl 命令与机器无关。Python、PHP、C、R、Java 和 Rust 等编程语言会将原始 curl 命令转换为其首选格式，以便在其代码或库中使用。

pycurl 使用 C 编程语言开发，为 Python 代码提供基于 curl 的功能。与其他库相比，requests 库通常被认为易于使用。pycurl 提供了与 curl 几乎相同的特性和功能，包括其属性和方法。

以下是 pycurl 相对于 requests 的一些主要优势：

- 支持大量通信协议（HTTPS、POP3、FTP、LDAP、IMAP 等）
- 与 requests 和其他类似 Python 库相比更快

现代浏览器甚至提供原始 curl 命令，可以使用基于浏览器的开发者工具找到，如前面第 2 章所述。这些 curl 命令可以被复制并用于以下场景：

- **命令行界面（CLI）**应用程序（Bash shell 或脚本）或终端
- 基于 Web 的终端和 API 工具，例如 **Postman**（https://www.postman.com/）
- 带有请求头的 Python 库，例如 requests（https://curlconverter.com/python/）
- 将命令转换为其他编程语言

图 12.1 展示了使用“Copy as cURL (bash)”选项从基于浏览器的开发者工具中复制得到的用于加载 https://curl.se/ 的 curl 命令。

```
curl 'https://curl.se/' \
  -H 'authority: curl.se' \
  -H 'accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7' \
  -H 'accept-language: en-US,en;q=0.9' \
  -H 'cache-control: max-age=0' \
  -H 'if-modified-since: Thu, 01 Jun 2023 10:05:02 GMT' \
  -H 'if-none-match: "2153-5fd0e9e9a8fc-gzip"' \
  -H 'sec-ch-ua: "Not.A/Brand";v="8", "Chromium";v="114", "Google Chrome";v="114"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "Windows"' \
  -H 'sec-fetch-dest: document' \
  -H 'sec-fetch-mode: navigate' \
  -H 'sec-fetch-site: none' \
  -H 'sec-fetch-user: ?1' \
  -H 'upgrade-insecure-requests: 1' \
  -H 'user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36' \
  --compressed
```

图 12.1：curl 命令





> **重要说明**
>
> 使用 **Copy as cURL (bash)** 选项几乎就像从浏览器开发者工具中复制 *请求头*。浏览器会提取、美化并以某种格式呈现此类信息。**Postman** 也允许导入 cURL 命令并以 *HTML <form>*-based 布局呈现（可编辑文本框等），在其中开发者可以做出选择并进行网络请求，同时并行开展测试。

通过浏览器获得的 cURL 命令，如图 12.1 所示，可以复制到 https://curlconverter.com/python 并转换为可用于 `requests` 和 `http.client` 等库的 Python 代码。以下代码是为 requests 转换/生成的 Python 代码：

```
import requests
headers = {
    'authority': 'curl.se',
    'accept': 'text/html,application/xhtml+xml,
               application/xml;q=0.9, image/avif,
               image/webp, image/apng,*/*;q=0.8,
               application/signed-exchange;v=b3;q=0.7',
    'accept-language': 'en-US,en;q=0.9',
    'cache-control': 'max-age=0',
    'if-modified-since': 'Thu, 01 Jun 2023 10:05:02 GMT',
    'if-none-match': '"2153-5fd0e91e9a8fc-gzip"',
    'sec-ch-ua': '"Not.A/Brand";v="8", "Chromium";v="114",
                "Google Chrome";v="114"',
    'sec-ch-ua-mobile': '?0',
    'sec-ch-ua-platform': '"Windows"',
    'sec-fetch-dest': 'document',
    'sec-fetch-mode': 'navigate',
    'sec-fetch-site': 'none',
    'sec-fetch-user': '?1',
    'upgrade-insecure-requests': '1',
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64;
                   x64) AppleWebKit/537.36 (KHTML, like Gecko)
                   Chrome/114.0.0.0 Safari/537.36',
}
response = requests.get('https://curl.se/',
                        headers=headers)
```

使用 cURL 命令，甚至使用 pycURL，都将极大地帮助我们学习如何使用与基于 Web 的通信相关的核心技术。随着与技术开发相关的流程正被路由到 AI 驱动和 ML 为基础的系统，理解这些核心概念将产生差异。

在下一节中，我们将学习可在网络请求中使用的附加功能（中间件功能）。

## 代理

代理或代理 Web 服务器（也称为中间件）是网络通信中，尤其是网络抓取中最重要和核心的技术之一。在抓取过程中，网络通信中会出现大量与网络安全相关的挑战，使用代理有助于我们克服它们。

在抓取时使用代理通常会隐藏原始机器的 IP。请注意，某些提供商并不总是会隐藏原始 IP。代理使用代理服务提供商可用的其他安全 IP 与目标站点通信。代理充当中间件，有助于绕过物理机器上的网络安全。在网络抓取中最常用的代理类型之一是轮换 **住宅代理**，它在每次新请求时更改 IP 地址。

互联网是开放内容。Web 服务器实施了大量安全措施以保持其内容安全。你们中的许多人会遇到一些被阻止查看或访问的内容，这可以通过使用代理来克服。在某种程度上，无论是在公共网络还是本地网络上，代理都提供强大的安全和过滤功能。

在处理长格式内容的网络抓取时，例如每日博客、评论、股票信息和评分，存在针对固定站点的各种重复抓取任务，以及包括按小时间隔的计划抓取选项。然而，需要考虑可能遇到网络安全措施而导致被阻止的可能性，例如以下情况：

- IP 黑名单
- 每个页面都面临 CAPTCHA
- 无法使用过滤选项
- 重定向到未针对的目标页面（如托管在与用户不同的国家或语言的页面）
- 仅重定向到第一页
- 重定向到主页

这些情况可能导致浪费时间、财务损失、磁盘和网络空间损失、收集无关数据、数据质量下降等等。为了解决这个问题，开发人员需要使用和实施基于 Web 的渗透测试，例如使用代理；为选定国家选择轮换 IP、代理与 **User-Agent** 的随机组合，或随机或标准的 *请求头*；或在终端中执行 `curl` 命令以验证内容。

以下是代理可能有帮助的一些情况：

- 克服地域（边境）限制
- 防止被列入黑名单
- 轮换 IP 地址
- 性能增强
- 绕过 CAPTCHA、恶意软件、垃圾邮件等
- 克服被阻止的访问
- 实现隐私功能

代理使用有众多优势，但也有两个显著的缺点，如下所示：

- 额外的财务需求（购买代理）
- 额外的配置（在现有网络和代码之上）

代理通常被发现并被规定用于抓取或爬取任务。

如以下示例代码模板所示，Python random 库的 shuffle 方法正被用于对 user_agents 和 proxyList 中的值进行随机化。以下代码在我们从网站获取有效 api-key 的付费订阅后才会运行，这将导致多个包含 http 和 https 端口的 IP 集合：

```
import requests
import random
user_agents = ['Mozilla/5.0 (iPhone; CPU iPhone OS 16.5 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) CriOS/114.0.5735.99 Mobile/15E148 Safari/604.1', 'Mozilla/5.0 (iPod; CPU iPhone OS 16_5 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) CriOS/114.0.5735.99 Mobile/15E148 Safari/604.1', 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36', 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36',]
# proxyList = dict(requests.get("https://api.demo-web.com?api-key=XXXX-XX-XXXX-XXX&zone=US&type=multiple&timeloc=12h&channel=chr_moz").content)
proxyList = {'http':['xxx.xxx.xxx.xxx:xx', 'xxx.xxx.xxx.xxx:xxx','xxx.xxx.xxx.xxx'],
'https':['xxx.xxx.xxx.xxx:xx','xxx.xxx.xxx.xxx:xxx', 'xxx.xxx.xxx.xxx']}
def rand_output(input):
    return random.shuffle(input)
response = requests.get('https://www....com',
headers={'User-Agent': rand_output(user_agents)},
proxies = rand_output(proxyList['https'])).text
```

proxyList 包含已访问服务提供商页面的 IP 列表。IP 也可以通过向代理提供商发出 API 请求来收集。rand_output 函数接收一个 Python list() 对象并返回 `shuffle` 列表。`requests` 支持 `headers` 和 `proxies` 参数等，可以在其中提供所需的值。

大多数基于云的平台和 Web 主机组织在需要时提供使用其内部代理服务的选项，甚至允许通过一些额外配置使用第三方代理设置。许多代理提供商使用 Web API 并允许用户使用某种身份验证系统（API 密钥）访问代理。用户可以在 Google 上搜索 `https://www.google.com/search?q=proxy+providers` 以找到最佳代理解决方案并使用或选择适当的解决方案。

> **重要说明**
>
> 有大量基于浏览器的扩展提供 **虚拟专用网络 (VPN)** 访问、轮换 IP 等。Python 库也存在于代理、用户代理、生成与安全相关的令牌等。 这些扩展和库可能会导致系统漏洞，或暂时或永久工作，但使用它们与否的决定是用户的责任，在决定之前应进行适当的调查。

在下一节中，我们将介绍一些在数据处理中有用的新工具和应用程序。

## 数据处理

在网络抓取的上下文中，数据处理是指存储、处理、管理和分析从抓取中生成的数据。在本书的前几章中，我们专注于有效和高效抓取的概念，并附带代码示例。

随着对数据需求的增长，技术也在不断发展并适应新的变化。目前，随着 AI/ML 为基础的系统激增，竞争在于提供易于使用且快速的解决方案，而不损害质量。

在接下来的章节中，我们将介绍一些有助于数据处理的技術。

## PySpark

用于 **Apache Spark** 的 Python 库，`pyspark` (`https://spark.apache.org/`)，用于处理和分析数据，尤其是大容量数据。**Spark** 是一个用于处理大数据（具有多样性、容量和速度的数据）的框架，比 **Hadoop** (`https://hadoop.apache.org/`) 更有效，Hadoop 是一个用于并行处理、调度和资源管理的框架。

Apache Spark 以其分布式处理系统、数据处理、环境的有效集群、CLI 应用程序和大容量数据的并行处理而闻名。它基于 Hadoop 的 **MapReduce**，并针对快速计算进行了额外优化，也可用于各种编程语言，如 C#、Java 和 Python。





pyspark 以其惰性计算和分析大数据而闻名，并受以下 Spark 功能支持：

- 内存处理
- 在 DataFrame 上使用结构化查询语言（SQL）
- 使用 pandas API
- 内置的 MLlib 机器学习库
- 计算速度
- 容错性

PySpark 还使用 Python 的 pandas API，因此大多数可以用 pandas 完成的操作也适用于 PySpark，同时还具备 Apache Spark 的额外优势和功能。通常，对于大型数据集，PySpark 非常有效。PySpark 的功能也可以独立使用，例如仅用于数据分析、使用机器学习、将数据集分块以及合并数据集的选定分块。

与 pandas 和其他 Python 库一样，PySpark 可用于可视化、数据清洗、数据格式化、数据采样等。Spark DataFrame 具有与 pandas 相似的属性，并且具有许多实现 Spark 框架各种功能的高级特性。请访问 https://spark.apache.org/docs/latest/quick-start.html 和 https://sparkbyexamples.com/ 了解更多详情。

在下一节中，我们将探讨一个因其处理速度而日益流行的 Python 库。

## polars

polars（https://www.pola.rs/）Python 库是一个数据分析库，被认为是速度最快的 DataFrame 库之一。polars 使用 Rust 编程语言构建，旨在实现更快、更高效的计算。在处理大型数据集时，polars 已被发现比 Python 的 pandas 更有效。

用 polars 编写的代码稍长，可读性也不如 pandas。下面列出了 polars 的一些优点，使其比其他数据分析库更具优势：

- 图形处理单元（GPU）集成
- 惰性求值（仅在需要时执行）
- 操作的并行处理
- 用 Rust 实现的联机分析处理（OLAP）查询引擎
- 由现代处理器实现的基于单指令多数据（SIMD）的向量化（用单条指令处理多个数据集）

polars 使用 Apache Arrow 或 Arrow（https://arrow.apache.org/）数组，而 pandas 使用 NumPy 数组。Arrow 为数据使用独立于语言的列式内存格式。

Arrow 推荐使用 Apache Parquet（https://parquet.apache.org/），这是一种实现数据压缩和编码方案的文件格式。Parquet 的使用正在迅速增长，因为它比 CSV 和 JSON 更快，并支持以列格式存储数据。Parquet 文件的主要优势之一是在数据加载方面：它们允许在不加载整个数据结构的情况下访问列。Arrow 也兼容于与 pandas、NumPy、PySpark 等一起使用或实现。

**重要说明**
PyArrow 是一个实现 Arrow 的 Python 库。Arrow 可以与内置的 Python 对象（如 NumPy 和 pandas）集成。Arrow 在同一个数组中管理数据和数据类型，数组中的数据被分组以创建表。PyArrow 将数据实现为表。每个数组代表表的列中的数据。PyArrow 还通过 ExtensionArray 接口集成到 pandas 中。它还支持将表读写为 DataFrame，反之亦然。

在下一节中，我们将讨论就业市场和职业，这些可以结合我们在本书各章中讨论和探索的背景和信息来追求。

## 就业与职业

到目前为止，在本书中，我们已经涵盖了有关 Python 编程和网络爬取的各个主题。我们学习了不同的技术，特别是侧重于数据的技术，包括搜索、获取、挖掘、转换、分析和可视化。

能够让人们运用所学技能的同时获取最新技能的工作机会，是继续学习和提升技能的强大动力。数据相关的职业在全球提供了诱人的前景，并提供优厚的薪资。

在本节中，我们将提供一份与网络爬取和 Python 编程相关的职位名称列表，供您参考。考虑到当前该领域的需求，这对于那些有兴趣发展数据相关技能（如数据科学和 AI/ML）的人尤其重要。

以下是一些来自全球各招聘网站的职位名称：

- 高级 Python 程序员
- 专家级 Python 程序员
- 网络爬虫工程师
- 数据提取工程师
- 数据研究员
- 历史数据研究员
- 地理数据研究员
- 数据科学家
- 数据科学从业者
- 数据分析师
- 数据安全
- 网络安全
- 数据可视化工程师
- 数据自由职业者
- 数据与大数据工程师
- 管理信息系统专家
- 报表开发工程师（数据驱动）
- 信息分析师
- 信息从业者
- 机器学习工程师
- 机器学习从业者
- 自然语言处理从业者
- 情感分析师

这些职位名称只是一些示例，您也可以考虑探索自由职业的机会。可以肯定的是，在与数据科学和管理相关的各个领域中，您都会遇到数据及其相关的处理活动。每当任务涉及分析、信息生成、信息处理和报告等活动时，我们在本书各章中学到的技能都将发挥作用。

## 总结

在本章中，我们探讨了市场上和数据科学领域的一些热门技术。技术框架和工具总是在不断演进。因此，开发人员有责任跟上技术的最新更新。

网络爬取或数据提取是数据科学的核心领域之一，尽管数据处理和分析任务紧随其后。使用爬取技术从目标网站收集、采集和存储数据曾经是网络爬取的核心方面。然而，由于与数据交互的系统取得了各种突破，提供可直接实现于系统中的高质量数据、以客户要求的格式存储的数据、适用于实时处理的数据等也需要被考虑。

数据或数据集通过 API、专门提供研究工具的网站等提供。当我们需要以所需格式从目标网站获取特定数据时，网络爬取就发挥作用了。以这种方式接收的数据可以被收集并与其他数据或数据集合并或追加，以创建数据集。

随着在线应用程序和框架的使用日益增长，无论是小规模数据集还是大规模数据集，或是爬取脚本实时收集的记录或行，它们在与数据相关系统交互时都需要具备可访问性和高性能，以验证使用 Python 编程进行网络爬取的需求。

我们已经到了本书的结尾。网络爬取是数据科学和数据驱动系统中的核心任务。它一直是动态的、进步的，并对数据、信息和知识的生成具有辅助作用。人工智能、机器学习、数据挖掘、自然语言处理和 Python 编程等主题总是以某种方式与网络爬取相关联。从职业和知识的角度来看，与网络爬取相关的主题值得探索。

## 延伸阅读

- 数据科学：
  - https://aws.amazon.com/what-is/data-science/
  - https://www.ibm.com/topics/data-science
- Apache Arrow：https://arrow.apache.org/docs/python/
- Apache Spark：https://spark.apache.org/docs/3.3.1/api/python/index.html
- 数据科学 – 职业：
  - https://hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century
  - https://www.coursera.org/articles/data-science-career
  - https://www.discoverdatascience.org/career-information/
  - https://www.montecarlodata.com/blog-the-future-of-big-data-analytics-and-data-science/
  - https://emeritus.org/in/learn/scope-of-data-analytics-in-the-future/
  - https://www.knowledgehut.com/blog/data-science/data-scientist-future




- *Python 并发：*
  - https://docs.python.org/3/library/concurrent.futures.html
  - https://vegibit.com/how-to-perform-parallel-programming-with-pythons-concurrent-futures-library/
- *AI 与数据：*
  - https://www.mygreatlearning.com/blog/difference-data-science-machine-learning-ai/
  - https://www.forbes.com/sites/nishatalagala/2022/11/10/ai-and-data-sciencewhat-is-the-difference/?sh=81569d64b442
  - https://hevodata.com/learn/artificial-intelligence-in-data-science/
- *大数据：* https://www.oracle.com/big-data/what-is-big-data/
