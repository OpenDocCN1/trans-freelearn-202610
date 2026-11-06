
## The queryPlanner section

queryPlanner section is where you see basic information about the query, including how it was parsed into canonical or normalized form, queryHash which is a shortcut way of identifying the same query shapes with different values, planCacheKey of the plan will be present if the query plan was in the cache, indication whether the query plan selection was influenced by any special settings, the winningPlan, and rejected plans in the case when there were additional plan options.

## The executionStats section

The explain command that actually ran the query will include the executionStats section, where all the actual observed metrics during the run are captured. For the query filter portion, it includes fields such as totalKeysExamined (number of index keys looked at), totalDocsExamined (number of documents looked at), nReturned (number of documents that would be returned to the client if explain was not specified), and the same details for each execution stage that was observed.

For in-memory sorts, the explain command includes a SORT section with information about how much memory was used. This section appears when there’s no eligible index to support getting data in sorted order. It also indicates whether the data had to be spilled to disk and, if so, how much disk space was used to aid the sort.

In the case of aggregations, explain also lists every stage after the initial $cursor stage (which is responsible for fetching documents from the collection to stream into the rest of the pipeline). It includes the number of documents that were returned from each stage, as well as the cumulative time taken by all the stages up to this one.

Here is a portion of sample output for an aggregation execution stats section listing stages:

```
stages: [
  {
    '$cursor': { queryPlanner: { executionStats: { ... } } }
    nReturned: Long('10805'),
    executionTimeMillisEstimate: Long('21')
  },
  {
    '$set': { ... },
    nReturned: Long('10805'),
    executionTimeMillisEstimate: Long('21')
  },
]
```

```
{
  '$unwind': { ... },
  nReturned: Long('14804'),
  executionTimeMillisEstimate: Long('22')
},
{
  '$group': { ... },
  maxAccumulatorMemoryUsageBytes: { count: Long('1768') },
  totalOutputDataSizeBytes: Long('3534'),
  usedDisk: false,
  nReturned: Long('13'),
  executionTimeMillisEstimate: Long('23')
},
{
  '$sort': { sortKey: { count: -1 } },
  totalDataSizeSortedBytesEstimate: Long('3638'),
  usedDisk: false,
  spills: Long('0'),
  nReturned: Long('13'),
  executionTimeMillisEstimate: Long('23')
}
```

The first stage is called $cursor, and it indicates that the source of the documents for the pipeline is a collection. Early $match stages and other stages that are executed the same way as the find command can be observed inside the $cursor stage. This stage returned a certain number of documents, which then flowed through the pipeline. The execution time estimate is cumulative, so we see that the query portion took 21 milliseconds, with most other stages taking less than 1 millisecond, except for $unwind and $group, which took 1 millisecond each.

In sharded collections, the mongos router forwards a query with the explain option to each shard, merges their results, and includes its own work in the explain output, which has separate sections for shards’ work and mongos work.

Here is an example of the explain output fields for an aggregation query in a sharded cluster:

