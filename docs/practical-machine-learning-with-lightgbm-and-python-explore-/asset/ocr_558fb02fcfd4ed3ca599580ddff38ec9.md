Each LightGBM worker then works in concert to train a single LightGBM model, using the Dask cluster to communicate. When data parallel training is performed (as is the case with Dask), LightGBM uses a **Reduce-Scatter** strategy:

+   1. During the histogram-building phase, each worker builds histograms for different non-overlapping features. Then, a **Reduce-Scatter** operation is performed: each worker shares a part of its histogram with each other worker.

+   2. After the **Reduce-Scatter**, each worker has a complete histogram for a subset of features and then finds the best split for these features.

+   3. Finally, a gathering operation is performed: each worker shares its best split with all other workers, so all workers have all the best splits.

The best feature split is chosen, and the data is partitioned accordingly.

Fortunately, the complexity of the distributed algorithm is hidden from us, and the training code is identical to the scikit-learn training code we’re used to:

```
dask_model = lgb.DaskLGBMClassifier(n_estimators=200, client=client)
dask_model.fit(X_train, y_train)
```

Running the preceding code trains the LightGBM model, and we can see the progress by checking the **Status** page of the Dask dashboard, as shown in **Figure 11.2**.

![](bbox=[0.19, 0.5, 0.8, 0.87])

Figure 11.2 – Status page of the Dask dashboard showing the task stream while a LightGBM model is training