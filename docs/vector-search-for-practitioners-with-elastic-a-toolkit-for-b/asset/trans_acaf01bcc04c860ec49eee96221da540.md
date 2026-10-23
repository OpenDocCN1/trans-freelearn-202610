

接下来，我们设置并确认模型 ID。为了使名称与 Elasticsearch 兼容，/ 被替换为 __（双下划线）：

```
es_model_id = tm.elasticsearch_model_id()
```

以 TorchScript 形式导出模型，这是 Elasticsearch 使用的格式：

```
tmp_path = "models"
Path(tmp_path).mkdir(parents=True, exist_ok=True)
model_path, config, vocab_path = tm.save(tmp_path)
```

## 将模型加载到 Elasticsearch

在此，模型将被序列化并加载到 Elasticsearch，以便随后在 ML 节点上启动：

```
ptm = PyTorchModel(es, es_model_id)
ptm.import_model(model_path=model_path, config_path=None, vocab_path=vocab_path, config=config)
```

## 启动模型

此步骤将输出有关模型的信息，例如名称和状态：

```
# List the in Elasticsearch
m = MLClient.get_trained_models(es, model_id=es_model_id)
```

## 部署模型

以下代码将在 ML 节点上加载模型并启动使其可用于 NLP 任务的过程：

```
s = MLClient.start_trained_model_deployment(es, model_id=es_model_id)
```

接下来，我们将验证模型是否已无问题启动：

```
stats = MLClient.get_trained_models_stats(es, model_id=es_model_id)
stats.body['trained_model_stats'][0]['deployment_stats']['nodes'][0]['routing_state']
```

你应该会看到以下输出，表明模型已部署到 ML 节点：

```
{'routing_state': 'started'}
```

## 为查询生成向量

在运行 kNN 查询之前，我们需要将查询字符串转换为向量。

接下来，我们将创建一个示例查询句子以验证一切是否已正确设置：

```
docs = [
    {
        "text_field": "Last week I upgraded my iOS version and ever since then my phone has been overheating whenever I use your app."
    }
]
```

我们调用 _infer 端点，它提供 model_id 和我们想要向量化的文档：

```
z = MLClient.infer_trained_model(es, model_id=es_model_id, docs=docs, )
```

第一个文档的向量可以在响应字典中访问，如下所示：

```
doc_0_vector = z['inference_results'][0]['predicted_value']
doc_0_vector
```

在加载并成功测试嵌入模型后，我们现在可以在生产环境中使用它。

## 在 Elasticsearch 中生成向量

向量可以在摄取期间在文档被索引（写入）到 Elasticsearch 之前使用摄取管道生成。摄取管道中的每个处理器执行不同的任务，例如丰富、修改或删除数据。该管道中的关键处理器是推理处理器，它将文本传递给嵌入模型，接收向量表示，然后在将文档传递以存储到索引之前将向量添加到原始文档负载中。这确保了文档的向量表示在摄取后立即可用于索引和搜索。

接下来是一个 Elasticsearch 摄取管道配置示例，它使用推理处理器与我们之前加载的 sentence-transformers/msmarco-MiniLM-L-12-v3 模型。管道从输入文档中获取名为 summary 的字段，使用嵌入模型对其进行处理以生成嵌入，并将生成的向量存储在名为 vector 的字段中。

这里，我们使用 HTTP 调用而不是 Python 代码来演示另一种方法。请注意，假设模型已在集群中加载，因为我们在之前的示例中已完成此操作：

```
PUT _ingest/pipeline/embedding_book_pipeline
{
  "description": "Ingest pipeline using bert-base-uncased model",
  "processors": [
    {
        "inference": {
            "target_field": "vector",
            "model_id": "sentence-transformers__msmarco-minilm-l-12-v3",
            "field_map": {
                "summary": "text_field"
            }
        }
    }
]
```

要在索引文档时使用此摄取管道，请在索引请求中包含 pipeline 参数：

```
PUT my_index/_doc/1?pipeline=embedding_book_pipeline
{
    "summary": "This is an example text for the embedding model."
}
```

