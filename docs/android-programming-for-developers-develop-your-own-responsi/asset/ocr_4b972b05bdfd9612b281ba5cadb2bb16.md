# # Introduction to JobScheduler

The Android development team, under the project Volta umbrella, introduced in the API Level 21 Lollipop release some enhancements and features in order to improve the power usage on the Android platform. Apart from the tools introduced to monitor and follow the battery usage in the Android platform, a new API for scheduling background was officially released to help the developer. It saves extra power cycles when the jobs used to support the developer's application do not need a time of execution, and can be deferred until the device has better battery and network context.

The API was not created to completely replace AlarmManager; however, the JobScheduler API is able to perform a better battery management and supply extra behaviors.

The main features introduced with Scheduler API are as follows:

+   - **Less power consumption**: The job task could be delayed until the device is powered to the charger or they are grouped to run in batches on a regular basis
+   - **Persistent jobs across reboots**: We are able to install job schedules that persistent task across device reboots
+   - **Better network bandwidth management**: The job could be delayed until a higher bandwidth network is available, such as when a Wi-Fi network connection is available
+   - **Less intrusive execution**: The job could be delayed until the user is not interacting with the device

JobScheduler is a singleton system service that we can retrieve via a Context object instance, using a code similar to the following:

```
JobScheduler js = (JobScheduler)
getSystemService(Context.JOB_SCHEDULER_SERVICE);
```

The JobScheduler singleton service instance object helps us to manage our running jobs and provides us with member functions to schedule, cancel, and retrieve a list of deferred jobs.