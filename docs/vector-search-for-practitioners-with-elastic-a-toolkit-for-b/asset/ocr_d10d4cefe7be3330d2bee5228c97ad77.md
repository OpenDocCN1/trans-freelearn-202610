# Performing the bulk index operation

With our bulk_index_body array ready, we can now perform the bulk index operation:

```
try:
    response = helpers.bulk(es, bulk_index_body, chunk_size=500,
    request_timeout=60*3)
    print ("\nRESPONSE:", response)
except BulkIndexError as e:
    for error in e.errors:
        print(f"Document ID: {error['index']['_id']}")
        print(f"Error Type: {error['index']['error']['type']}")
        print(f"Error Reason: {error['index']['error']['reason']}")
```

We use the helpers.bulk() function from the Elasticsearch library to provide our Elasticsearch connection (es)—the bulk_index_body array we just created—with a chunk_size value of 500 (which specifies that we want to send 500 documents per request) and a request_timeout value of 180 seconds, which specifies that we want to allow each request to take up to 3 minutes before timing out because the indexing could take a long time with ELSER.

The helpers.bulk() function will return a response indicating the number of operations attempted and the number of errors, if any.

If any errors occur during the bulk index operation, these will be raised as BulkIndexError. We can catch this exception and iterate over its errors attribute to get information about each individual error, including the ID of the document that caused the error, the type of error, and the reason for it.

At the end of this process, you will have successfully indexed your entire Allrecipes.com dataset in Elasticsearch, ready for it to be retrieved and processed by your RAG-enhanced CookBot.

# Building the retriever—RRF with ELSER

In the context of our recipe retrieval task, our goal is to maximize the relevance of the returned recipes based on a user's query. We will utilize a combination of classic full-text search (via BM25), semantic search (with ELSER), and a robust rank fusion method (RRF). This combination allows us to handle more complex queries and return results that align closely with the user's intent.

Let's consider the following query:

```
GET recipes/_search
{
  "_source": { "includes": [ "name", "ingredient" ] },
  "sub_searches": [
    {
      "query": {
        "bool": {
```