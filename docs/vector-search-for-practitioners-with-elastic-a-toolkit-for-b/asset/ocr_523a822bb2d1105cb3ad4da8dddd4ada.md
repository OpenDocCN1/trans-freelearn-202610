In the upcoming exercises, you will mainly need the Elasticsearch endpoint, but you might want to access Kibana to run queries, or for use when we build visualizations.

# Dense vector mapping

Now, let's take a look at how to build a mapping in Elasticsearch for dense vectors. It's a straightforward exercise; here is an example of a mapping:

```
{
  "mappings": {
    "properties": {
      "embedding": {
        "type": "dense_vector",
        "dims": 768,
      }
    }
  }
}
```

Simply put, the preceding example defines a field called embedding where the dense vectors will be stored. We set the dimensions as 768 because, in the following example, we use a BERT model, bert-base-encased, which has 768 hidden units. The BERT model is a pre-trained deep learning model used for NLP tasks, which processes lowercase text and can understand the context of a word based on its surrounding words:

```
!pip install transformers elasticsearch
import numpy as np
from transformers import AutoTokenizer, AutoModel
from elasticsearch import Elasticsearch
import torch

# Define Elasticsearch connection with credentials
es = Elasticsearch(
    ['https://hostname:port'],
    http_auth=('username', 'password'),
    verify_certs=False
)

# Define the mapping for the dense vector field
mapping = {
'properties': {
'embedding': {
'type': 'dense_vector',
dims': 768 # the number of dimensions of the dense vector
```