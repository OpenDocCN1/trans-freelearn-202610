Next, we set and confirm the model ID. To make the name compatible with Elasticsearch, the / is replaced with __ (double underscore):

```
es_model_id = tm.elasticsearch_model_id()
```

Export the model in a TorchScript representation, which Elasticsearch uses:

```
tmp_path = "models"
Path(tmp_path).mkdir(parents=True, exist_ok=True)
model_path, config, vocab_path = tm.save(tmp_path)
```

# Loading the model into Elasticsearch

Here, the model will be serialized and loaded into Elasticsearch so it can then be started on an ML node:

```
ptm = PyTorchModel(es, es_model_id)
ptm.import_model(model_path=model_path, config_path=None, vocab_path=vocab_path, config=config)
```

# Starting the model

This step will give us information about the model, such as the name and status, as the output:

```
# List the in Elasticsearch
m = MLClient.get_trained_models(es, model_id=es_model_id)
```

# Deploying the model

The following code will load the model on the ML nodes and start the process(es) of making it available for the NLP task:

```
s = MLClient.start_trained_model_deployment(es, model_id=es_model_id)
```

Next, we will verify that the model starts without an issue:

```
stats = MLClient.get_trained_models_stats(es, model_id=es_model_id)
stats.body['trained_model_stats'][0]['deployment_stats']['nodes'][0]['routing_state']
```

You should see the following output, letting you know the model has been deployed to the ML nodes:

```
{'routing_state': 'started'}
```