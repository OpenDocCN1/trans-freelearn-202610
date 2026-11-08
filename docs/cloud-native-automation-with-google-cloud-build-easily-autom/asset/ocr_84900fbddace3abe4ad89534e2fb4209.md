Another mechanism that leverages the manual trigger is when build pipelines need to execute at a specific time or interval. Cloud Scheduler has direct integration with Cloud Build in order to execute a Cloud Build manual trigger at a specified time. From a Cloud Build manual trigger, you can run a trigger on a schedule, which can be created via the UI:

![](bbox=[0.24, 0.19, 0.76, 0.74])

Figure 5.7 – Cloud Scheduler job creation

Going through the wizard, you select the service account that will be used by Cloud Scheduler to invoke the trigger. A default service account is created or one that is set up with the appropriate permissions can be specified.