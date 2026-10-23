Still in Kibana | Dev Tools, we are now going to create a new mapping and add our new field:

![](bbox=[0.115, 0.112, 0.845, 0.47])

Figure 8.9: New mapping with the rank features field

Now that our destination is ready, we need to build the ingestion pipeline, which will take our initial index data and index it into our new index.

# Ingestion pipeline

The ingestion pipeline in this case is fairly simple; it involves taking the right source field and using the right inference from the ELSER model to expand the text and build our token sparse vector. This vector will be saved into the ml.tokens field.

(You can read more about the Ingest Pipeline API here: https://www.elastic.co/guide/en/elasticsearch/reference/current/inference-processor.html.)