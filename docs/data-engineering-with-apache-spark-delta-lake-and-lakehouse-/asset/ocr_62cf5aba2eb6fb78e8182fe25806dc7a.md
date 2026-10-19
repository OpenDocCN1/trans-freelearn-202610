# The deployment strategy

We have worked extremely hard to get to the point where we have three functional pipelines, as follows:

+   - **The Electroniz ingestion pipeline:** `electroniz_batch_ingestion_pipeline`
+   - **The Electroniz curation pipeline:** `electroniz_curation_pipeline`
+   - **The Electroniz aggregation pipeline:** `electroniz_aggregation_pipeline`

Just as a recap, in the last few chapters, we followed multiple steps in order to create these pipelines. After their creation, we invoked each one manually to unit test their functionality. Finally, we validated the data that each one produced to make sure it matched the expectation of the **Electroniz** use cases. That's a lot of work, so we should be proud to have reached this far.

Assuming we are happy with the outcomes of the unit tests performed on the preceding pipelines, it is time to start thinking about the best way to deploy these pipelines in production. As per best practices, the three pipelines should run as one complete unit. Each pipeline run, which is also referred to as a job, should work by invoking the ingestion pipeline to pull incremental data from various sources. This is followed by invoking the curation pipeline, which cleanses the data and merges it in **Delta Lake** tables. Finally, the aggregation pipeline summarizes data for use by end users.

Typically, pipelines are deployed in two ways, as described next.

# The time-based method

In the **time-based method**, each pipeline is deployed separately and invoked according to a time-based approach. For example, you can invoke the ingestion pipeline at 12:00 a.m., invoke the curation pipeline at 12:20 a.m., and invoke the aggregation pipeline at 12:30 a.m. The following is a list of some of the pros and cons of this method:

+   - **The overall deployment is simple:** You simply need to schedule each pipeline at a designated time.
+   - **It requires time precision:** The invocation time of each pipeline needs to be carefully calculated; otherwise, they might overlap each other. You do not want the second pipeline to start before the first one has finished.