```
"mergeType" : "mongos",
"splitPipeline" : {
"shardsPart" : [ ... ],
"mergerPart" : [ ... ]
```
```

This shows that the merging of outputs from each shard takes place on mongos. It also shows how the overall pipeline is split into two parts. The first part is sent to relevant shards, while the merging part is executed on mongos.

Here is an example of explain on a shard that needs to filter out orphan documents:

```
winningPlan: {
  stage: 'SHARD_MERGE',
  shards: [
    {
      shardName: 'shard01',
      winningPlan: {
        stage: 'FETCH',
        inputStage: {
          stage: 'SHARDING_FILTER',
          inputStage: {
            stage: 'IXSCAN',
            keyPattern: { id: 1 },
            ...
          }
        }
      }
    }
  ]
}
```

This example shows a query on a particular field that efficiently uses an index for the query. However, since the shard key is not in the index, an additional step is needed to fetch the document to check whether its shard key value belongs to this particular shard. This query can be made more efficient by adding a compound index that includes shard key fields (usually as trailing fields) in the index.

### SHARD_MERGE versus SINGLE_SHARD stages

SHARD_MERGE indicates that the query is being handled by multiple shards. If the query can target a single shard, the name of the stage will be SINGLE_SHARD.

## Analyzing log messages

As useful as the explain output is, there is a lot of information available in the logs from each actual execution of a command that you cannot see in explain. Here is an example formatted for readability and trimmed of fields not specific to query filter execution:

```
"type" : "command",
"ns" : "test.coll",
"collectionType" : "normal",
```
```json
"command" : {
    "aggregate" : "coll",
    "pipeline" : [
        {"$match" : {"$or" : [
            {"x" : {"$gt" : 5,"$lt" : 10}},
            {"a" : {"$gt" : 5,"$lt" : 10}}
        ]}},
        {"$sortByCount" : "$b"}
    ],
    "$clusterTime" : { ... },
    "$db" : "test"
},
"planSummary" : "IXSCAN { x: 1 }, IXSCAN { a: 1 }",
"planningTimeMicros" : 2572,
"keysExamined" : 567,
"docsExamined" : 567,
"hasSortStage" : true,
"nreturned" : 40,
"planCacheKey" : "DAC1B215",
"queryFramework" : "sbe",
"locks" : {"Global" : {"acquireCount" : {"r" : 4}}},
"storage" : { },
"workingMillis" : 12,
"durationMillis" : 12
}
```

You can see which query plan was used (two indexes, one for each $or clause) and that there was a SORT stage (hasSortStage: true), indicating there was an in-memory sort of results. You can also see how selective the indexes were by the number of keys versus documents examined, and you can see how long the operation took, along with a breakdown of where the time was spent. If the multiple-plan evaluation process were run, then you would also see "fromMultiPlanner": true in the log message.

Messages in the logs are in json format, so if you want to analyze them programmatically, you can load them into their own MongoDB collection, or you can use grep and other command-line tools to look for problematic patterns.

## Identifying problematic patterns

Just as not all fast queries are optimally efficient, sometimes well-written, index-supported queries can still be slow. It’s important to understand why they are slow to avoid wasting time tuning something that’s not the root cause of the performance issue.

Will problematic queries always be slow? Not necessarily. We already know that slow is relative, and by default, queries that are slower than 100 milliseconds are recorded in the mongod logs. This threshold can be configured to be higher, lower, or based on something other than an absolute time threshold; see Chapter 14, Monitoring and Observability, for more details. However, inefficient queries may still slip under the radar if they fall just below the threshold, even though they could still be significantly faster with optimal indexes. We can look for these slow queries by adjusting the logging threshold and looking at the query targeting ratio, disk use, or other resource contention.

## Query targeting ratio

One of the most important metrics for how efficient your queries are is called the query targeting ratio. Now that we’ve seen what details are available in explain and log messages, let’s look at what goes into this ratio. When you query for all documents that satisfy a particular criterion, the fewer documents the system has to look at to determine which ones to return, the faster the query will complete.

Query targeting ratio refers to either the number of index keys read compared to the number of documents returned or the number of documents read compared to the number of documents returned. The ideal number for either of these would be 1, as we’d like to only look at index entries or only documents that need to be returned to the client. If there is an optimal index for a query and each index entry scanned ends up being in the result set, then the ratio will be 1:1. Covered index queries are a special case where we don’t even need to look at any documents and will be indicated by having "docsExamined": 0, but let’s put those aside for the purpose of understanding the following examples.

The simplest example is query {a: 5} and an index on a. Every index entry we look at will be for the value we want to match, and we will only visit documents that need to be returned to the client. That’s an ideal ratio. If there is no index on a, then we have to do a collection scan, meaning we look at every document in the collection. If there are 50 documents that match our condition and need to be returned, and there are 50 million documents in the collection, then our targeting ratio will be 1 million to one, which is not very good, given the ideal ratio would be one to one.
