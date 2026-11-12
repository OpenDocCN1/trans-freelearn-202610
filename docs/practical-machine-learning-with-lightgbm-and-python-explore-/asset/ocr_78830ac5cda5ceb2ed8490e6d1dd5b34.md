Here is the resulting plot:

![](bbox=[0.17, 0.19, 0.85, 0.53])

Figure 5.4 – A parallel coordinate plot for our study. Each horizontal line is the configuration for a single trial. Darker lines indicate more successful trials (higher F1 scores)

The parallel coordinate plot shows that the best trials all used DART as the boosting type and have a learning rate of just below 0.1 and more than 200 estimators. We can also visually see some parameter interactions: GBDT models correlate with slightly higher learning rates. Far fewer leaf nodes are required when there is a large number of estimators because having many estimators and large numbers of leaf nodes leads to overfitting.

# Multi-objective optimization

In the optimization studies shown previously, we focused on a single optimization objective: maximizing our F1 score. However, in some instances, we would like to optimize two potentially competing objectives. For example, say we want to create the smallest GBDTs possible (fewest leaves) while obtaining a good F1 score. Reducing the number of leaves can potentially negatively impact our performance, so a trade-off exists.