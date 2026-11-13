for example status=running, which provides the list of applications that have requested status:

```
curl http://localhost:4040/api/v1/applications?status=running
[
  {
    "id" : "local-1479401168077",
    "name" : "Spark shell",
    "attempts" : [ {
      "startTime" : "2016-11-17T16:46:05.429GMT",
      "endTime" : "1969-12-31T23:59:59.999GMT",
      "lastUpdated" : "2016-11-17T16:46:05.429GMT",
      "duration" : 0,
      "sparkUser" : "",
      "completed" : false,
      "startTimeEpoch" : 1479401165429,
      "endTimeEpoch" : -1,
      "lastUpdatedEpoch" : 1479401165429
    } ]
  }
]
```

Execute the following command to list the jobs executed by the application (REST URL: http://localhost:4040/api/v1/applications/[app-id]/jobs):

```
curl http://localhost:4040/api/v1/applications/local-1479401168077/jobs
[
  {
    "jobId" : 1,
    "name" : "count at <console>:28",
    "submissionTime" : "2016-11-17T16:47:33.799GMT",
    "completionTime" : "2016-11-17T16:47:33.827GMT",
    "stageIds" : [ "1" ],
    "status" : "SUCCEEDED",
    "numTasks" : 2,
    "numActiveTasks" : 0,
    "numCompletedTasks" : 2,
    "numSkippedTasks" : 0,
    "numFailedTasks" : 0,
    "numActiveStages" : 0,
    "numCompletedStages" : 1,
    "numSkippedStages" : 0,
    "numFailedStages" : 0
  }, {
    "jobId" : 0,
    "name" : "collect at <console>:29",
    "submissionTime" : "2016-11-17T16:47:26.329GMT",
    "completionTime" : "2016-11-17T16:47:26.754GMT",
    "stageIds" : [ "0" ],
    "status" : "SUCCEEDED",
    "numTasks" : 2,
    "numActiveTasks" : 0,
    "numCompletedTasks" : 2,
    "numSkippedTasks" : 0,
    "numFailedTasks" : 0,
    "numActiveStages" : 0,
    "numCompletedStages" : 1,
    "numSkippedStages" : 0,
    "numFailedStages" : 0
  } ]
```