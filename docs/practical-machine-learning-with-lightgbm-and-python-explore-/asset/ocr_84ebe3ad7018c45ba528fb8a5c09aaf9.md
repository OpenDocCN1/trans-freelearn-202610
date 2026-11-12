The reset_parameter callback takes a function as input. The function receives the current iteration and returns the parameter value. Learning rate decay is a technique where we decrease the value of the learning rate over time. Learning rate decay improved the overall accuracy achieved. Ideally, we want the initial trees to have a more significant impact on correcting the prediction errors. In contrast, later on, we want to reduce the impact of additional trees and have them make minor adjustments to the errors. We implement a slight exponential decay that reduces the learning rate from 0.09 to 0.078 throughout training.

Now, we are ready for training. We use lgb.train to train the model:

```
```python
gbm = lgb.train(params, training_set, num_boost_round=150, valid_sets=test_set, callbacks=callbacks)
```
```

We use 150 boosting rounds (or boosted trees). In conjunction with a lower learning rate, having many boosting rounds should improve accuracy.

After training, we can use lgb.predict to get predictions for our test set and calculate the F1 score:

```
```python
y_pred = np.argmax(gbm.predict(X_test, num_iteration=gbm.best_iteration), axis=1)
f1_score(y_test - 1, y_pred, average="macro")
```
```

The LightGBM predict function outputs an array of activations, one for each class. Therefore, we use np.argmax to choose the class with the highest activation as the predicted class. LightGBM also has support for some plotting functions. For instance, we can use plot_metric to plot our AUC_mu results as captured in the metrics:

```
```python
lgb.plot_metric(metrics, 'auc_mu')
```
```

The results of this are shown in Figure 3.3.

![](bbox=[0.3, 0.625, 0.71, 0.865])

Figure 3.3 – A plot of the AUC μ metric per training iteration created using lgb.plot_metric