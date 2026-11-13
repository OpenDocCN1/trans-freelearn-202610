# Canceling a job

There are some situations where we want to provide for the users an ability to cancel the job because the environment situation has changed or it does not make sense to execute the job anymore—for example, the user changed a piece of information that the job depends on and the job is no longer applicable. The JobScheduler service offers us the support for job cancellation with the following cancel and cancelAll methods:

```
void cancel(int jobId);

void cancelAll();
```

The first method, cancel(jobId), allows us to cancel a specific job using the job identifier returned from the schedule(JobInfo job) function or the jobId available on JobInfo objects returned by the getAllPendingJobs function.

The cancelAll() method allows us to cancel the scheduled jobs that have been registered with the JobScheduler by the current application.

With JobInfo from the previous example we are able to cancel a specific job passing the job identifier:

```
final JobInfo ji = ...;
JobScheduler jobScheduler = (JobScheduler)
mContext.getSystemService(mContext.JOB_SCHEDULER_SERVICE);
// Cancel a Specific Job based on the JobInfo->jobId
jobScheduler.cancel(ji.getId());
```

Whenever we cancel a schedule, the job will be removed from the JobScheduler future execution queue and will no longer be executed by the SyncJobService or any other JobService.