此请求将索引一个文档，其中包含提供的文本字段和一个额外的向量字段，其中包含由 bert-base-uncased 模型生成的嵌入。

也可以直接调用 _infer 端点以在查询时（请参阅下一节）或出于任何其他临时原因生成向量：

```
POST _ml/trained_models/sentence-transformers__msmarco-minilm-l-12-v3/_infer
{
    "docs": [
    {"text_field": "How do I load an NLP model from Hugging Face"}
    ]
}
```

这将返回 200 响应代码和提交文本的向量表示：

```
{
    "inference_results": [
        {
            "predicted_value": [
                0.012806695885956287,
                -0.045506563037633896,
                0.014663844363951864,
                ...
                0.002125605707988143,
                0.06371472030878067
    ]
    }
]
}
```

在搜索时，可以调用 _infer 端点从输入文本生成向量，然后将其用作向量搜索查询的一部分。在这里，我们将使用之前生成的向量作为 kNN 查询的一部分：

```
GET search-elastic-docs-vectorized/_search
{
  "knn": {
    "field": "body_content-vector",
    "k": 1,
    "num_candidates": 20,
    "query_vector": [
      0.012806695885956287,
      -0.045506563037633896,
      0.014663844369351864,
      ...
      0.002125605707988143,
      0.06371472030878067
    ]
  },
  "fields": [ "body_content", "headings" ],
  "_source": false
}
```

或者，可以在 _knn 查询中指定嵌入模型（Elasticsearch 8.7 之后），Elasticsearch 将在运行其余 _knn 查询之前处理生成查询向量。这允许更简化的向量搜索功能集成并简化 kNN 搜索过程：

```
GET search-elastic-docs-vectorized/_search
{
  "knn": {
    "field": "dense-vector-field",
    "k": 10,
    "num_candidates": 100,
    "query_vector_builder": {
      "text_embedding": {
        "model_id": "my-text-embedding-model",
        "model_text": "The opposite of blue"
}
}
}
}
```

让我们看看如何在生产环境中规划集群容量和资源。

## 规划集群容量和资源

规划足够的集群容量和资源对于任何生产环境都至关重要，尤其是在实施相当规模的向量搜索用例时。为了确保最佳性能和效率，必须进行仔细的考虑、规划和测试。

在下一章中，我们将深入探讨负载测试，这是微调和优化 Elasticsearch 部署的重要部分。但在此之前，我们将探讨在 Elasticsearch 的 ML 节点上运行嵌入模型所需的条件，概述需要考虑的关键因素，以便在性能和资源利用之间取得正确的平衡。在本节中，我们将讨论 CPU、RAM 和磁盘需求的关键方面，为全面了解 Elasticsearch 中的资源管理奠定基础。

## CPU 和内存需求

Elasticsearch 中向量搜索的 CPU 需求与传统搜索没有太大区别。主要区别在于向量数据在 **近似 kNN**（为简单起见，我们在此称之为 **kNN**）查询期间如何被访问。

在传统搜索和暴力/精确匹配向量搜索中，随着更多文档的添加，磁盘空间会扩展，CPU 会扩展以适应增加的吞吐量。

在 Elasticsearch 中的 kNN 搜索中也是如此；然而，为了获得最快的响应时间，我们希望有足够的非堆 RAM 可用以将所有向量嵌入加载到内存中。当 Elasticsearch JVM 之外的 RAM 不足以容纳嵌入时，Elasticsearch 将不得不从磁盘读取，从而导致响应时间变慢，尤其是在频繁发生时。这种数据访问方式的转变可以提高性能，因为观察到当向量必须从磁盘而不是直接从内存读取时，更有可能出现热线程。通过直接从非堆 RAM 访问数据，向量搜索减少了这些热线程的影响并提供更高效的查询过程。

> **注意**
> `hot_threads` 是一个诊断 API，提供有关 Elasticsearch 节点中当前运行的最繁忙内部任务的见解。它通过突出显示消耗异常高 CPU 资源线程来帮助识别性能瓶颈。

