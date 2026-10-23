

```
'address': fake.address(),
    'ssn': fake.ssn(),
    'birthdate': fake.date_of_birth(tzinfo=None, minimum_age=18, maximum_age=90).strftime('%Y-%m-%d'),
    'gender': fake.random_element(elements=('M', 'F'))
}
# Append the fake data to the list
fake_data.append(person)
return fake_data

# Generate fake personal identification information for N individuals
num_records = 10 # Set the desired number of records here
fake_pii_data = generate_fake_pii(num_records)
print(fake_pii_data)
```

前面的代码允许你设置要生成的文档数量。该数据集应具有足够的（虚假）PII，供你测试脱敏管道。

一旦这些测试文档生成后，你可以执行第二步，将文档批量索引到 Elasticsearch 中：

```
from elasticsearch import Elasticsearch, helpers
# Bulk Indexer
# Define a function to generate an array of documents for Elasticsearch
def generate_documents_array(fake_data, index_name):
# Create an empty list to store the documents
    documents = []
# Iterate over the fake data and format it as documents
    for person in fake_data:
# Create a document with the _index and _source keys
    document = {
        '_index': index_name,
        '_source': person
    }
# Append the document to the list of documents
    documents.append(document)
return documents
# Generate the array of documents for Elasticsearch
index_name = 'pii' # Set the desired index name here
documents_array = generate_documents_array(fake_pii_data, index_name)

# Initialize the Elasticsearch client
es = Elasticsearch(['HOSTNAME:PORT'],basic_auth=('USERNAME', 'PASSWORD'),verify_certs=False)
```

```
# Convert the bulk index body to a single string with newline separators
print("Bulk request: ")
print(documents_array)

try:
    response = helpers.bulk(es, documents_array)
    print ("\nRESPONSE:", response)
except Exception as e:
    print("\nERROR:", e)
After running this code, you should see the following type of document in the "pii" index in your Elasticsearch cluster:
{
    "_index": "pii",
    "_id": "Hzge9IcBeW7_RVqxfOJf",
    "_score": 1,
    "_source": {
        "first_name": "Jessica",
        "last_name": "Baker",
        "email": zrojas@example.com,
        "phone_number": "+1-537-947-4261",
        "address": "\"96702 Cannon Mall Serranofurt, KS 41609\"",
        "ssn": "811-48-4463",
        "birthdate": "1977-10-30",
        "gender": "M"
    }
}
```

在生成并索引了虚假数据后，我们需要配置默认的 ingest 管道和索引模板，并加载 NER 模型。

## 安装默认管道

要在 Elasticsearch 中设置 PII 脱敏管道，我们需要安装默认管道并将其配置为使用来自 Hugging Face 的提供的 NER 模型。步骤如下：

- 1. ingest pipeline 是一系列处理器，用于在数据被索引到 Elasticsearch 之前修改或丰富数据。在 PII 脱敏管道的上下文中，管道将用于从数据中删除敏感信息。要安装管道，你需要从 GitHub 仓库复制提供的 JSON 示例，并在 Elasticsearch 中使用此配置创建一个新管道。

你可以使用 Elasticsearch 的 REST API 按如下方式创建管道：

```
PUT _ingest/pipeline/pii_redaction_pipeline
{
  "description": "PII redacting ingest pipeline",
  "processors": [
    {
      "set": {
        "field": "redacted",
        "value": "{{message}}"
      }
    },
    {
      "inference": {
        "model_id": "dslim__bert-base-ner",
        "field_map": {
          "message": "text_field"
        }
      }
    },
    {
      "script": {
        "lang": "painless",
        "source": """String msg = ctx['message'];
        for (item in ctx['ml']['inference']['entities']) {
          msg = msg.replace(item['entity'], '<' + item['class_name'] + '>')
        }
        ctx['redacted']=msg""",
        "if": "return ctx['ml']['inference']['entities'].isEmpty() == false",
        "tag": "ner_redact",
        "description": "Redact NER entities"
      }
    },
    {
      "redact": {
        "field": "redacted",
        "patterns": [
          "%{PHONE:PHONE}",
          "%{SSN:SSN}"
        ],
        "pattern_definitions": {
          "SSN": """\d{3}-?\d{2}-?\d{4}""",
          "PHONE": """\d{3}-?\d{3}-?\d{4}"""
        }
      }
    }
  ]
}
```

