

前面的代码展示了一个由 Embedchain 提取的文档块以及使用 OpenAI 的 embeddings 数组。因此，最终在我们的文档用例中，我们希望数据源的结构与在 Elastic 中的结构保持一致。如果用户输入“告诉我 Elastic 的 Customer Architect 职位的职责是什么”，该问题会通过插件从 ChatGPT 传递给 Embedchain。插件首先查询 Elasticsearch 以从已抓取的内容中获取最合适的 URL，然后 Embedchain 会动态地将该 URL 添加到上下文中以使用 GPT 生成响应。

在以 Elasticsearch 和嵌入技术奠定基础之后，我们现在将焦点转向对话的核心：ChatGPT。我们如何确保我们的强大搜索引擎能够与这位对话者无缝通信？让我们深入探讨集成过程。

## 集成 ChatGPT——创建实时对话者

在本节中，我们将探讨将 ChatGPT 与我们丰富的 Elasticsearch 数据集融合的机制。通过桥接这些技术，我们旨在创建一个不仅具有反应性，而且还能深刻地以我们构建的动态上下文为依据的机器人。我们将逐步介绍集成步骤，确保 ChatGPT 能够流畅地访问并解释存储在 Elasticsearch 中的大量信息。

## 处理 Elasticsearch 与 ChatGPT 之间的实时数据流

让我们先退一步，提醒自己期望的组件和流程：

- 1. 用户仅与 ChatGPT 交互。
- 2. 用户已激活插件。
- 3. 当用户就 Elastic 文档中的最新信息提问时，用户提示会传递给插件。
- 4. 插件分析提示，然后查询包含已抓取文档的索引。
- 5. Elasticsearch 返回一个或多个 URL。
- 6. 这些 URL 由 Embedchain 添加到上下文中。
- 7. Embedchain 生成响应。
- 8. 插件将 Embedchain 返回的响应打印到 ChatGPT 中供用户查看。

到目前为止，我们已经有了包含文档的索引——现在，我们必须构建 ChatGPT 插件。请参考 OpenAI 的文档了解插件框架的基础知识（https://platform.openai.com/docs/plugins/introduction）。我们将假设你已阅读文档并理解创建插件所需的基础知识。

## 插件代码

插件必须符合 OpenAI 文档的要求才能在 ChatGPT 中通过验证并安装。我们将首先查看存储库结构，然后继续创建一些必要的文件以及实现代码以实现我们的目标。

## 存储库结构

存储库结构很简单，如下图所示：

![](img/21c3d4250a746422a7489bc2647611d8_217_0.png)

图 10.13：ChatGPT 插件存储库结构

让我们分解结构：

- - 主要服务代码在 app.py 中
- - 插件的 logo（logo.png）将在 ChatGPT 插件商店中显示
- - 插件清单存储在 .well-known 目录下的 ai-plugin.json 文件中
- - OpenAPI 描述符包含在 openapi.yaml 文件中

请注意，我们在此使用 Dockerfile 作为部署插件的资源。插件部署平台可以根据你可用的资源而有所不同；我使用的是 Google Cloud Compute。你需要选择你喜欢的部署平台或云提供商。插件代码部署超出了本书的范围。现在，让我们看看公开我们 API 的代码。

## 服务代码

服务代码由 ChatGPT 为验证插件和运行代码而期望的四个 REST API 端点组成：

```
...
@app.get("/search")
...

@app.get("/logo.png")
```

```
async def plugin_logo():
    filename = 'logo.png'
    return await quart.send_file(filename, mimetype='image/png')

@app.get("./.well-known/ai-plugin.json")
async def plugin_manifest():
    host = request.headers['Host']
    with open("./.well-known/ai-plugin.json") as f:
        text = f.read()
    text = text.replace("PLUGIN_HOSTNAME", f"https://{host}")
    return quart.Response(text, mimetype="text/json")

@app.get("/openapi.yaml")
async def openapi_spec():
    host = request.headers['Host']
    with open("openapi.yaml") as f:
        text = f.read()
    text = text.replace("PLUGIN_HOSTNAME", f"https://{host}")
    return quart.Response(text, mimetype="text/yaml")
...
```

第一个 /search API 是实现的核心。其余部分按出现顺序返回 logo、清单和 OpenAPI 定义。

