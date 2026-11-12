Dask offers many ways to run a distributed cluster, including integrating Kubernetes, MPI, or automatic provisioning into a hyperscaler such as AWS or Google Cloud Platform.

When running on a single machine, Dask still distributes the workload across multiple threads, which can significantly speed up workloads.

Dask provides cluster management utility classes to set up a cluster easily. A local cluster can be run as follows:

```
```
cluster = LocalCluster(n_workers=4, threads_per_worker=2)
client = Client(cluster)
```
```

The preceding code creates a local cluster with four workers, configuring each worker to run two threads. The cluster runs on localhost, with the scheduler running on port 8786 by default. The host IP and port can be configured using parameters. In addition to running the scheduler, Dask also starts a diagnostic dashboard that's implemented using Bokeh (https://docs.bokeh.org/en/latest/). By default, the dashboard runs on port 8787. We can check the **Workers** page to see the status of our running cluster, as shown in Figure 11.1.

![](bbox=[0.13, 0.42, 0.87, 0.66])

Figure 11.1 – Dask diagnostics dashboard showing four running workers with some technical statistics for each

With a cluster up and running, we can prepare our data for use on the distributed cluster.

Dask offers its own implementation of a data frame called the Dask DataFrame. A Dask DataFrame comprises many smaller pandas DataFrames, which are split based on the index. Each part can be stored on disk or distributed across a network, which allows working with much larger datasets than can fit into a single machine's memory. Operations performed on the Dask DataFrame are automatically distributed to the pandas DataFrames.