Optuna supports solving this type of problem by using **multi-objective optimization (MOO)**. When optimizing multiple objectives, we return two evaluations from our `objective` function and specify the optimization directions.

As an example, consider the trade-off between the learning rate and performance. We want to train our model as fast as possible, which requires a high learning rate. However, we know the best performance is achieved using a small learning rate and many iterations.

We can use Optuna to optimize this trade-off. We define a new `objective` function, fixing all other parameters to the optimal values found earlier. We return two evaluations: the learning and the cross-validated F1-score. We want to maximize both values:

```
def moo_objective(trial):
    learning_rate = trial.suggest_float("learning_rate", 0.0001, 0.5,
    log=True),

    model = lgb.LGBMClassifier(
        force_row_wise=True,
        boosting_type='gbdt',
        n_estimators=200,
        num_leaves=6,
        bagging_freq=7,
        learning_rate=learning_rate,
        max_bin=320,
    )

    scores = cross_val_score(model, X, y, scoring="f1_macro")
    return learning_rate[0], scores.mean()
```

When calling optimize, we then set the direction for the optimization of both evaluations:

```
study = optuna.create_study(directions=["maximize", "maximize"])
study.optimize(moo_objective, n_trials=100)
```

When performing MOO, there isn't always a single best result: a trade-off often exists between the objectives. Therefore, we want to visualize the study results to explore the trade-off and select parameter values that perform well with both objectives. This type of visualization is called a **Pareto front** and can be created as follows: