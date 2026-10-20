
2. In this example, we will be using the California housing dataset, which is included as an example dataset in scikit-learn. This dataset holds data from the 1990 US census with house values for 20,000 houses in California, along with some features with data about income, number of inhabitants, rooms, and so on. To fetch this dataset, we will use the fetch_california_housing() scikit-learn function and dump the results into variables named features and target:

```
features, target = fetch_california_housing(return_X_y=True)
```

3. Now that we have split our dataset into the features and the target, we will scale the prediction variables, which is a common practice in machine learning and deep learning. This way, we will ensure we have scaling consistency. This will help us overcome several problems that arise when our features have not been discretized into scaled variables. The features that are available in our dataset are as follows:

+   a) Median house income per block
+   b) House age
+   c) Average number of rooms in a house per block
+   d) Average number of bedrooms
+   e) Total block population
+   f) Average number of house occupants
+   g) Latitude
+   h) Longitude

4. By reviewing the scale of the features in each column – a simple operation that can be done in a pandas DataFrame using the describe() method – we can see that the orders of magnitude in the mean values vary a lot:

```
import pandas as pd
```

```
features_df = pd.DataFrame(features)
```

```
features_df.describe()
```

5. As we can see, the ranges and scales of these variables can be very different:

```
import pandas as pd

features_df = pd.DataFrame(features)
features_df.describe()
```

Out[5]:

|  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|---|
| count | 20640.000000 | 20640.000000 | 20640.000000 | 20640.000000 | 20640.000000 | 20640.000000 | 20640.000000 | 20640.000000 |
| mean | 3.870671 | 28.639486 | 5.429000 | 1.096675 | 1425.476744 | 3.070655 | 35.631861 | -119.569704 |
| std | 1.899822 | 12.585558 | 2.474173 | 0.473911 | 1132.462122 | 10.386050 | 2.135952 | 2.003532 |
| min | 0.499900 | 1.000000 | 0.846154 | 0.333333 | 3.000000 | 0.692308 | 32.540000 | -124.350000 |
| 25% | 2.563400 | 18.000000 | 4.440716 | 1.006079 | 787.000000 | 2.429741 | 33.930000 | -121.800000 |
| 50% | 3.534800 | 29.000000 | 5.229129 | 1.048780 | 1166.000000 | 2.818116 | 34.260000 | -118.490000 |
| 75% | 4.743250 | 37.000000 | 6.052381 | 1.099526 | 1725.000000 | 3.282261 | 37.710000 | -118.010000 |
| max | 15.001100 | 52.000000 | 141.909091 | 34.066667 | 35682.000000 | 1243.333333 | 41.950000 | -114.310000 |

Figure 10.3 - The statistical summary of the features

6. The population of a block is measured in thousands, but the average number of bedrooms is centered around 1.1. To prevent situations in which features with large values are deemed to be more important, scaling the features and normalizing them is a standard practice. To normalize the features so that they're the same scale, we can use the scikit-learn StandardScaler function:

```
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
scaled_features = scaler.fit_transform(features)
```

7. Now that we have scaled the predictor features, we can check that the mean is close to 0 by calling the mean(axis=0) NumPy method on the scaled_features dataset:

```
scaled_features.mean(axis=0)
```

8. Now that our predictor features have been normalized and scaled, we will have to convert the numeric target column into discrete values:

```
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
scaled_features = scaler.fit_transform(features)
print(scaled_features.mean(axis=0))
```

[ 6.60969987e-17  5.50808322e-18  6.60969987e-17 -1.06030602e-16
 -1.10161664e-17  3.44255201e-18 -1.07958431e-15 -8.52651283e-15]

Figure 10.4 – The scaled feature means

9. This target column vector is the value of each house and is a continuous positive scalar. We will transform this into a target column vector with discrete values that are 1 when the house value is greater than the mean and 0 where it is not. This way, the prediction can be framed as a question; for example, given the predictor features, is the values of this house greater than the mean? To discretize the values, we will compare this with the mean and use the numpy.where() function to convert the Booleans values that the comparison yields into 1s and 0s:

```
target_discrete = np.where(target < np.median(target), 0, 1)
```

This way, we have encoded our target variable into a column vector with two levels, either 1 or 0. This indicates whether the housing price is above the mean.

10. Now that we have prepared our dataset, we can start setting the stage for optimizing the hyperparameters of several scikit-learn models that we will use to train an effective classification model. To this end, we will use the following three scikit-learn classifier models:

+   a) Support Vector Machines
+   b) Logistic Regression
+   c) Random Forest

11. We will create an objective function to be minimized that will be passed to Hyperopt. This will run the training and calculate the cross-validation performance metric over several types of models, defined as 'type' parameters. One thing to bear in mind about this function is that because we are using the fmin() Hyperopt function, this will try to minimize the objective function. So, to improve the performance metric – which is accuracy, in this case – we must use the negative accuracy. Otherwise, Hyperopt will try to minimize the accuracy, which is something that we obviously don't want to do:

```
def objective(params):
    classifier_type = params['type']
    del params['type']
    if classifier_type == 'svm':
        clf = SVC(**params)
    elif classifier_type == 'rf':
        clf = RandomForestClassifier(**params)
    elif classifier_type == 'logreg':
        clf = LogisticRegression(**params)
    else:
        return 0
    accuracy = cross_val_score(
        clf,
        scaled_features,
        target_discrete).mean()
    return {'loss': -accuracy, 'status': STATUS_OK}
```

Notice that the output of this function is a key-valued dictionary with the two required parameters. These are the 'loss', which is the negative accuracy, and the 'status', which is defined using one of the Hyperopt status strings that we imported previously.

12. The next step is to define the search space we will use to scan all the possible hyperparameters for each of the selected scikit-learn models. This will help us find the one that yields the best accuracy. We will use the hyperopt.choice() function to select the different models and specify the search space for each classification algorithm. The label of this selection is classifier_type. It will iterate over the previously defined types of algorithms in the objective function based on the given labels in the search space:

```
search_space = hp.choice('classifier_type', [
    {
        'type': 'svm',
        'C': hp.lognormal('SVM_C', 0, 1.0),
        'kernel': hp.choice('kernel', ['linear', 'rbf'])
    },
    {
        'type': 'rf',
        'max_depth': hp.quniform('max_depth', 1, 3, 5),
        'criterion': hp.choice('criterion', ['gini', 'entropy'])
    },
    {
        'type': 'logreg',
        'C': hp.lognormal('LR_C', 0, 1.0),
        'solver': hp.choice('solver', ['liblinear', 'lbfgs'])
    },
])
```

The hyperopt.choice() function that is inside the search space expression will pass the 'type' parameter to the objective function, which, in turn, creates an internal variable named classifier_type and uses it to select the appropriate algorithm. It then passes the remaining parameters to the model. Notice how, in the objective function, the 'type' parameter was removed after the internal variable was created to avoid any possible naming conflicts in the target algorithm.

13. Now, we must choose the search algorithm that will be used to scan the search space. As we have seen previously, Hyperopt provides us with two main options for this:

+   a) `hyperopt.tpe.suggest`: This is the **Tree of Parzen Estimators (TPE)** algorithm, which is a Bayesian algorithm that adaptatively selects hyperparameters based on the previous results.
+   b) `hyperopt.rand.suggest`: This is a random search algorithm that differs from the TPE algorithm in that it is a non-adaptive approach that uses a sampling strategy over the search space.

14. In this example, we will use the TPE algorithm, so we will choose `tpe.suggest`, as shown here:

```
search_algorithm = tpe.suggest
```

15. The next step is to define the object that will hold the results of each of the trials. We will use SparkTrials to keep track of the progress of the exploration and log it in MLflow. As we mentioned previously, we will use the SparkTrials object instead of the native `Trials` Hyperopt object because we are using algorithms designed to be run on a single machine. If we were to use an MLlib algorithm, we must use the MLflow API to keep track of the `Trials` result.

In turn, SparkTrials is an object that can receive two optional arguments, as follows:

+   a) `parallelism`: This is the number of algorithms to fit and evaluate in parallel. The default value of this parameter is equal to the number of available Spark task slots.
+   b) `timeout`: Maximum time (in seconds) that `fmin()` can run for. There is no maximum time limit.

The default `parallelism` value in Spark is 8. The recommended setting for this is to explicitly set this value to a positive value, since the Spark task slots is dependent on the cluster's size.

16. The final step of preparing the optimization run is to use the `mlflow.start_run()` MLflow wrapper. We will use it to log the results of this exploration in the MLflow platform. This will automatically keep track of the progress of the run, along with all the defined variables and labels in the objective function and search space:

```
with mlflow.start_run():
    best_hyperparams = fmin(
```
