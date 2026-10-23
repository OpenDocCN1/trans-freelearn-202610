

鉴于 kNN 搜索对 page-cache 内存需求尤为重要，用户需要留意有多少个向量能放入节点的可用 page-cache RAM 中。假设使用分层可导航小世界 Hierarchical Navigable Small World (HNSW) 的默认配置，所需字节数的粗略计算如下：

```
num_vectors * 4 * (num_dimensions + 12) = total RAM in bytes
```

举例来说，如果你使用的是输出 512 维向量的模型，并且预计要将 2000 万个向量加载到 Elasticsearch 中，公式如下：

```
20,000,000 * 4 * (512 + 12) = 41,920,000,000 bytes
41,920,000,000 / 1024 ~= 39 GB RAM
```

2000 万个 embedding 字段大致估算需要约 39 GB 的 off-heap RAM 才能使 kNN 搜索性能最佳。如果将 Elasticsearch 中的一个完整节点视为 64 GB——其中一半分配给 Elasticsearch JVM，几 GB 分配给操作系统和其他进程——那么初步估算需要 2 个数据节点来处理这些向量。此外，请注意这尚未考虑副本。

这只是一个初步估算，是开始测试时一个不错的起点。它并非精确的 RAM 需求。后续章节中将介绍的测试将在投入生产前为你提供最佳数值。

在明确了 RAM 使用的要求和影响后，我们将注意力转向生态系统中的另一个关键资源：存储。

## 磁盘需求

正如我们所见，kNN 搜索性能的首要考虑因素之一是确保有足够的 off-heap RAM 可用于存储向量。然而，需要注意的是，这些向量仍然是在磁盘上生成和存储的。因此，用户必须确保有足够的存储空间可用。不过，由于磁盘大小相对于 RAM 大小，磁盘容量在规划集群容量时通常不是限制因素。

上一节中提到的估算所需 RAM 的公式与向量数据的磁盘容量计算相同，但你还需要考虑与向量一起存储的任何其他字段，例如向量的文本、其他文本字段和元数据。

磁盘容量的粗略初始估算如下：

```
size of a single document * number of documents * number of replicas * 1.2
```

这里，1.2 是 JSON 扩展与磁盘压缩之间的净乘数。请注意，这是一个非常粗略的估算。

然而，了解你的数据在 Elasticsearch 中的表现的唯一真实方法就是加载一个样本数据集。你可以轻松使用 Elastic Cloud 的免费试用版来启动一个小型集群并加载一个小型的代表性数据集。开始试用非常快速，只需在 https://cloud.elastic.co/registration 提供一个电子邮件地址即可。

你将获得一个小型、功能齐全的 Elasticsearch 集群，包括一个 ML 节点和所有企业功能。https://www.elastic.co/getting-started 上的入门指南可以帮助你快速上手。

加载数据后，你将能够看到数据实际扩展后的确切大小，这是规划生产需求的唯一真实方法。即使在免费试用期结束后，你也可以根据需要逐步扩展集群，采用灵活的按月付费方式。

你可以使用下一节介绍的磁盘使用 API 来查看磁盘使用情况，我们将在下一章介绍集群调优、负载测试和完整扩容测试。

既然我们已经讨论了了解数据大小和扩展影响的重要性，那么拥有能够提供这些资源实际使用情况精确洞察的工具就至关重要。Elasticsearch 套件中这样一个关键工具就是 analyze disk usage API。让我们探讨其功能以及它如何帮助了解磁盘空间消耗。

## Analyze Index Disk Usage API

一旦向量和其他数据被加载到 Elasticsearch 中，你可以使用 **analyze index disk usage API** 来计算占用的磁盘空间。对于 analyze disk usage API 的更多信息，请访问文档页面 https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-disk-usage.html。

search-elastic-docs 索引包含多个数据量相对较小的向量字段，其中之一是 title-vector：

```
POST /search-elastic-docs/_disk_usage?run_expensive_tasks=true&pretty
```

这会给出以下响应（API 调用的完整响应非常长，这里仅显示包含相关部分的截断输出）：

```
{
  "_shards": {
    "total": 2,
    "successful": 2,
    "failed": 0
  },
  "search-elastic-docs": {
    "store_size": "989.5mb",
    "store_size_in_bytes": 1037592858,

```

```
"all_fields": {
    "total": "987.6mb",
    "total_in_bytes": 1035596641,
    "inverted_index": {
        "total": "69.8mb",
        "total_in_bytes": 73269048
    },
...
    "knn_vectors": "83.4mb",
    "knn_vectors_in_bytes": 87462952
},
"fields": {
...
    "title-vector": {
        "total": "40mb",
        "total_in_bytes": 42046916,
        "inverted_index": {
            "total": "0b",
            "total_in_bytes": 0
        },
...
        "knn_vectors": "40mb",
        "knn_vectors_in_bytes": 42046916
    },
...
```

我们可以看到向量占用了 83.4 MB。分析完磁盘使用情况后，我们可以将注意力转向 ML 节点容量规划。

## ML 节点容量

对于在 Elasticsearch 的 ML 节点上运行的 embedding 模型，你需要规划以确保节点在推理时有足够的容量来运行模型。Elastic Cloud 允许根据 CPU 需求自动扩展 ML 节点，这使得它们在计算需求增加时能够扩展，并在需求降低时缩减。

我们将在下一章更详细地介绍为推理调优 ML 节点，但至少，你需要一个具有足够 RAM 来加载至少一个 embedding 模型实例的 ML 节点。随着性能需求的增加，你也可以增加单个模型的分配数量以及每个分配分配的线程数。

要检查模型的大小以及加载模型所需的内存量，你可以运行 get trained models statistics API（有关此 API 的更多信息，请访问文档页面 https://www.elastic.co/guide/en/elasticsearch/reference/current/get-trained-models-stats.html#get-trained-models-stats）：

```
GET _ml/trained_models/dslim__bert-base-ner-uncased/_stats?human
```

这将返回各种统计信息，包括模型大小和内存需求：

```
{
  "model_id": "dslim__bert-base-ner-uncased",
  "model_size_stats": {
    "model_size": "415.4mb",
    "model_size_bytes": 435663717,
    "required_native_memory": "1gb",
    "required_native_memory_bytes": 1122985674
  },
}
```

当模型运行时，你也可以在 Kibana UI 中通过进入 Machine Learning > Model Management > Trained Models 并展开模型 ID 来获取其统计信息。

在规划 ML 节点容量时，需要考虑两种不同的情况：

- 索引时：通常，会有大量文档的初始批量加载到 Elasticsearch 索引中，这需要生成向量。这会对 ML 节点产生很大的需求，要求它们尽可能快地运行推理。这就是能够轻松扩展 ML 节点并动态增加模型分配数量的能力非常有用的地方。你可以在初始批量加载运行时扩展模型分配，然后在加载完成后减少分配。

- 搜索时推理：当用户发送搜索查询时，你需要使用与生成文档向量相同的 embedding 模型来生成查询向量。跟上平均查询负载将有助于你规划正常运行规模。如果预计查询负载会出现计划性峰值，例如电子商务中的繁忙购物周末，你可以主动增加模型分配数量，从而扩展 ML 节点。

测试这两种条件的唯一真实方法是对实际数据进行负载测试和性能调优，我们将在下一章更深入地介绍。

