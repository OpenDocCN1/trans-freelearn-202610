Clicking on the **Add New Trigger (+)** button on the **Triggers** page opens a new page to create a new trigger. The options shown on the screen depend on the type of trigger you select. *Figure 3.21* shows the inputs required to create a time-based trigger:

![](bbox=[0.135, 0.17, 0.865, 0.535])

Figure 3.21 – Time-based trigger creation in UiPath Orchestrator

Time-based trigger creation requires the options shown in *Figure 3.21*. The following describes each option on the screen:

+   - **Name**: The name of the trigger.
+   - **Timezone**: The timezone to consider for the time-based trigger.
+   - **Process Name**: Includes a dropdown that consists of all the processes available in Orchestrator. The user must select which process to execute based on the trigger.
+   - **Job priority**: Similar to the job creation screen, this property describes which priority to consider when creating a job through a trigger.
+   - **Frequency**: Select the frequency at which the process is triggered. For example, if the process needs to be executed every 3 hours, the frequency must be set to **Hourly** and the time interval of **Every 3 hours** must be selected.