+   - Boosting: This involves iteratively training models on the errors of previous models. The final prediction is made by combining the prediction of all models in the chain.
+   - Stacking: This involves methods where multiple base models are trained, then a higher-order model (known as a meta-model) is trained to learn from the base model predictions and make the final prediction.
+   - Blending: This is very similar to stacking. However, the meta-model is trained on predictions made by the base models on a hold-out set (a part of the training data the base learners were not trained on) instead of the whole training set.

The purpose of ensemble learning methods is to improve our prediction performance, and there are several ways ensembles improve the performance over individual models, as outlined here:

+   - Improved accuracy: By combining predictions, we increase the likelihood that the final prediction is accurate as, in aggregate, models make fewer mistakes.
+   - Improved generalization and overfitting: By aggregating predictions, we reduce the variance in the final prediction, improving generalization. Further, in some ensemble methods, the models cannot access all data (bagging ensembles), reducing noise and overfitting.
+   - Improved prediction stability: The aggregation of predictions reduces the random fluctuations of individual predictions. Ensembles are less sensitive to outliers, and outlying predictions of member models have a limited impact on the final prediction.

Decision trees are well suited for ensemble learning, and decision tree-specific algorithms exist for ensemble learning. The following section discusses bagging ensembles in decision trees focusing on random forests.

# # Bagging and random forests

Bagging is an ensemble method where multiple models are trained on subsets of the training data. The models’ predictions are combined to make a final prediction, usually by taking the average for numerical prediction (for regression) or the majority vote for a class (for classification). When training each model, we select a subset of data from the original training dataset with replacement—that is, a specific training pattern can be a member of multiple subsets. Since each model is only presented with a sample of the training data, no single model can “memorize” the training data, which reduces overfitting. The following diagram illustrates the bagging process: