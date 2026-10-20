5. Now that we have a unified dataset, we will create a new feature named high_quality for all the wines that have a quality above 7, as follows:

```
high_quality = (data.quality >= 7).astype(int)
data.quality = high_quality
```

6. Afterward, we can split our dataset in train and test datasets, which we will use to train our machine learning model later on, inferring on the quality column. In the following code block, we import the scikit-learn train_test_split function in order to split the data into the features defined by X and the objective column defined as y:

```
from sklearn.model_selection import train_test_split
train, test = train_test_split(data, random_state=123)
X_train = train.drop(["quality"], axis=1)
X_test = test.drop(["quality"], axis=1)
y_train = train.quality
y_test = test.quality
```

As a result, we have the features and objective columns split into train and test datasets that can be used to train and benchmark the model.

# Building the ML model

Here, we will use a classifier given the fact that what we are trying to predict is a binary output on a tabular dataset. The selected algorithm will be a Random Forest classifier available in the scikit-learn Python library. We will also make use of MLflow to keep track of the model performance, and to save the model for later use:

+   1. We will start by doing all the necessary imports, as follows:

```
import mlflow
import mlflow.pyfunc
import mlflow.sklearn
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import roc_auc_score
from mlflow.models.signature import infer_signature
```