In our example, we use a SQLite database as an RDB:

```
```
study_name = "lgbm-tpe-rdb-study"
storage_name = f"sqlite:///{study_name}.db"
study = optuna.create_study(
    study_name=study_name,
    storage=storage_name,
    load_if_exists=False,
    sampler=sampler,
    pruner=pruner)
```
```

Optuna manages the connection to the RDB and the persistence of the results. After setting up the connection, optimization can proceed as usual.

Restoring the study from an RDB backend is straightforward; we specify the same `storage` and set `load_if_exists` to True:

```
```
study = optuna.create_study(study_name=study_name, storage=storage_name, load_if_exists=True)
```
```

# **Understanding parameter effects**

In many cases, it's also valuable to better understand the effects of hyperparameters when solving a specific problem. For example, the `n_estimators` parameter directly affects the computational complexity of a model. If we know the parameter to be less important, we can choose smaller values to improve our model's runtime performance. Optuna provides several visualizations to dive deeper into the results of a study and gain insight into hyperparameters.

A straightforward visualization plots the **importance of each parameter**: how much each affected the training outcome. We can create an importance plot as follows:

```
```
fig = optuna.visualization.plot_param_importances(study)
fig.show()
```
```

The importance plot for our study is shown here: