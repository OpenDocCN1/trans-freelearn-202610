benchmark the model results. Validation data is also data that has never been seen before, which is incorporated after the model has passed validation against the test dataset and prior to deployment. Validation datasets are often brought from a completely different source from the original training/test data source, the goal being to provide a second corroboration, or refutation, of the model results.

Along with training, test, and validation data, you will also hear some other terms which are related:

+   - In-sample data refers to the data which was used to fit (model) the data and is used on the training dataset
+   - Out-of-sample data refers to data not used to fit the data

This means that out-of-sample data is typically used only on the test and validation datasets. Another technique to validate the data is known as **k-fold cross-validation**. This technique will use multiple folds to define different subsets of in-sample and out-of-sample datasets in order to perform multiple iterations of model testing in order to randomize the selection of training and test data.

# Time series validation

Of course time series models also use this concept; however, time periods alone can be used to partition test and training data, for example, a model might be built on the previous five years of historical data (training) and then forecast on the current sixth year of data (testing or validation data).

Variations of this are common. A *walk forward* testing modeler will develop a time-series model on years 1-5, test the results on year 6, and then develop a similar model on years 2-6, and then test the results on year 7. This procedure would be repeated until there is no more data to test. In all these cases, the model is built on the *in-sample* data, and tested on out-of-sample data which is similar to k-fold validation.

# Benchmark against best champion model