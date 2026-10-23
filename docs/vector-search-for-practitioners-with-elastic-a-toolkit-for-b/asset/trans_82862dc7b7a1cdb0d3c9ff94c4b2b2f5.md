

## 从 _source 中排除 dense_vector

Elasticsearch 在索引时将原始 JSON 文档存储在 _source 字段中。默认情况下，搜索结果中的每个匹配项都包含完整文档。然而，当文档包含高维 dense_vector 字段时，_source 字段可能会变得相当大，导致加载数据时产生大量开销。因此，kNN 搜索的速度可能会显著降低，因为处理这些更大的 _source 字段需要额外的时间。

为缓解此问题，您可以使用 excludes 映射参数禁用在 _source 中存储 dense_vector 字段。这种方法可防止在搜索操作期间加载和返回大型向量，从而减少处理开销和索引大小。尽管被从 _source 中省略，向量仍可用于 kNN 搜索，因为搜索过程依赖于单独的数据结构以高效执行搜索。

在应用 excludes 参数之前，了解从 _source 中省略字段的潜在缺点非常重要。这样做可能会限制检索文档或执行需要访问原始字段值的某些更新操作的能力。

映射中此设置的示例如下：

```
PUT my_index
{
  "mappings": {
    "_source": {
      "excludes": [
        "vector_field"
      ]
    },
    "properties": {
      "vector_field": {
        "type": "dense_vector",
        "dims": 384,
        "index" : True,
        "similarity" : "dot_product"
      },
      "text_field": {
        "type": "text"
      }
    }
  }
}
```

除非您计划在某时重新索引该字段，否则对于任何包含 `dense_vector` 字段的索引，用户都应考虑设置此项，因为它将节省空间并提高 kNN 搜索速度。

在 Elasticsearch 中从 `_source` 中排除 `dense_vector` 字段可以实现高效的 kNN 搜索速度并减少索引大小。然而，用户应权衡其优点与潜在的重新索引需求，确保为其特定用例提供优化且有效的存储方法。

## 摘要

在本章中，我们深入探讨了 Hugging Face 生态系统的复杂性以及 Elasticsearch 的 Eland Python 库的功能，并提供了在 Elasticsearch 中使用嵌入模型的实用示例。我们探索了 Hugging Face 平台，强调了其数据集、模型选择以及其 Spaces 的潜力。此外，我们提供了对 Eland 库的实践方法，说明了其功能并解决了映射、ML 节点和模型集成等关键考虑因素。我们还触及了集群容量规划的细微差别，强调了 RAM、磁盘大小和 CPU 的考虑。最后，我们强调了多种存储效率策略，重点关注降维、量化以及映射设置，以确保您的 Elasticsearch 集群获得最佳性能和资源节约。

在下一章中，我们将深入数据工作的运营阶段，并学习如何为推理和查询调优性能。

## 性能调优 – 处理数据

在上一章中，我们介绍了如何将 Elasticsearch 与 **机器学习 (ML)** 模型集成。在本章中，我们将重点关注优化 Elasticsearch 中的向量搜索性能。

我们将涵盖以下主题：

- ML 模型部署调优技术
- 估算 Elasticsearch 节点的向量容量
- 使用 Elastic 的性能工具 Rally 进行负载测试
- 排查缓慢的 **k-最近邻 (kNN)** 搜索响应时间

到本章结束时，您将了解如何估算用例所需的 Elasticsearch 节点数量，如何使用性能基准测试确保这些估算符合您的要求，以及识别缓慢 kNN 搜索响应时间的潜在原因。

让我们开始部署一个 **自然语言处理 (NLP)** 模型。

## 部署 NLP 模型

部署 NLP 模型需要选择一种部署方法，该方法能够在生产高峰期间处理预期的推理负载，同时高效且安全。在测试阶段，如果模型足够小，通常可以在本地运行。然而，生产用例需要生产部署策略。模型可以部署在虚拟机上，通过云提供商服务部署，在 Hugging Face 等专用模型托管服务上部署，或在可能的情况下部署在向量将被使用的同一服务中，例如 Elasticsearch。每种方法在效率、延迟、扩展和管理之间都有权衡。

