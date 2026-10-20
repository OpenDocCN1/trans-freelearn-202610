The model has now been logged in to the MLflow Model Registry, and all the related artifacts and performance metrics can be tracked there. We can now register the model using the MLflow API or the Model Registry UI in the Azure Databricks workspace:

+   1. To create a new registered model using the API, we can use the mlflow.register_model() function. This way, we will create a new model with a Version 1 name. We will specify a path where the model will be stored and a model name to do this, as follows:

```
model_path = "wind_farm_model"
model_name = "power_output_forecast"
model_uri = f"runs:/{run_id}/model"
model_details = \
mlflow.register_model(model_uri=model_uri,
    name=model_name)
```

+   2. We can use MLflow not only to store the model artifacts but also to provide valuable context relating to the development of the model using descriptions and keeping lineage control, stating the model's version, as illustrated in the following code snippet:

```
from mlflow.tracking.client import MlflowClient
description="This model was trained as an example for the Azure Databricks book with data from US wind farms to predict the power output based on wind speed, direction and air temperature."
mflow_client = MlflowClient()
mflow_client.update_registered_model(
    name=model_details.name,
    description= description
)
```

Providing meaningful descriptions helps to keep track of the purpose of the context of the development of the model, provide details about its attributes and usage, and—particularly—explicit the algorithm that was used and the data source used in the training. Descriptions then can be used to do the following:

+   a) Provide context that drives the development of the model.
+   b) Mention the methodology used and algorithm selected.