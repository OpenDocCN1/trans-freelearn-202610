

You will see your documents with the embedding field containing the dense vectors.

There are some caveats if you don’t customize the mapping slightly, which we will look at in the next section.

## Brute-force kNN search

As said, there is a caveat with the previous example, and it concerns the fact that the vector field is not indexed by default, which means that you cannot use it with the kNN endpoint. You can use the vectors in the script score function, with a couple of similarity functions available out of the box to perform a brute-force or exact kNN search, as explained at this link: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-script-score-query.html#vector-functions. The script scoring query is particularly useful when you want to avoid applying the scoring function on all documents searches, and only apply it on the filtered set of documents. The drawback is that the more filtered the document, the more expensive the script score can be, and it linearly increases.

Elasticsearch offers the following out-of-the-box similarity functions:

- CosineSimilarity: Calculates cosine similarity
- dotProduct: Calculates the dot product
- l1norm: Calculates the L1 distance
- l2norm: Calculates the L2 distance
- doc[<field>].vectorValue: Returns a vector’s value as an array of floats
- doc[<field>].magnitude: Returns a vector’s magnitude

Thankfully, at this stage of the chapter, you have a good understanding of what the preceding represents, and you also know that the recommended similarities for vectors are the first four options.

As a rule of thumb, if the number of documents after filtering is under 10,000 documents, then not indexing and using one of the similarity functions should give good performance.

The last two options refer to situations where you want to access vectors directly. While it gives an important level of control to users, the performance of the code will depend on how narrowed down the document set is and the quality of the script on multidimensional vectors.

## kNN search

In this section, we’ll explore how to activate kNN search for your indices and execute an illustrative example.

### Mapping

Now, to be able to use kNN search, you need to modify the mapping so that the dense vectors are indexed, as follows:

```
{
  "mappings": {
    "properties": {
      "embedding": {
        "type": "dense_vector",
        "dims": 768,
        "index": true,
        "similarity": "dot_product"
      }
    }
  }
}
```

Note that you also need to set the similarity function. The three options for the similarity are l2_norm, dot_product, and cosine. We recommend, when possible, to use dot_product for vector search in production. Using the dot product avoids you having to calculate the vector magnitudes for every similarity computation (because the vectors are normalized in advance to all have a magnitude of 1). This means it can improve search and indexing speed by ~2-3x.

The difference with the brute-force kNN search based on the script scoring query is that, in this case, the HNSW graph is built and stored in memory. In fact, it is stored at the segment level, and this is why:

- Force merging is recommended at the index level to merge all the segments in an index into a single segment. It will not only optimize the search performance but also avoid HNSW being rebuilt at any segments. To merge all segments, the following API can be called:
  POST /my-index/_forcemerge?max_num_segments=1

- Updating a document, thus the dense vector, is not recommended at scale, since this would rebuild the HNSW again.

### An example illustrating the use of kNN search

As a reminder, the kNN search API is used to find the k approximate nearest neighbors to a query vector. The query is also a vector of numbers that represent the text search query. The k nearest neighbors are the documents whose vectors are the most similar to the query vector.

The following example illustrates a fun way to use Elastic to build a jokes database. In this, the Python notebook builds a joke index, where the BERT model is used to represent jokes as vectors. It vectorizes a query string and then uses it in a kNN search to get similar jokes.

We start by importing the dependencies and defining the connection to our cluster:

```
!pip install transformers elasticsearch
import numpy as np
from transformers import AutoTokenizer, AutoModel
from elasticsearch import Elasticsearch
import torch

es = Elasticsearch(
['https://host:port'],
http_auth=('username', 'password'),
verify_certs=False
)
```

Then we set our index mapping as well as defining a couple of documents representing jokes:
```
mapping = {
'properties': {
'embedding': {
'type': 'dense_vector',
'dims': 768,
'index': true,
"similarity": "cosine"
}
}
}
```

es.indices.create(index='jokes-index', body={'mappings': mapping})

jokes = [
{
'text': 'Why do cats make terrible storytellers? Because they only have one tail.',
'category': 'cat'
},
[... other funny dad jokes ...]
{
'text': 'Why did the frog call his insurance company? He had a jump in his car!',
'category': 'puns'
}]
```

We will now load our BERT model, generate the embeddings, and index it in Elasticsearch:

```
# Load the BERT tokenizer and model
tokenizer = AutoTokenizer.from_pretrained('bert-base-uncased')
model = AutoModel.from_pretrained('bert-base-uncased')

# Generate embeddings for the jokes using BERT
for joke in jokes:
    text = joke['text']
    inputs = tokenizer(text, return_tensors='pt', padding=True,
                       truncation=True)
    with torch.no_grad():
        output = model(**inputs).last_hidden_state.mean(dim=1).squeeze(0).numpy()
    joke['embedding'] = output.tolist()

# Index the jokes in Elasticsearch
for joke in jokes:
    es.index(index='jokes-index', body=joke)
```

Lastly, we define a query, convert it to a query vector, and run a vector search in Elasticsearch:

```
query = "What do you get when you cross a snowman and a shark?"
inputs = tokenizer(query, return_tensors='pt', padding=True,
                   truncation=True)
with torch.no_grad():
    output = model(**inputs).last_hidden_state.mean(dim=1).squeeze(0).numpy()
query_vector = output

# Define the Elasticsearch kNN search
search = {
  "knn": {
    "field": "embedding",
    "query_vector": query_vector.tolist(),
    "k": 3,
    "num_candidates": 100
  },
  "fields": [ "text" ]
}
# Perform the kNN search and print the results
response = es.search(index='jokes-index', body=search)
for hit in response['hits']['hits']:
    print(f"Joke: {hit['_source']['text']}")
```

Here, we use the joke, “What do you get when you cross a snowman and a shark?” as the query.

The joke is not present in the dataset but has a similar semantic to returned jokes:

```
Joke: What did the cat say when he lost all his money? I am paw.
Joke: Why do cats make terrible storytellers? Because they only have one tail.
Joke: Why don't cats play poker in the jungle? Too many cheetahs.
```

Also, note the structure of the kNN search query:

```
search = {
"knn": {
"field": "embedding",
"query_vector": query_vector.tolist(),
"k": 3,
"num_candidates": 100
},
"fields": [ "text" ]
}
```

The query requires the dense vector field (here, the field called embedding) as well as the vector representation of the text query, k, and num_candidates.

In kNN search, the API first finds a certain number of approximate nearest neighbor candidates on each shard; the number is num_candidates. Then, it calculates the similarity between these candidates and the query vector, selecting k most similar results from each shard. Finally, the results from each shard are merged to obtain the top k nearest neighbors in the entire dataset.

## Summary

At this stage of the book, you should have a pretty good understanding of the fundamentals of vector search, including vector representation, how vectors are organized in an HNSW graph, and the method to calculate similarity between vectors. In addition, we have seen how to set up your Elastic Cloud environment as well as your Elasticsearch mapping to run Vector Search queries and leverage the k-nearest neighbors algorithm.

Now, you are equipped with the fundamental knowledge to explore all the subsequent chapters. We’ll discover vector search domains of applications in various code examples and fields such as observability and security.

In the following chapter, we will go a step further – we’ll not only learn how to host a model and generate vectors within Elasticsearch, as opposed to handling it externally, but also explore the intricacies of managing it at different scales and optimizing a deployment from a resource standpoint.

