# # Jobs

Jobs is the default tab of Spark UI. It shows the status of all the applications executed within a SparkContext. It can be accessed at http://localhost:4040/jobs/.

It consists of three sections:

+   - Active Jobs: This section is for the jobs that are currently running
+   - Completed Jobs: This section is for the jobs that successfully completed
+   - Failed Jobs: This section is for the jobs that were failed

It is shown in the following screenshot:

![](bbox=[0.112, 0.419, 0.884, 0.682])

Sections of Spark UI are created lazily and can be visible if required. For example, the Active Jobs section will only be visible if there is a job that is currently running. Similarly, the Failed Jobs and Completed Jobs sections are visible only if there is a job that failed or successfully completed.

The Jobs tab section of Spark UI is rendered using the

```
`org.apache.spark.ui.jobs.JobsTab class that uses`
```

```
`org.apache.spark.ui.jobs.JobProgressListener to get the statistics of the job.`
```