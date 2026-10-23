

让我们来看一下可以让你设置一个简单的 Gradio 界面的代码：

```
import gradio as gr
from transformers import pipeline
sentiment_pipeline = pipeline("sentiment-analysis")

def sentiment_analysis(text):
    result = sentiment_pipeline(text)
    return result[0]["label"]

iface = gr.Interface(fn=sentiment_analysis, inputs="text",
outputs="text")
iface.launch()
```

在这个示例中，使用 Hugging Face Transformers 库创建了一个情感分析 pipeline，并定义了一个 Gradio 接口来接受文本输入并将情感标签作为输出显示。一旦代码上传到 Hugging Face Space，用户就可以通过网络浏览器与应用程序交互，使用自己的文本输入测试情感分析模型。Spaces 提供了一种简单而强大的方式来展示 ML 模型并促进社区内的协作。

![](img/21c3d4250a746422a7489bc2647611d8_75_0.png)

图 3.5：测试情感分析模型的一个简单示例

虽然 Hugging Face Spaces 为 NLP 应用提供了卓越的平台，但 Hugging Face 并不是我们唯一可用的资源。让我们通过将重点转移到 Eland 及其在 Elasticsearch 与传统数据处理框架（如 pandas）之间架起桥梁的作用上来丰富我们的工具箱。

## Eland

Eland 是 Elastic 开发的 Python 库，允许用户无缝地与 Elasticsearch 接口进行数据操作和分析。该库构建在官方 Elasticsearch Python 客户端之上，并扩展了 pandas API 到 Elasticsearch。这使得用户能够使用熟悉的 pandas 语法和约定与 Elasticsearch 数据交互，从而更轻松地将 Elasticsearch 与现有的数据分析工作流程和工具集成。

Eland 对于处理无法放入内存且需要分布式处理的大型数据集特别有用。Elasticsearch 可以通过将数据分布到集群中的多个节点来水平扩展。这使用户能够高效地处理比在笔记本电脑上可能处理的更大得多的<|message|> 集合。让我们来看一下 Eland 的一些功能：

- Eland 的一个关键用例是查询存储在 Elasticsearch 中的数据。用户无需编写原始 Elasticsearch 查询，而是可以编写类似于 pandas 语法的 Python 代码来过滤、排序和聚合数据。例如，你可以按如下方式从 Elasticsearch 索引中获取数据：
```
import eland as ed

df = ed.DataFrame("http://localhost:9200", "my_index")
filtered_df = df[df['some_field'] > 100]
```
- 除了数据查询和转换之外，Eland 还支持聚合，使用户能够生成汇总统计信息或对 Elasticsearch 数据计算复杂聚合。这对于理解大型数据集的分布和特征很有用。以下是如何计算特定字段平均值的示例：
```
average_value = df['some_field'].mean()
```
- Eland 还可用于数据可视化，因为它可与 Matplotlib 和 Seaborn 等流行的 Python 可视化库无缝集成。一旦使用 Eland 获取和操作数据，你就可以轻松地将结果转换为 pandas DataFrame 并创建可视化。以下是一个示例：
```
import seaborn as sns
import pandas as pd

filtered_df = df[df['some_field'] > 100]
pandas_df = filtered_df.to_pandas()
sns.boxplot(x='category', y='value', data=pandas_df)
```

最近，Eland 已扩展为将 NLP transformers 模型导入 Elasticsearch。Elasticsearch 目前支持在 PyTorch 中训练并符合标准 BERT 模型接口的一组选定架构。这允许 Elasticsearch 原生运行命名实体识别、零样本分类、问答等 NLP 任务。当前支持的任务列表可在文档中查看：https://www.elastic.co/guide/en/machine-learning/current/ml-nlp-model-ref.html。

与本书最相关的是对文本嵌入模型的原生支持。Eland 用于加载本地存储的模型，或更常见的是，直接从 Hugging Face 加载模型到 Elasticsearch。模型被下载然后分块，以便可以加载到 Elasticsearch 中。