与外部部署方法的交互通常涉及使用 API 发送文本输入并接收向量作为输出。无论向量如何生成，它们都会发送到数据存储以进行存储，并且以后可以使用精确匹配或近似最近邻搜索进行搜索。查询时生成的向量用于搜索之前存储的向量。

为获得最佳性能和效率而调优已部署模型可能涉及对硬件资源、软件配置和模型参数等因素进行调整。然而，具体的调优指南超出了本书的范围。

在 Elasticsearch 中部署嵌入模型时，模型在 Elasticsearch 集群内的专用 ML 节点上运行。这使得模型与 Elasticsearch 的搜索和索引功能无缝集成。我们接下来将介绍一些部署调优设置。

## 将模型加载到 Elasticsearch

Elasticsearch 版本 8.0+ 原生支持 libtorch，即 pytorch 中使用的 C++ 库。这使其能够在堆栈内运行基于 PyTorch 的模型并生成嵌入。

要将模型加载到 Elasticsearch，我们将使用上一章中讨论的 Eland Python 库。Eland 支持从本地驱动器加载模型或受支持的架构，但最简单的方法是从 Hugging Face 加载模型。文档涵盖了当前受支持的模型类型：https://www.elastic.co/guide/en/machine-learning/current/ml-nlp-model-ref.html。

在以下示例中，我们将直接从 Hugging Face 使用 Eland 加载 sentence-transformers/msmarco-MiniLM-L-12-v3 (https://huggingface.co/sentence-transformers/msmarco-MiniLM-L-12-v3) 模型。Eland 将下载模型并将其分块以加载到 Elasticsearch。

在上一章中，我们介绍了如何使用 Python 将 NLP 模型加载到 Jupyter Notebook 中。作为提醒，您可以在本书 GitHub 仓库的 chapter 3 文件夹中查看使用 Eland 的 Python 代码：https://github.com/PacktPublishing/Vector-Search-for-Practitioners-with-Elastic/tree/main/chapter3。

或者，如果您只需要使用它来加载模型，也可以将 Eland 与 Docker 一起使用。Docker 是一个开源平台，可在轻量级且可移植的容器内自动化应用程序的部署。它确保软件在从一个计算环境移动到另一个环境时可靠运行。它通过使用容器来简化开发流程，容器允许开发人员将应用程序及其所有依赖项打包到软件开发的标准单元中。有关安装和使用 Docker 的更多信息，请访问 www.docker.com。

Eland Docker 容器（一个标准化的、封装的环境，可在底层系统和其他容器之外隔离运行应用程序及其依赖项）可以从 Eland GitHub 仓库下载：https://github.com/elastic/eland。

要使用 Docker 从 Hugging Face 安装模型，您可以运行以下命令：

```
$ docker run -it --rm --network host \
  elastic/eland \
  eland_import_hub_model \
  --url http://host.docker.internal:9200/ \
  --hub-model-id sentence-transformers/msmarco-MiniLM-L-12-v3\
  --task-type ner
```

模型加载后，让我们看看如何部署它，以及我们有哪些配置选项。

## 模型部署配置

一旦模型加载到 Elasticsearch 中，它必须被部署或启动。模型在 ml 节点类型的节点上运行，就像在 Elastic Cloud 中部署时一样。

NLP 模型，例如嵌入模型，可以通过配置分配数和每个分配的线程数来扩展以处理所需生产推理负载：

- **分配数**: 在启动模型时增加分配数设置会增加模型在所有可用 ML 节点上的分配量。增加分配数通常会提高整体吞吐量。此设置即使在模型已启动后也可以增加或减少。
- **每个分配的线程数**: 增加每个分配的线程数会影响每个模型实例使用的 CPU 线程数。增加此数字通常会改善每个请求的推理时间。此值只能在模型部署时更改。

这两个设置的组合不得超过所有可用 ML 节点上可用的线程总数。增加这些数字时会出现收益递减点。您应使用特定工作负载进行测试以找到最佳设置：

