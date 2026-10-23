

针对我们的特定需求，从 https://www.elastic.co/guide 获取 Elastic 文档用例，Elastic 网页爬虫至关重要。它系统地扫描指南，确保全面捕获最新的文档。此数据一旦编入 Elasticsearch 索引，即可为语义嵌入和后续与 ChatGPT 的交互做好准备。

借助 Elastic 网页爬虫等工具，我们可以少担心如何收集数据，而更专注于如何有效利用这些数据来提升聊天机器人的性能。

让我们首先确保您在 Elastic Cloud 部署中运行着一个 Enterprise Search 节点：

+   1. 打开您的部署并检查是否看到如下截图中所示的节点：

![](img/21c3d4250a746422a7489bc2647611d8_210_0.png)

图 10.3：Elastic Cloud 控制台中的 Enterprise Search 节点

+   2. 如果没有，则需要编辑您的部署以添加一个。点击左侧的 Edit 按钮：

![](img/21c3d4250a746422a7489bc2647611d8_210_1.png)

图 10.4：Elastic Cloud 控制台中的侧边菜单

3. 向下滚动并使用以下设置添加一个 Enterprise Search 实例：

## Enterprise Search

Edit user settings

## Enterprise Search instances info

Add modern search to your application or connect and unify content across your workplace.

Size per zone
2 GB RAM | Up to 4.2 vCPU

Availability zones
1 zone 2 zones 3 zones

Total (size x zone)
2 GB RAM | Up to 4.2 vCPU

图 10.5：Enterprise Search 实例配置

4. 更新您的部署。完成后，导航到您的 Kibana 实例，然后进入 Content/Indices 下的 Search 部分。从那里，您应该会看到一个 Create an Index 按钮，如以下截图所示：

## Elasticsearch indices

Create a new index

## Welcome to Enterprise Search

## Add data and start searching

Your content is stored in an Elasticsearch index. Get started by creating an Elasticsearch index and selecting an ingestion method. Options include the Elastic web crawler, third party data integrations, or using Elasticsearch API endpoints.

Whether you're building a search experience with App Search or Elasticsearch, you can now get started here.

Create an Elasticsearch index

Learn more

图 10.6：Enterprise Search 添加数据助手

5. 点击此按钮并从出现的选项中选择 Use a web crawler（已知问题链接：https://www.elastic.co/guide/en/enterprise-search/current/crawler-known-issues.html）：

## Select an ingestion method

Create a search optimized Elasticsearch index to store your content. Start by selecting an ingestion method.

图 10.7：摄取方法选择

6. 为您的索引命名——这里，我使用了名称 packt-cdl-source：

## Web crawler search index

Use the web crawler to programmatically discover, extract, and index searchable content from websites and knowledge bases.

## Create an Elasticsearch index

This index will hold your data source content, and is optimized with default field mappings for relevant search experiences. Give your index a unique name and optionally set a default language analyzer for the index.

图 10.8：网页爬虫搜索索引创建

7. 在 Domain URL 字段中，添加 https://www.elastic.co，因为这是我们想要爬取的：

## search-packt-cdl-source

## Add a domain to your index

Configure the domains you'd like to crawl, and when ready trigger your first crawl.

Domain URL

https://www.elastic.co

Domain URLs require a protocol and cannot contain any paths.

Web Crawler entry point has been set as /guide

Initial validation

Network connectivity

Indexing restrictions

Found a robots.txt file at https://www.elastic.co/robots.txt and it allows us access to the domain with some restrictions that may affect content indexing.

Content verification

图 10.9：爬虫域名验证

8. 由于我们只想爬取文档，请添加 /guide 入口点：

## Manage https://www.elastic.co

## Entry points

Include the most important URLs for your website here. Entry point URLs will be the first pages to be indexed and processed for links to other pages.

Learn more about entry points.

URL

https://www.elastic.co/guide

图 10.10：爬虫入口点

