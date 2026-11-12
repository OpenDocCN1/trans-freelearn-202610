
```
CAST(monthlycharges AS REAL),
CAST(totalcharges AS REAL)
)
) AS prediction
FROM pgml.telco_churn;
```

Similar to calling the predict function manually, we use the ROW expression to pass data to the pgml.predict function but select the data from the table.

This also clearly illustrates the advantage of using PostgresML: a consumer of the ML model could query new customer data alongside the predictions with minimal overhead and in the same business transaction with a single network call.

## Summary

This chapter provided an overview of PostgresML, a unique MLOps platform that allows training and calling models from SQL queries on top of an existing PostgreSQL database.

We discussed the platform's advantages in simplifying an ML-enabled landscape and reducing overhead and network latency in a service-oriented architecture. An overview of the core features and the API was provided.

This chapter concluded with a practical example of leveraging PostgresML for a classification problem, illustrating how to train a LightGBM model, perform hyperparameter optimization, deploy it, and leverage it for predictions in a handful of SQL queries.

In the next chapter, we will look at distributed and GPU-based learning with LightGBM.

## References

- [1] PostgresML is 8-40x faster than Python HTTP microservices, [Online]. Available at https://postgresml.org/blog/postgresml-is-8x-faster-than-python-http-microservices.

## Distributed and GPU-Based Learning with LightGBM

This chapter looks at training LightGBM models on distributed computing clusters and GPUs. Distributed computing can significantly speed up training workloads and enable the training of much larger datasets than the memory available on a single machine. We'll look at leveraging Dask for distributed computing and LightGBM's support for GPU-based training.

The topics covered in the chapter are as follows:

- Distributed learning with LightGBM and Dask
- GPU training for LightGBM

## Technical requirements

The chapter includes examples of training and running LightGBM models on distributed computing clusters and GPUs. A Dask environment and GPUs are required to run the examples. Complete code examples are available at https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-11.

## Distributed learning with LightGBM and Dask

Dask is an open-source Python library for distributed computing. It's designed to integrate seamlessly with existing Python libraries and tools, including scikit-learn and LightGBM. This section looks at running distributed training workloads for LightGBM using Dask.

Dask (https://www.dask.org/) allows you to set up clusters on both a single machine and across many machines. Running Dask on a single machine is the default and requires no setup. However, workloads that run on a single-machine cluster (or scheduler) can readily be run with a distributed scheduler.

Dask offers many ways to run a distributed cluster, including integrating Kubernetes, MPI, or automatic provisioning into a hyperscaler such as AWS or Google Cloud Platform.

When running on a single machine, Dask still distributes the workload across multiple threads, which can significantly speed up workloads.

Dask provides cluster management utility classes to set up a cluster easily. A local cluster can be run as follows:

```
cluster = LocalCluster(n_workers=4, threads_per_worker=2)
client = Client(cluster)
```

The preceding code creates a local cluster with four workers, configuring each worker to run two threads. The cluster runs on localhost, with the scheduler running on port 8786 by default. The host IP and port can be configured using parameters. In addition to running the scheduler, Dask also starts a diagnostic dashboard that's implemented using Bokeh (https://docs.bokeh.org/en/latest/). By default, the dashboard runs on port 8787. We can check the **Workers** page to see the status of our running cluster, as shown in Figure 11.1.

![](img/31b70e4a77c3afbe11032f68ab874b02_230_0.png)

Figure 11.1 – Dask diagnostics dashboard showing four running workers with some technical statistics for each

With a cluster up and running, we can prepare our data for use on the distributed cluster.

Dask offers its own implementation of a data frame called the Dask DataFrame. A Dask DataFrame comprises many smaller pandas DataFrames, which are split based on the index. Each part can be stored on disk or distributed across a network, which allows working with much larger datasets than can fit into a single machine's memory. Operations performed on the Dask DataFrame are automatically distributed to the pandas DataFrames.

> **Note**
> When your dataset fits into RAM, using standard pandas DataFrames instead of a Dask DataFrame is recommended.

We can create a Dask DataFrame by loading a CSV file. Note that the CSV file may be located on S3 or HDFS and can be very large. The following code creates a Dask DataFrame from a CSV file:

```python
import dask.dataframe as dd
df = dd.read_csv("covtype/covtype.csv", blocksize="64MB")
```

Here, we also specify the block size for loading the CSV file. The block size sets the chunks the dataset is divided into and gives us granular control over the memory of individual DataFrame parts. When calling df.shape, we get an interesting result:

```python
df.shape
# (Delayed('int-a0031dlf-945d-42b4-af29-ea5e40148f3f'), 55)
```

The number of columns is returned as a number. However, looking at the number of rows, we got a wrapper class called Delayed. This illustrates that even though we've created the DataFrame, the data is not loaded into memory. Instead, Dask loads the data as needed on the workers that use the data. We can force Dask to compute the row count as follows:

```python
df.shape[0].compute()
# 581012
```

With the data available in a DataFrame, we can prepare it for training. We split our data into a training and test set using the train_test_split function from dask_ml:

```python
X = df.iloc[:, :-1]
y = df.iloc[:, -1]
X_train, X_test, y_train, y_test = dask_ml.model_selection.train_test_split(X, y)
```

Although the function from dask_ml mirrors the functionality of scikit-learn's train_test_split, the Dask version maintains the distributed nature of the underlying Dask DataFrame.

Our cluster is now set up, and the data is prepared for training. We can now look toward training our LightGBM model.

The LightGBM library team offers and maintains Dask versions of each available learning algorithm: DaskLGBMRegressor, DaskLGBMClassifier, and DaskLGBMRanker. These are wrappers around the standard LightGBM scikit-learn interface with additional functionality to specify the Dask cluster client to use.

When LightGBM runs on a Dask cluster, training occurs with one LightGBM worker per Dask worker. LightGBM concatenates all data partitions on a single worker into a single dataset, and each LightGBM worker uses the local dataset independently.

Each LightGBM worker then works in concert to train a single LightGBM model, using the Dask cluster to communicate. When data parallel training is performed (as is the case with Dask), LightGBM uses a **Reduce-Scatter** strategy:

- 1. During the histogram-building phase, each worker builds histograms for different non-overlapping features. Then, a **Reduce-Scatter** operation is performed: each worker shares a part of its histogram with each other worker.
- 2. After the **Reduce-Scatter**, each worker has a complete histogram for a subset of features and then finds the best split for these features.
- 3. Finally, a gathering operation is performed: each worker shares its best split with all other workers, so all workers have all the best splits.

The best feature split is chosen, and the data is partitioned accordingly.

Fortunately, the complexity of the distributed algorithm is hidden from us, and the training code is identical to the scikit-learn training code we're used to:

```
dask_model = lgb.DaskLGBMClassifier(n_estimators=200, client=client)
dask_model.fit(X_train, y_train)
```

Running the preceding code trains the LightGBM model, and we can see the progress by checking the **Status** page of the Dask dashboard, as shown in Figure 11.2.

![](img/31b70e4a77c3afbe11032f68ab874b02_232_0.png)

Figure 11.2 – Status page of the Dask dashboard showing the task stream while a LightGBM model is training