```
}
    }
},
{
    "remove": {
        "field": [
            "message",
            "ml"
        ]
    }
}
],
"on_failure": [
    {
        "set": {
            "field": "failure",
            "value": "pii_script-redact"
        }
    }
]
}
```

- 2. 从 Hugging Face 加载并启动 NER 模型。

- 3. 你需要使用 Python 中的 eland 库从 Hugging Face Model Hub 加载一个兼容的 NER 模型。示例中使用的模型是 dslim/bert-base-NER，但你可以使用任何 Elastic 兼容的 NER 模型。

- 4. 要加载并启动模型，你可以按照以下步骤操作：

```
# Install the necessary Python packages:
pip install eland elasticsearch transformers sentence_transformers torch==1.13

#Import the required Python libraries:
from pathlib import Path
from eland.ml.pytorch import PyTorchModel
from eland.ml.pytorch.transformers import TransformerModel
from elasticsearch import Elasticsearch
from elasticsearch.client import MLClient
from elasticsearch.exceptions import NotFoundError

# Set up an Elasticsearch connection using your Elastic Cloud ID and cluster-level API key:
es = Elasticsearch(cloud_id=es_cloud_id,
                api_key=(es_api_id, es_api_key))
```

```
)
es.info() # should return cluster info

# Download the model from Hugging Face and load it into Elasticsearch:
hf_model_id='dslim/bert-base-NER'
tm = TransformerModel(model_id=hf_model_id, task_type="ner")

es_model_id = tm.elasticsearch_model_id()
es_model_id

# Load the model into Elastic Cluster
# <Load the model using the provided Python code from the GitHub repository>
```

- 5. 配置 inference processor 以使用 NER 模式 ID。

inference processor 负责在摄取的数据上运行 NER 模型。你需要配置处理器以使用你在上一步中加载的 NER 模型的 model_id。在 ingest pipeline 的 JSON 示例中，你会找到以下部分：

```
{
  "inference": {
    "model_id": "dslim__bert-base-ner",
    "field_map": {
      "message": "text_field"
    }
  }
}
```

将 model_id 值（"dslim__bert-base-ner"）替换为你已加载到 Elasticsearch 中的 NER 模型的实际 model_id。

- 6. 在设置好管道和 NER 模型后，你需要确保摄取到 Elasticsearch 索引中的数据首先经过管道。你可以通过将管道指定为索引的一部分来实现这一点。配置数据源或索引设置，以便进入索引的数据首先经过管道。请查阅文档以了解如何将 ingest pipeline 添加到索引请求中的示例（https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html#add-pipeline-to-indexing-request）。

在配置了 ingest pipeline 并加载了 NER 模型后，我们最终将配置索引模板。模板定义了字段数据类型，并配置写入索引的数据通过脱敏管道。

随附的 Python 笔记本包含完整的 Python 代码。我们使用以下设置定义管道：

```
"index.default_pipeline": "pii_redaction_pipeline_book"
```

最后一步是使用 Elasticsearch bulk index API 索引虚假数据并确认敏感数据已被脱敏。

完成此步骤后，输出文档应类似于以下示例：

```
{'_source':
  {
  'redacted': '<PER> called in from 001-<PHONE>x1311. Their account
  number is <SSN>'
  'status': 'active'
  }
```

你会注意到个人的姓名、电话号码和社会安全号码（SSN）已被脱敏。

## 预期结果

一旦在 Elasticsearch 中配置并设置了 PII 脱敏管道，预期结果如下：

- 当 NER 模型识别出一个实体时，该实体将被替换为用尖括号括起来的对应实体类型。例如，如果 NER 模型识别出一个人的姓名，它将被替换为 <PERSON>。
- 当正则表达式模式匹配一个值时，该值将被 <redacted> 术语替换。这确保了敏感信息，例如电话号码和 SSN，在处理后的数据中永远不可见。

在设置了示例 PII 脱敏管道后，让我们看看如何进一步自定义它以满足组织的特定需求。