然后，通过点击右上角的 Crawl 按钮开始爬取所有域名：

![](img/21c3d4250a746422a7489bc2647611d8_214_0.png)

图 10.11：爬虫操作

9. 您可以通过在 Dev Tools 中运行 GET search_packt_cdl_source/_search 查询来验证文档是否正在进入。

好消息是我们不必对这些文档进行任何转换或数据处理即可与 Embedchain 一起使用。相反，我们将专注于这些文档的 URL 字段，因为这是构建我们的可插拔 DCL 的关键。

## Elevating data with Embedchain

在本节中，我们深入探讨 Embedchain（https://github.com/embedchain/embedchain）的机制，Embedchain 是一个开源数据平台，旨在利用 LLM 功能增强数据集。我们将探讨它如何无缝集成 Elasticsearch，使 ChatGPT 能够基于数据集增强的语义上下文进行理解和响应。本节提供了利用 Embedchain 在原始数据和有意义交互之间架起桥梁的如何以及为何。

我们喜欢将 Embedchain 视为 LangChain、向量数据库和 LLM 之上的另一个抽象层。它提供更高级别的控制，并专门用于从不同类型的数据源向 LLM 加载上下文。我能想到的最接近的例子是在电影《黑客帝国》中，Trinity 请求一个直升机训练程序，Tank 将其加载到她的脑海中。Embedchain 基本上以同样的简易程度工作。以下是一个示例：

```
elastic_bot.add(" https://jobs.elastic.co/jobs/csm/san-francisco-ca/customer-architect/5112073?gh_jid=5112073#/" )
```

这将加载包含职位描述的页面到名为 elastic_bot 的 Embedchain 应用中，这样当用户提交有关此职位的提问时，elastic_bot 应用将拥有上下文：

```
elastic_bot.query("Tell me about  the Customer Architect role at Elastic")
```

这是此示例的 Embedchain 机器人输出：

```
elastic_bot.query("Tell me about the Customer Architect role at Elastic")
```

'The Customer Architect role at Elastic is a position within the Customer Success Group. Customer Architects at Elastic are responsible for working with customers to understand their needs and requirements, and then designing and implementing solutions using Elastic's technology stack. They help customers optimize their use of Elastic's products for search, observability, and security purposes. Customer Architects also provide technical guidance and support to customers throughout the implementation process.'

图 10.12：Embedchain 输出

就这么简单！那么，让我们将其置于我们的目标背景下。我们希望用户通过 ChatGPT 与我们的插件交互，该插件会响应提示，通过 Embedchain 加载正确的文档页面。当 Embedchain 应用添加内容时，它实际上将其存储在底层向量数据库中。它使用 OpenAI 的 Ada 嵌入模型从其抓取的数据块创建嵌入。所有的数据分块和水合处理均由 Embedchain 完成。

在此示例中，我们将使用 Elasticsearch 作为向量数据库。以下代码块显示了当添加链接时 Embedchain 索引的文档结构：

```
{
    "_index": "embedchain_store_1536",
    "_id": "635ef64b5a99725d3b2cdf1cca86d5570a70388aa4d8d328cb2a02dd74b232c8",
    "_score": 1,
    "_source": {
        "text": "Aligning with customer business goals, you'll also guide them through onboarding, ensure they adopt our solutions, and aid in their growth. As a Customer Architect, you'll stand as the Go-to Technical Counsel, collaborating closely with the customer and partnering with teams like Field and Services. Your role ensures that Elastic's offerings consistently surpass customer hopes. Leveraging your technical knowledge, business acumen, and commitment to value, you'll enhance and elevate the customer journey.",
        "metadata": {
            "url": "https://jobs.elastic.co/jobs/csm/united-states/customer-architect/5112021?gh_jid=5112021#",
            "data_type": "web_page",
            "hash": "832c08a036185237195ac37a6133db16"
        },
        "embeddings": [...] 
    }
}
```

