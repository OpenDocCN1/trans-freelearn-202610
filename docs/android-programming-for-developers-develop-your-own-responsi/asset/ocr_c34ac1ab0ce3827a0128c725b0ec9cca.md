Note that we are being careful to handle the error situations, so, to notify the result of the background work to the user, we will write an onPostExecute function that runs on the main thread that is going to publish a system notification that informs the user whether the task was completed successfully or failed miserably:

```
@Override
protected void onPostExecute(Result<JobParameters> result) {
    NotificationCompat.Builder builder =
        new NotificationCompat.Builder(jobService);

    ...

    if ( result.exc != null ) {
        // Error handling
        jobService.jobFinished(result.result, true);
        builder.setContentTitle("Failed to sync account")
            .setContentText("Failed to sync account " + result.exc);
    } else {
        // Success handling
        builder.setContentTitle("Account Updated")
            .setContentText("Updated Account Successfully at " +
                new Date().toString()));
        jobService.jobFinished(result.result, false);
    }
    nm.notify(NOTIFICACTION_ID, builder.build());
}
```

When the task is done, we invoke jobFinished(JobParameters params, boolean needsRescheduled) to let the system know that we are finished with that task; however, when an exception happens, we inform the system that we were unable to finish the task with success passing true on the second jobFinished argument.

When a finished job failed and needs to be rescheduled, we pass false as the second jobService.jobFinished argument, and the Scheduler API will reschedule our job using the back-off time specified in the JobInfo object. However, since our job only executes on idle mode, the failed job will be added to the scheduler queue and re-executed within a future idle maintenance window without using the back-off times specified in the JobInfo.

It is important to always call jobFinished to release the WakeLock assigned to the job and to inform the system that it can process additional jobs.