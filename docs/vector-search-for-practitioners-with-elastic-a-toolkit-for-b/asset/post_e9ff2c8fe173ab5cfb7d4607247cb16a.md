
With page-cache memory requirements being particularly important to kNN search, users need to be mindful of how many vectors fit in a node’s available page-cache RAM. Assuming the default configurations for Hierarchical Navigable Small World (HNSW) are used, a loose calculation for the number of bytes required is as follows:

```
num_vectors * 4 * (num_dimensions + 12) = total RAM in bytes
```

As an example, if you are using a model that outputs 512-dimensional vectors and you expect to load 20 million vectors into Elasticsearch, the formula would look like this:

```
20,000,000 * 4 * (512 + 12) = 41,920,000,000 bytes
41,920,000,000 / 1024 ~= 39 GB RAM
```

Twenty million embedding fields translates to a loose estimate of requiring approximately 39 GB of off-heap RAM for kNN search to be most performant. If you consider a full-sized node in Elasticsearch to be 64 GB—half going to the Elasticsearch JVM, and several GB going to the OS and any other processes—you are left with a starting estimate of 2 data nodes to handle those vectors. Also, keep in mind that is without considering replicas.

This is a starting estimate and a good place for when you start your testing. It is not intended to be an exact RAM requirement. Testing, covered later in the chapter, will give you the best numbers before moving to production.

Having set the stage with the requirements and implications of RAM usage, we pivot our attention to another critical resource in the ecosystem: storage.

## Disk requirements

As we’ve seen, one of the primary considerations for performant kNN search is ensuring there is adequate off-heap RAM available to store vectors. However, it is important to note that those vectors are still generated and stored on disk. As such, users have to ensure there is adequate storage available. However, due to the size of disks versus the size of RAM, disk capacity is generally not the limiting factor when planning cluster capacity.

The formula to estimate the required RAM mentioned in the previous section is the same calculation for disk capacity for vector data, but you will also have to take into consideration any other fields you are storing along with the vectors, such as the text of the vectors, other text fields, and metadata.

A rough starting estimate for disk capacity is as follows:

```
size of a single document * number of documents * number of replicas * 1.2
```

Here, 1.2 is the net multiplier between JSON expansion and disk compression. Please note that this is a very rough estimate.

However, the only true way to get an idea of how your data will behave in Elasticsearch is to load a sample dataset. You can easily use a free trial of Elastic Cloud to spin up a small cluster and load a small representative dataset. Starting a trial is quick and only requires an email address at https://cloud.elastic.co/registration.

You will have access to a small, fully functional Elasticsearch cluster including an ML node and all Enterprise features. The Getting Started guides at https://www.elastic.co/getting-started can help you get up and running quickly.

Once you have loaded your data, you’ll be able to see the exact size your data will expand out to, which is the only true way to plan for your production requirements. Even past the free trial, you can grow your cluster slowly if needed with easy month-to-month payments.

You can use the disk usage API covered in the next section to see disk usage, and we cover cluster tuning, load testing, and full scaling testing in the next chapter.

Now that we’ve discussed the importance of understanding your data size and scaling implications, it’s paramount to have tools that provide precise insights into the real-world utilization of these resources. One such crucial tool in the Elasticsearch suite is the analyze disk usage API. Let’s explore its functionality and how it aids in understanding disk space consumption.

## Analyze Index Disk Usage API

Once vectors (and any other data) have been loaded into Elasticsearch, you can use the **analyze index disk usage API** to calculate how much disk space is being taken up. For more information on the analyze disk usage API, visit the documentation page at https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-disk-usage.html.

The search-elastic-docs index has multiple vector fields that are relatively small in data size. One of those is title-vector:

```
POST /search-elastic-docs/_disk_usage?run_expensive_tasks=true&pretty
```

This gives the following response (the full response of the API call is very long, so we are only showing the truncated output containing relevant parts here):

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

We can see that the vectors are taking up 83.4 MB. With disk usage analyzed, we can turn our attention to ML node capacity planning.

## ML node capacity

For embedding models run on ML nodes in Elasticsearch, you will need to plan to ensure your nodes have enough capacity to run the model at inference time. Elastic Cloud allows the auto-scaling of ML nodes based on CPU requirements, which allows them to scale up and out when more compute is required and scale down when those requirements are reduced.

We cover tuning ML nodes for inference in the next chapter in more detail, but minimally, you will need an ML node with enough RAM to load at least one instance of the embedding model. As your performance requirements increase, you can increase the number of allocations of the individual model as well as the number of threads allocated per allocation.

To check the size of a model and the amount of memory (RAM) required to load the model, you can run the get trained models statistics API (for more information on this API, visit the documentation page at https://www.elastic.co/guide/en/elasticsearch/reference/current/get-trained-models-stats.html#get-trained-models-stats):

```
GET _ml/trained_models/dslim__bert-base-ner-uncased/_stats?human
```

This will return various stats, including model size and memory requirements:

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

When a model is running, you can also get stats about it in the Kibana UI by going to Machine Learning > Model Management > Trained Models and expanding the model ID.

When planning ML node capacity, two distinct situations need to be considered:

- At indexing time: Generally, there is an initial large bulk load of documents into an Elasticsearch index, which requires vector generation. This will put a large demand on your ML nodes to run inference as quickly as possible. This is where the ability to easily scale ML nodes and increase the number of model allocations dynamically is very useful. You can scale out the model allocations while your initial bulk load is running, and then reduce the allocations when that load is complete.

- Around search time inference: When a search query is sent by a user, you will need to use the same embedding model to generate a query vector that you used to generate the document vector. Keeping up with your average query load will allow you to plan your normal operating size. Should you have a planned spike in query load approaching, for example, a busy shopping weekend in e-commerce, you can proactively increase the number of model allocations and, in turn, scale the ML nodes.

The only true way to test both of these conditions is with load testing and performance tuning on your actual data, which we cover in more depth in the next chapter.
