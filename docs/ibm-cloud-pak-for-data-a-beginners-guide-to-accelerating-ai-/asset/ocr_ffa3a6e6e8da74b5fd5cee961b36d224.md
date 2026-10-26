The following table shows available tools for building a model that classifies textual data:

| Tool | Code editor | Graphical canvas | Automated tool |
|---|---|---|---|
| Jupyter Notebook | ✓ |  |  |
| JupyterLab | ✓ |  |  |
| RStudio | ✓ |  |  |
| SPSS modeler |  | ✓ |  |
| Experiment builder |  | ✓ |  |

Figure 5.9 – Tools available for textual data classification

The following table shows tools for image data:

| Tool | Code editor | Graphical canvas | Automated tool |
|---|---|---|---|
| Jupyter Notebook | ✓ |  |  |
| JupyterLab | ✓ |  |  |
| RStudio | ✓ |  |  |
| Experiment builder |  | ✓ |  |

Figure 5.10 – Tools available for analytics on image data

We have seen the tools/technologies to handle different types of data. Let's now look into the process of establishing a collaborative development environment.

# Establishing a collaboration environment

In the building-AI-models phase of the AI life cycle, data scientists need to collaborate with data engineers and other data scientists to build the AI models, which could involve sharing data assets and/or existing data pipelines. For a given use case, data scientists train tens or hundreds of AI models before picking the best model. To support and maximize the productivity of the data science team, Cloud Pak for Data supports multiple modes of collaboration, including the following:

+   - Local project collaboration without Git
+   - Collaboration via Git

A local collaboration model enables all collaborators to work on one copy of assets in a project. Any asset being updated by one user (for example, a notebook) is locked until that user or an administrator unlocks it. This enables changes to be immediately available to all collaborators.