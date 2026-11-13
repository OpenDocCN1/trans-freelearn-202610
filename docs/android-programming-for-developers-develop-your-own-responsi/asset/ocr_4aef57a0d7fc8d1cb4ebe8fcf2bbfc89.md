backoff initial value that determines the interval between retries and the retry increase policy. The JobScheduler API provides two policies that define the way in which the retry time increases between subsequent tries:

+   • BACKOFF_POLICY_LINEAR: The interval time between retries increases linearly—initial_backoff_millis * num_retries

+   • BACKOFF_POLICY_EXPONENTIAL: The interval time between retries increases exponentially—initial_backoff_millis * 2 ^ (num_retries)

![](bbox=[0.17, 0.25, 0.89, 0.51])

The backoff interval will increase until we reach a backoff of five hours (JobInfo. MAX_BACKOFF_DELAY_MILLIS) and the initial default value is 30 seconds (JobInfo .DEFAULT_INITIAL_BACKOFF_MILLIS).

Now, in the following examples we are going to show you how to create a linear and an exponential backoff policy for our jobInfo object:

```
```
// Initial Backoff of 10 minutes that grows linearly
jobIBuilder.setBackoffCriteria(TimeUnit.MINUTES.toMillis(10L),
JobInfo.BACKOFF_POLICY_LINEAR);
```
```

```
```
// Initial Backoff of 3 minutes that grows exponentially
jobIBuilder.setBackoffCriteria(TimeUnit.MINUTES.toMillis(3),
JobInfo.BACKOFF_POLICY_EXPONENTIAL);
```
```

The builder class also provides us a method to set some parameters to forward the job over a PersistableBundle object: