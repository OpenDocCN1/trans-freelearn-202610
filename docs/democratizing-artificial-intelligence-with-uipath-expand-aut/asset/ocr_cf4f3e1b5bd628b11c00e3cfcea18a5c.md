And that's it! Within Figure 9.33, we can see what our End Process sequence should resemble. In this section, we successfully added retraining capabilities to our automation project by first building a full pipeline to retrain our ML package every 7 days and then building an automation workflow to extract validated emails and add the validation to our training pipeline. With the development and testing portions complete, in the next section, we will venture into deploying the automation into UiPath Assistant.

# Deploying with the end-user experience in mind

With the initial development and testing of the use case complete, let’s venture into deploying the automation into a production environment. In this section, we will work on deploying the automation into UiPath Assistant so that it can be run in an attended scenario.

# Deploying the project

In a production setting, we shouldn’t run our automation directly from UiPath Studio, nor should we expect our end users to have access to UiPath Studio. Thus, we must deploy our automation into UiPath Orchestrator to allow others to run our automation from UiPath Assistant.

> **Important Note**
> More information on deploying automation can be found at the following link:
> `https://docs.uipath.com/studio/docs/about-publishing-automation-projects`