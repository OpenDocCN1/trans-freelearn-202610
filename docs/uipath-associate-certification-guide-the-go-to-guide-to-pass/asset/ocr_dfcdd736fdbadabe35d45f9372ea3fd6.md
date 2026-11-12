28. Click **Next** and then the **Publish** button from the **Publish Process** popup. As we are using the default workspace folder, the published project will be available in the default folder of Orchestrator. You will see the following confirmation message upon publishing the project from Studio:

![](bbox=[0.23, 0.19, 0.77, 0.38])

Figure 4.52 – The published project

In this section, you have successfully created your second RPA process and published it to Orchestrator. In the upcoming section, let’s see how to deploy the published processes from Orchestrator to be executed by UiPath Assistant.

> **Important Note**
> Try the given project with the **Get Full Text** activity instead of **Get Visible Text** and observe the differences. You can similarly play around with the different web scraping activities that are available to make your learnings even more interesting!

# Managing packages in Orchestrator

The package published from UiPath Studio must be available in UiPath Orchestrator. The package will be deployed further and assigned to the correct robot for execution. Let’s see the step-by-step instructions on how to perform these operations.

# Using the latest version of the published package

Every time you publish a package to Orchestrator, the version number is updated. Therefore, it is essential to use the latest package to get all the latest updates and/or code. Follow these step-by-step instructions to use the latest version:

+   1. Navigate to the **Packages** tab in Orchestrator: