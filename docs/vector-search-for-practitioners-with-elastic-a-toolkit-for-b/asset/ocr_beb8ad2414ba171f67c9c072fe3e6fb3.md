![](bbox=[0.15, 0.1, 0.815, 0.46])

Figure 7.3: Loaded trained model in Kibana

# Ingest pipeline

Now, you might have noticed in the previous bulk indexing code that we reference a pipeline called vectorize-log. This pipeline is intended to use the model that we loaded to generate vectors from the expanded logs. To define a pipeline, you can go to Kibana | Stack Management | Ingest Pipelines or try my preferred technique, which is to push a pipeline via the create pipeline API (https://www.elastic.co/guide/en/elasticsearch/reference/current/put-pipeline-api.html).

Here is the definition of our pipeline:

```
[
  {
    "inference": {
      "model_id": "sentence-transformers__all-distilroberta-v1",
      "target_field": "description_vectorized"
    }
  },
  {
    "set": {
      "field": "description_vectorized",

```