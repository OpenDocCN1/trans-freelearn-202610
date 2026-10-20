# Deleting a cluster

Deleting a cluster terminates the cluster and removes its configuration. Use this carefully because this action cannot be undone.

To delete a cluster, click the delete icon in the cluster actions on the Job Clusters or All-Purpose Clusters tab:

![](bbox=[0.13, 0.2, 0.87, 0.25])

Figure 1.36 – Deleting a cluster from the Job Clusters tab

You can also invoke the permanent delete API endpoint to programmatically delete a cluster.

# Cluster information

Detailed information on Spark jobs is displayed in the Spark UI, which can be accessed from the cluster list or the cluster details page. The Spark UI displays the cluster history for both active and terminated clusters:

![](bbox=[0.13, 0.43, 0.87, 0.68])

Figure 1.37 – Cluster information

Cluster information allows us to have an insight into the progress of our process and identify any possible bottlenecks that could point us to possible optimization opportunities.