![](bbox=[0.045, 0.13, 0.915, 0.56])

Doug Cutting's post at https://issues.apache.org/jira/browse/NUTCH-193 announced his intent to separate Nutch Distributed FS (NDFS) and MapReduce to a new subproject called Hadoop.

# The fundamental premise of Hadoop

The fundamental premise of Hadoop is that instead of attempting to perform a task on a single large machine, the task can be subdivided into smaller segments that can then be delegated to multiple smaller machines. These so-called smaller machines would then perform the task on their own portion of the data. Once the smaller machines have completed their tasks to produce the results on the tasks they were allocated, the individual units of results would then be aggregated to produce the final result.