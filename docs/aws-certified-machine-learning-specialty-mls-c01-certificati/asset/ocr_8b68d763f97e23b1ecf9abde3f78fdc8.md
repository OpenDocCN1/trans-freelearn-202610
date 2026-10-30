9. You are using grid search to tune a machine learning model. During the tuning process, you obtain good performance metrics. However, when you execute the model in production, the model performance is not acceptable. You have to troubleshoot the problem. Which of the following options are valid reasons for this issue? (Select all correct answers.)

+   a) You are tuning and evaluating the model in the training data, which is causing overfitting.
+   b) The production data does not have the same distribution as the training data.
+   c) You are not using cross-validation in the training process.
+   d) You are not tuning the right hyperparameters.

Answer
You can’t tune and test the model in the same dataset at the risk of overfitting it. That’s why option "a" is correct. If the production data does not follow the same distribution of the training data, the model will not work, so option "b" is also correct. Option "c" is not valid because cross-validation is not mandatory for model evaluation. Option "d" would be correct if you find bad results in the training data, not in the production data.

10. You are working for a global financial company. Your team has created a binary classification model to identify fraudulent transactions. The model has been put into production and is automatically flagging fraudulent transactions and sending them for further screening. The operation team is complaining that this model is blocking too many transactions and they would prefer to flag a smaller number of transactions. According to the preceding scenario, what is the expectation of the operation team?

+   a) They want to calibrate the model threshold at 0.5.
+   b) They want to prioritize precision over recall.
+   b) They want to prioritize recall over precision.
+   b) They want to use F-measure.