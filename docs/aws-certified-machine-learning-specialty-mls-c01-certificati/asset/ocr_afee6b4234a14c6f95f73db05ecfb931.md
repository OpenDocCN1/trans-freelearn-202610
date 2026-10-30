Let's elaborate a little more on the third item on that list, since this is covered in the AWS Machine Learning Specialty exam. Let's assume we are creating a binary classification model, using cross-validation during training and using a testing set to extract final metrics (hold-out validation). If we get 80% accuracy in the cross-validation results and 50% accuracy in the testing set, it means that the model was overfitted to the train set, and cannot be generalized to the test set.

On the other hand, if we get 50% accuracy in the training set and 80% accuracy in the test set, there is a systemic issue in the data. It is very likely that the training and testing sets do not follow the same distribution.

**Important note**
Accuracy is a model evaluation metric commonly used on classification models. It measures how often the model made a correct decision during its inference process. We have selected this metric just for the sake of example, but be aware that there are many other evaluation metrics applicable for each type of model (which will be covered at the appropriate time).

# Bootstrapping methods

Cross-validation is a good strategy to validate ML models, and you should try it in your daily activities as a data scientist. However, you should also know about other resampling techniques available out there. **Bootstrapping** is one of them.

While cross-validation works **with no replacement**, a bootstrapping approach works **with replacement**. With replacement means that, while you are drawing multiple random samples from a population dataset, the same observation might be duplicated across samples.

Usually, bootstrapping is not used to validate models as we do in the traditional cross-validation approach. The reason is simple: since it works with replacement, the same observation used for training could potentially be used for testing, too. This would result in inflated model performance metrics, since the estimator is likely to be correct when predicting an observation that was already seen in the training set.

Bootstrapping is often used by ML algorithms in an embedded way that requires resampling capabilities to process the data. In this context, bootstrapping is not being used to *validate* the model, but to *create* the model. **Random forest**, which will be covered in the algorithms chapter, is one of those algorithms that uses bootstrapping internally for model building.