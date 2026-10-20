We can also see the current Databricks runtime version with the following command:

```
spark.conf.get("spark.databricks.clusterUsageTags.sparkVersion")
```

These properties are required by the Clusters and Jobs APIs to communicate between themselves.

On the cluster details page, the **Notebooks** tab will show all the notebooks attached to the cluster, as well as the status and the last time it was used:

![](bbox=[0.13, 0.28, 0.87, 0.47])

Figure 1.14 – Notebooks attached to a cluster

Attaching a notebook to a cluster is necessary in order to make them work; otherwise, we won't be able to execute the code in it.

Notebooks are detached from a cluster by clicking in the currently attached cluster and selecting **Detach**:

![](bbox=[0.39, 0.6, 0.61, 0.7])

Figure 1.15 – Detaching a notebook from a cluster

This causes the cluster to lose all the values stored as variables in that notebook. It is good practice to always detach the notebooks from the cluster once we have finished working on them. This prevents the autostopping of running clusters, in case there is a process running in the notebook (which could cause undesired costs).