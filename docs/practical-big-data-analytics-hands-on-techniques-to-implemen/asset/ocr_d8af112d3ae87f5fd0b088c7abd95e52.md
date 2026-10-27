# Job scheduling in YARN

It is not uncommon for large Hadoop clusters to have multiple jobs running concurrently. The allocation of resources when there are multiple jobs submitted from multiple departments becomes an important and indeed interesting topic. Which request should receive priority if say, two departments, A and B, submit a job at the same time but each request is for the maximum available resources? In general, Hadoop uses a First-In-First-Out (FIFO) policy. That is, whoever submits the job first gets to use the resources first. But what if A submitted the job first but completing A's job will take five hours whereas B's job will complete in five minutes?

To deal with these nuances and variables in job scheduling, numerous scheduling methods have been implemented. Three of the more commonly used ones are:

+   - **FIFO**: As described above, FIFO scheduling uses a queue to priorities jobs. Jobs are executed in the order in which they are submitted.
+   - **CapacityScheduler**: CapacityScheduler assigns a value on the number of jobs that can be submitted on a per-department basis, where a department can indicate a logical group of users. This is to ensure that each department or group can have access to the Hadoop cluster and be able to utilize a minimum number of resources. The scheduler also allows departments to scale up beyond their assigned capacity up to a maximum value set on a per-department basis if there are unused resources on the server. The model of CapacityScheduler thus provides a guarantee that each department can access the cluster on a deterministic basis.
+   - **Fair Schedulers**: These schedulers attempt to evenly balance the utilization of resources across different apps. While an even balance might not be feasible at a certain given point in time, balancing allocation over time such that the averages are more or less similar can be achieved using Fair Schedulers.

These, and other schedulers, provide finely grained access controls (such as on a per-user or per-group basis) and primarily utilize queues in order to prioritize and allocate resources.

# Other topics in Hadoop

A few other aspects of Hadoop deserve special mention. As we have discussed the most important topics at length, this section provides an overview of some of the other subjects of interest.