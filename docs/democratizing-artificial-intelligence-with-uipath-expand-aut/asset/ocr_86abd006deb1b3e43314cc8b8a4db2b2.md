# Building an automation solution

With our chatbot built, we can shift our focus to creating automation workflows for the use case. Approaching this use case, we will build our project in a couple of steps, as follows:

+   1. Creating a project
+   2. Building `Reset_Password` and `Upgrade_App` workflows
+   3. Building the **Main** workflow

After completing these steps, we will have the foundation of our project complete and ready for testing.

# Creating a project

For this project, we'll create a new blank project in Studio. To start a project, complete the following steps:

+   1. Navigate to the **Start** tab of UiPath Studio.
+   2. Choose **Process**.
+   3. Name your project `IT_Helpdesk_Chatbot`, and provide a folder location to save the project to.
+   4. Click **Create** to create a project and to get started.

That concludes creating a project. We'll revert to the **Main** workflow in a later section where we build out the workflow, but for now, we'll transition into building supplemental workflows.

# Building Reset_Password and Upgrade_App workflows

With an automation project created, we can focus on creating some of the supplemental workflows that'll become building blocks of our **Main** workflow. Because we don't have actual accounts to reset or an application license to upgrade, our objective is to create modular workflow sequences that can simulate password resets or application license upgrades.

To start, let's create a `Reset_Password` workflow, as follows:

+   1. In the `IT_Helpdesk_Chatbot` project, create a new workflow file called `Reset_Password`.
+   2. Once the new workflow is created, create two arguments, as shown in the following screenshot: