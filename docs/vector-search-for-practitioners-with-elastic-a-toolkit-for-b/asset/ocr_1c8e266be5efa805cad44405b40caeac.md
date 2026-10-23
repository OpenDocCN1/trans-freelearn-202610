```
"bool": {
    "filter": [
        {
            "prefix": {
                "url": "https://www.elastic.co/guide"
            }
        }
    ],
    "must": [{
        "match": {
            "title": {
                "query": query_text,
                "boost": 1
            }
        }
    }]
}

fields = ["title", "body_content", "url"]
index = 'search-elastic-doc'
resp = es.search(index=index,
                query=query,
                fields=fields,
                size=10,
                source=False)

body = resp['hits']['hits'][0]['fields']['body_content'][0]
url = resp['hits']['hits'][0]['fields']['url'][0]
elastic_bot = App()
# Assuming 'resp' is the response object you've mentioned
for hit in resp['hits']['hits']:
    for url in hit['fields']['url']:
        print(url)
        #es_app.add(url)
        elastic_bot.add(url)

return elastic_bot.query("What can you tell me about " + query_text)
```

Note that we are keeping the prompt broad by prefixing the user input with “what can you tell me about.”