Choose **Create schedules** and provide a name for the schedule. We will currently schedule the data quality pipeline to be executed every hour using a recurring schedule, but you can set up more complex schedules using a cron-based approach as well, as seen here:

![](bbox=[0.179, 0.17, 0.828, 0.475])

Figure 7.55: EventBridge Scheduler – Setting up a schedule to run every hour

Under **Target API**, choose the **AWS Lambda (Invoke)** option and select the step function that we created in the last section, as shown here:

![](bbox=[0.192, 0.572, 0.81, 0.795])

Figure 7.56: EventBridge Scheduler – Calling the step function as the target

Leave the rest of the arguments at their defaults and create the schedule. Congrats! We have now set up a serverless data quality pipeline that is executed on a schedule.