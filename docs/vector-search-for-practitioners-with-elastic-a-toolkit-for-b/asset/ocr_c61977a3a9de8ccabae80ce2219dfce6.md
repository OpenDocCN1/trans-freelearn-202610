```
www.ward.com/index.htm HTTP/1.1" 200 3016', '3.178.238.218 - - [21/
Jan/2023:12:08:38 ] "PUT http://www.hobbs.com/ HTTP/1.1" 500 8772',
'110.184.88.144 - - [09/Jan/2023:20:20:37 ] "DELETE https://robertson.
com/tag/author/ HTTP/1.1" 404 3548'
```

The expanded logs become the following:

```
["On March 23rd, 2023 at 11:03:39 PM, a PATCH HTTP request was sent to http://www.obrien-arias.com/category/login.php and received a 200 response code with a payload size of 9727.",
"On March 11th, 2023 at 10:11:23 PM, a CONNECT HTTP request was sent to http://clark.com/terms/ and received a 200 response code with a payload size of 7022.",
"On April 6th, 2023 at 05:43:19 AM, a HEAD HTTP request was sent to https://www.ward.com/index.htm and received a 200 response code with a payload size of 3016.",
"On January 21st, 2023 at 12:08:38 PM, a PUT HTTP request was sent to http://www.hobbs.com/ and received a 500 response code indicating an internal server error, with a payload size of 8772.",
"On January 9th, 2023 at 08:20:37 PM, a DELETE HTTP request was sent to https://robertson.com/tag/author/ and received a 404 response indicating the page was not found, with a payload size of 3548."]
```

Expanding the logs makes it easier to find a general model to apply to the logs to vectorize them, which is what we are going to do in the next part.

# Semantic search on our logs

In this section, we will focus on transforming the expanded logs into vectors in Elasticsearch and then implementing a semantic search functionality on top of the vectorized content. We do this because—remember—our logs are now stored in human-readable language, so we can apply to them the principles of NLP and semantic search we saw earlier.

# Building a query using log vectorization

The following code takes the sequence of expanded logs to build a bulk indexing query for Elasticsearch:

```
# Generate the sequence of JSON documents for a bulk index operation
bulk_index_body = []
for index, log in enumerate(batchCompletion):
document = {
    "_index": "logs",
    "pipeline": "vectorize-log",
    "_source": {
        "text_field": log, "log": logs[index]
    }
}
bulk_index_body.append(document)
```