# The queryPlanner section

queryPlanner section is where you see basic information about the query, including how it was parsed into canonical or normalized form, queryHash which is a shortcut way of identifying the same query shapes with different values, planCacheKey of the plan will be present if the query plan was in the cache, indication whether the query plan selection was influenced by any special settings, the winningPlan, and rejected plans in the case when there were additional plan options.

# The executionStats section

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