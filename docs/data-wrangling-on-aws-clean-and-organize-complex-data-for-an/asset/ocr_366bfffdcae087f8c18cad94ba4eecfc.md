# Introducing AWS SDK for pandas

+   - QueryQueueTime: The number of ms that the query was in the query queue waiting for resources. This is a variable time and depends on the availability of resources at the specific Region and time.
+   - ServiceProcessingTime: The number of ms that Athena took to process the query results after the query engine finished running the query. If a query spends too much time in this phase, we rewrite a big dataset to S3 and try to reduce the size of the resulting set.

You can use these metrics to look at where each query spends time and then optimize them. However, note that these metrics don't include any network latencies and delays outside of the Athena query engine.

# How does Athena cache work?

We can enable cache logic by setting the MAX_CACHE_SECONDS variable to be greater than zero. This will help decrease the query execution time (by skipping the execution entirely and reusing the past results if they are still available).

+   1. Check whether the MAX_CACHE_SECONDS variable is greater than zero; if it's set to zero or less, return immediately and the queries are executed every time they are called. This is needed for use cases where data is updated more frequently and you require recent data rather than lower query execution times.
+   2. Retrieve queries based on MAX_REMOTE_CACHE_ENTRIES from the Athena history log using pagination. Check whether the query details are available in the local cache; if not, append the query details to the local cache object. The local cache stores the data about the most recent 100 queries so that the data can be retrieved much faster than the remote cache. In this aws wrangler Athena context, the cache can be understood as the storage for query metadata details, such as the query execution ID and query completion time.
+   3. Sort the list of queries from the local cache and the remote cache based on the condition that the elapsed seconds query completion time is not greater than MAX_CACHE_SECONDS.
+   4. Parse the query so that a successful match can be made with older queries. Currently, the query is stripped of white space and trailing semicolons and leading and trailing brackets are removed so that the queries can be matched to a reasonable degree of confidence.
+   5. If the older query is CTAS (executed with aws wrangler CTAS mode), then parse and take the inner query before it can be compared with a new query. Compare both queries, and if there is a match, return the cache match as a success.

For example, let us execute a simple query that extracts data from an example table, crawled on CloudFront logs and stored in an S3 bucket. You could use any table for this demonstration of Athena cache functionality.