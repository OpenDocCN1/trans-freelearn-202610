# # Spark REST APIs

**Representational state transfer (REST)** architecture is used very often while developing web services. These days, a lot of frameworks such as Hadoop, Apache Storm, and so on, provide RESTful web services that help users to interact with or monitor the framework. Apache Spark, being in the same league, also provides REST web services that can be used to monitor the Spark applications. In this section, we will learn about the REST APIs provided by Apache Spark.

The response type of Spark REST APIs is JSON, which can be used to design custom monitoring over Spark applications. The REST endpoints are mounted at /api/v1, which means that for a SparkContext with UI running https://localhost:4040, the REST endpoints will be mounted at https://localhost:4040/api/v1.

It is time to explore some of the REST APIs provided by Apache Spark. For that, launch Spark shell and run any of the jobs mentioned in Spark REPL also known as CLI section:

Execute the following command to list the applications executed/running in SparkContext:

```
curl http://localhost:4040/api/v1/applications
[
{
"id" : "local-1479401168077",
"name" : "Spark shell",
"attempts" : [ {
"startTime" : "2016-11-17T16:46:05.429GMT",
"endTime" : "1969-12-31T23:59:59.999GMT",
"lastUpdated" , "2016-11-17T16:46:05.429GMT",
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

The REST API provided previously also accepts some request parameters,