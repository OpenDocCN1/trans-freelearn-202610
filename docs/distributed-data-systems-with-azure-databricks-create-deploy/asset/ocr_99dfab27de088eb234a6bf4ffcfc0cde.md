# Technical requirements

This chapter will require you to have an Azure Databricks subscription available to work on the examples in notebooks attached to running clusters.

Without further ado, we can start by looking into the different ways in which we can install libraries in Azure Databricks and the differences of each method.

# Installing libraries in Azure Databricks

We can make use of third-party or custom code by installing libraries written in Python, Java, Scala, or R. These libraries will be available to notebooks and jobs running on your clusters depending on the level at which the libraries were installed.

In Azure Databricks, installing libraries can be done in different ways, the most important decision being at which level we will be installing these libraries. The options available are at the workspace, cluster, or notebook level:

+   - **Workspace** libraries serve as a local repository from which you create cluster-installed libraries. A workspace library might be custom code created by your organization or might be a particular version of an open-source library that your organization has standardized on.
+   - **Cluster** libraries are available to be used by all notebooks attached to that cluster. You can install a cluster library from a public repository or create one from a previously installed workspace library.
+   - **Notebook-scoped**: Python libraries allow you to install Python libraries and create an environment scoped to a notebook session. Notebook-scoped libraries will not be visible in other notebooks running on the same cluster. These libraries do not persist and need to be installed at the beginning of each session. Use notebook-scoped libraries when you need a custom Python environment for a specific notebook. With these libraries, it is easy to create, modify, save, reuse, and share Python environments. Notebook-scoped libraries are available using `%pip` in all Databricks Runtime instances and using the `%conda` magic commands in Databricks Runtime ML 6.4.