+   c) State differences between different versions of the same model.
+   d) Provide a high-level description of the data sources and feature engineering methods applied.

Now that we have properly described the model registered in MLflow, we can set it to one of the available stages. To do this, you will need to have the proper permissions at the user or model level. As mentioned before, we can choose to place our model in the None, Staging, Production, or Archived stage.

3. To transition a model between stages, we can use the MLflow client API `update_model_version()` method. If you don't have the appropriate level of permission, you can create a request for a model stage transition using the REST API. We will transition our model to the Production stage in the following code example:

```
mflow_client.transition_model_version_stage(
    name=model_details.name,
    version=model_details.version,
    stage='Production',
)
```

This results in the following output:

```
Cmd 17
1 from mlflow.tracking.client import MlflowClient
2
3 mflow_client = MlflowClient()
4
5 mflow_client.transition_model_version_stage(
6     name=model_details.name,
7     version=model_details.version,
8     stage='Production',
9 )
```

Out[30]: <ModelVersion: creation_timestamp=1617109202500, current_stage='Production', description ='', last_updated_timestamp=1617109209073, name='power_output_forecast', run_id='23d4e110709446b48931ffe518cc6165', run_link='', source='dbfs:/databricks/mlflow-tracking/197254732634776/23d4e110709446b48931ffe518cc6165/artifacts/model', status='READY', status_message='', tags={}, user_id='7882912336567795', version='4'>

Figure 11.4 – Transitioning the stage of a model