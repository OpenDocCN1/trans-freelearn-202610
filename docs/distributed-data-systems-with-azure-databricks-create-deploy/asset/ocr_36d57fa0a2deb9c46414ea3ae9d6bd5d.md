MLflow has in Azure Databricks a Model Repository user interface (UI) in which we can set our models to respond to REST API requests for inference, transition models between stages, and visualize metrics and unstructured data associated with the models, such as description and comments. It gives us the possibility of managing our models either from the Model Registry API or directly from the Model Registry UI:

![](bbox=[0.12, 0.22, 0.85, 0.4])

Figure 11.1 – Models in the MLflow registry

In this section, we will learn the core concepts of MLflow and reinforce some of the methods that we have seen in previous chapters.

Before we jump into the actual way in which we can manage models in MLflow, let's look at the core concepts associated with models, as follows:

+   - **Model**: This is the actual MLflow model that is logged in to the registry from an experiment or run using the mlflow.[model-flavor].log_model methods, after which it can then be registered in the Model Registry.
+   - **Registered Model**: This is a model that was first logged in to the Model Registry and then later registered in the model repository. This should have a unique name and has properties such as versions, lineage, and other associated artifacts.
+   - **Model Stage**: Models in MLflow can be in one or more of the available stages that we can assign to it. The available options of the stage are None, Staging, Production, and Archived. Any user that has the proper read and write permissions can transition a model between stages.
+   - **Model Version**: This is indicative of the number of times that a model was committed to the repository. This means that when we register a model it is assigned Version 1, and every time we register a model with the same name, the version will be incremented.