现在我们将逐步介绍一个 Jupyter notebook，其中我们将配置 Elasticsearch 连接，从 Hugging Face 加载模型，部署（启动）模型，并生成测试向量。

完整代码可在本书 GitHub 仓库 chapter 3 文件夹中的 Jupyter Notebook 中查看：https://github.com/PacktPublishing/Vector-Search-for-Practitioners-with-Elastic/tree/main/chapter3。

在对 Eland 如何促进 Hugging Face 模型与 Elasticsearch 集成有了基础理解之后，让我们深入探讨使用代码的实际方面。接下来，我们将逐步了解如何将 Sentence Transformer 从 Hugging Face 高效加载到 Elasticsearch 中，阐明希望利用这些平台组合强大功能的人员的过程。

## 从 Hugging Face 加载 Sentence Transformer 到 Elasticsearch

此代码将向你展示如何将受支持的嵌入模型从 Hugging Face 加载到 Elastic Cloud 中的 Elasticsearch 集群：https://cloud.elastic.co/。

在这里，我们将安装并导入所需的 Python 库：

- eland
- elasticsearch
- transformers
- sentence_transformers
- torch==1.11

Elastic 使用 Eland 库从 Hugging Face Hub 下载模型并将它们加载到 Elasticsearch：

```
pip -q install eland elasticsearch transformers sentence_transformers torch==1.13
from pathlib import Path
from eland.ml.pytorch import PyTorchModel
from eland.ml.pytorch.transformers import TransformerModel
from elasticsearch import Elasticsearch
from elasticsearch.client import MlClient
```

### 配置 Elasticsearch 身份验证

推荐的身份验证方法使用 Elastic Cloud ID（https://www.elastic.co/guide/en/cloud/current/ec-cloud-id.html）和集群级 API 密钥（https://www.elastic.co/guide/en/kibana/current/api-keys.html）。

你可以使用任何你希望的方法来设置所需凭据。在此示例中，我们使用 getpass 来提示输入凭据以避免将它们存储在 GitHub 中：

```
import getpass
es_cloud_id = getpass.getpass('Enter Elastic Cloud ID: ')
es_api_id = getpass.getpass('Enter cluster API key ID: ')
es_api_key = getpass.getpass('Enter cluster API key: ')
# Connect to Elastic Cloud
es = Elasticsearch(cloud_id=es_cloud_id, api_key=(es_api_id, es_api_key))
es.info() # should return cluster info
```

### 从 Hugging Face Hub 加载模型

Elastic Stack ML 功能支持符合标准 BERT 模型接口并使用 WordPiece 分词算法的 transformer 模型。

有关 WordPiece 分词的深入解释，请查看 Hugging Face NLP 课程的这一部分：https://huggingface.co/learn/nlp-course/chapter6/6?fw=pt。

使用如图 3.6 所示的复制链接从 Hugging Face 下载嵌入模型。

对于此示例，我们将使用 sentence-transformers/msmarco-MiniLM-L-12-v3 模型：https://huggingface.co/sentence-transformers/msmarco-MiniLM-L-12-v3。

### 下载模型

在这里，我们指定来自 Hugging Face 的模型 ID。获取此 ID 的最简单方法是单击复制模型名称的图标，该图标位于模型页面上名称旁边。

![](img/21c3d4250a746422a7489bc2647611d8_78_0.png)

图 3.6：model_id 的复制位置

调用 TransformerModel 时，你指定 Hugging Face 模型的 ID 和任务类型。你可以尝试指定 auto，Eland 将尝试根据模型配置中的信息确定正确的类型。这并不总是可能的，因此可以设置特定 task_type 的列表：

```
hf_model_id='sentence-transformers/msmarco-MiniLM-L-12-v3'
tm = TransformerModel( model_id=hf_model_id, task_type="text_embedding")
```