/search API 是魔法的发生地。以下是实现的分解：

```
async def search():
    query = request.args.get("query")
    url = ESSearch(query)
    return quart.Response(url)
```

ESSearch 函数是最重要的一部分，因为它在 Elasticsearch 中搜索最相关的文档并将它们的链接传递给 Embedchain 以构建上下文。请注意，我们在此使用简单的 BM25 Elasticsearch 查询，因为我们使用 Embedchain 为我们抽象上下文构建过程。这意味着 Embedchain 会根据我们提供的链接生成嵌入并返回生成的答案：

```
def ESSearch(query_text):
    cloud_url = os.environ['cloud_url']
    cid = os.environ['cloud_id']
    cp = os.environ['cloud_pass']
    cu = os.environ['cloud_user']
    es = es_connect(cid, cu, cp)
    # Elasticsearch query (BM25)
    query = {
```

```
"bool": {
    "filter": [
        {
            "prefix": {
                "url": "https://www.elastic.co/guide"
            }
        }
    ],
    "must": [{
        "match": {
            "title": {
                "query": query_text,
                "boost": 1
            }
        }
    }]
}

fields = ["title", "body_content", "url"]
index = 'search-elastic-doc'
resp = es.search(index=index,
                query=query,
                fields=fields,
                size=10,
                source=False)

body = resp['hits']['hits'][0]['fields']['body_content'][0]
url = resp['hits']['hits'][0]['fields']['url'][0]
elastic_bot = App()
# Assuming 'resp' is the response object you've mentioned
for hit in resp['hits']['hits']:
    for url in hit['fields']['url']:
        print(url)
        #es_app.add(url)
        elastic_bot.add(url)

return elastic_bot.query("What can you tell me about " + query_text)
```

请注意，我们通过在用户输入前加上“what can you tell me about”来保持提示的宽泛性。

测试代码应会在 URL http://0.0.0:5001 上公开一个端点。打开你选择的测试工具并传递类似以下的查询：

```
http://0.0.0:5001/search?query=detections
```

此查询要求机器人构建关于 Elastic Security detections 的上下文。如果你在控制台中打印 URL，你会看到 Embedchain 正在向上下文中添加内容：

```
https://www.elastic.co/guide/en/siem/guide/current/detection-engine-overview.html
Successfully saved https://www.elastic.co/guide/en/siem/guide/current/detection-engine-overview.html (DataType.WEB_PAGE). New chunks count: 15
https://www.elastic.co/guide/en/siem/guide/current/rule-api-overview.html
Successfully saved https://www.elastic.co/guide/en/siem/guide/current/rule-api-overview.html (DataType.WEB_PAGE). New chunks count: 5
https://www.elastic.co/guide/en/security/current/detection-engine-overview.html
Successfully saved https://www.elastic.co/guide/en/security/current/detection-engine-overview.html (DataType.WEB_PAGE). New chunks count: 17
https://www.elastic.co/guide/en/security/8.9/detection-engine-overview.html
Successfully saved https://www.elastic.co/guide/en/security/8.9/detection-engine-overview.html (DataType.WEB_PAGE). New chunks count: 17
https://www.elastic.co/guide/en/security/current/rule-api-overview.html
Successfully saved https://www.elastic.co/guide/en/security/current/rule-api-overview.html (DataType.WEB_PAGE). New chunks count: 5
https://www.elastic.co/guide/en/security/current/detections-permissions-section.html
Successfully saved https://www.elastic.co/guide/en/security/current/detections-permissions-section.html (DataType.WEB_PAGE). New chunks count: 18
```

再次说明，为本示例的目的，我们保持 Elasticsearch 查询相当宽泛，以避免过度缩小结果范围，但 SoC 在这里确实会受到影响，因为准确性可能会下降。因此，根据你的需求，你可能需要微调查询以产生更少或更多结果。

REST 调用应返回有关 Elastic detections 的内容：

```
Detections are alerts that are created based on rules in the Elastic Security Solution. These alerts are generated automatically when events and external alerts are sent to Elastic Security. They are displayed on the Detections page, and provide information about potential security threats or suspicious activities. For more detailed information about the differences between events, external alerts, and detection alerts, you can refer to the Elastic Glossary.
```

