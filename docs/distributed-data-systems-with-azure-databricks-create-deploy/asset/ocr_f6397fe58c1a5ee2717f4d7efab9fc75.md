It is useful to note that we have two types of experiment in MLflow, outlined as follows:

+   - **Workspace experiment**: This is not associated with any notebook. Any job or notebook can log a run into one of them by specifying the workspace experiment ID.

+   - **Notebook experiment**: This is associated with a specific notebook and it's the default option when no experiment ID was passed as a parameter of the run. Only MLflow runs within that specific notebook can be logged into the Notebook experiment.

Workspace experiments can be created in the Azure Databricks Workspace UI or by using the MLflow API:

+   - We can use the runs of the experiments to query the metrics and parameters used on them. For example, if we want to query the runs in an experiment with an **R-squared (R2)** metric greater than 0.3, we can go to the Search Runs field in the Experiment tab in the Notebook toolbar and enter the following expression:

```
metrics.r2 > 0.3
```

+   - In a similar way, we can filter the runs that have a certain set of parameters using the following syntax:

```
params.max_depth = 4
```

```
params.max_depth = 4 AND metrics.accuracy > 0.8
```

+   - You also have the option to filter the runs based on the state or version of a model by using the Filter option in the Search Runs box. You can access drop-down menus with the State and Linked models.

+   - We can track the version of the Notebook or Git project used for a run in the following ways:

+   a) We can click in the **Source** field of a run if this was created from an Azure Databricks notebook or job.

+   b) If the run was created from a Git project, the **Source** field of the run will be a link to the branch of the Git project used to trigger the run.

+   - Finally, if it's a workspace experiment and the run was logged in to the experiment, the model will be displayed as an artifact that can be downloaded from the registered model page in the Model Registry UI.

Now that we have learned a bit more about the concepts of experiments and runs, we will learn in the next sections how to register and manage the lifecycle of a machine or deep learning model in Azure Databricks.