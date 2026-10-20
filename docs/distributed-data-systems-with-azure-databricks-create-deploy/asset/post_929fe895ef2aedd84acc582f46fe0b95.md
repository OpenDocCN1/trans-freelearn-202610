
4. We can see the entire process of generating the variables, as shown in the following screenshot:

```
from pyspark.ml.feature import HashingTF, IDF, Tokenizer
sentenceData = sqlContext.createDataFrame([
(0, "Hi I heard about Spark"),
(0, "I wish Java could use case classes"),
(1, "Logistic regression models are neat")
], ["label", "sentence"])
tokenizer = Tokenizer(inputCol="sentence", outputCol="words")
wordsData = tokenizer.transform(sentenceData)
hashingTF = HashingTF(inputCol="words", outputCol="rawFeatures", numFeatures=20)
featurizedData = hashingTF.transform(wordsData)
idf = IDF(inputCol="rawFeatures", outputCol="features")
idfModel = idf.fit(featurizedData)
rescaledData = idfModel.transform(featurizedData)
for features_label in rescaledData.select("features","label").take(3):
    print(features_label)
```

Figure 8.8 – Output features of the TF-IDF operation

In this way, we have our features extracted from the text and converted into scaled numerical features that can be fed into any learning algorithm.

## Word2vec

Word2vec is an embedding technique that is used in feature engineering for NLP. It uses a neural network to learn the associations between words from a large corpus of text and it produces vectors that represent each token. This vector holds properties that allow us to indicate the semantic similarity between two words just by measuring the distance between the vectors that represent them, using (for example) cosine similarity. These vectors are often called word embeddings and they commonly have several hundreds of dimensions used to represent them.

In PySpark, Word2vec is an Estimator class that takes as input an array of tokens and trains a Word2VecModel, which is a model that maps each word into a single fixed-sized vector.

In the code example shown next, we start with a set of documents, each of them represented as a sequence of words. For each document, we transform it into a feature vector. This feature vector could then be passed to a learning algorithm:

1. In the following code example, we will create a Spark data frame with sentences that are split into their tokens using a simple split on the string, meaning that each row represents a BOW from that sentence:

```
from pyspark.ml.feature import Word2Vec
documentDF = sqlContext.createDataFrame([
    ("Hi I heard about Spark".split(" "), ),
    ("I wish Java could use case classes".split(" "), ),
    ("Logistic regression models are neat".split(" "), )
], ["text"])
```

2. Then, we can map those arrays into vectors, as follows:

```
word2Vec = Word2Vec(vectorSize=3, minCount=0,
inputCol="text", outputCol="result")
```

3. Finally, we can fit the Word2Vec model to the array we just created and obtain the resulting features by transforming the input DataFrame using the fitted model, as follows:

```
model = word2Vec.fit(documentDF)
result = model.transform(documentDF)
for feature in result.select("result").take(3):
    print(feature)
```

4. We can see here the whole process executed on a single cell in an Azure Databricks notebook:

```
Cmd 14
1 from pyspark.ml.feature import Word2Vec
2 documentDF = sqlContext.createDataFrame([
3   ("Hi I heard about Spark".split(" "), ),
4   ("I wish Java could use case classes".split(" "), ),
5   ("Logistic regression models are neat".split(" "), )
6 ], ["text"])
7 word2Vec = Word2Vec(vectorSize=3, minCount=0, inputCol="text", outputCol="result")
8 model = word2Vec.fit(documentDF)
9 result = model.transform(documentDF)
10 for feature in result.select("result").take(3):
11   print(feature)
12
```

(4) Spark Jobs
documentDF: pyspark.sql.dataframe.DataFrame = [text: array]
result: pyspark.sql.dataframe.DataFrame = [text: array, result: udt]
Row(result=DenseVector([-0.0627, -0.0219, -0.0816]))
Row(result=DenseVector([0.0242, 0.0236, 0.023]))
Row(result=DenseVector([0.0483, -0.0189, -0.0037]))

Figure 8.9 – Output embeddings using Word2Vec

In this way, we can very simply create a numerical representation obtained from text sources, using one of the most widely used models in text embeddings: Word2Vec.

## Training machine learning models on tabular data

In this example, we will use a very popular dataset in data science, which is the wine dataset of physicochemical properties, to predict the quality of a specific wine. We will be using Azure Databricks Runtime ML, so be sure to attach the notebook to a cluster running this version of the available runtimes, as specified in the requirements at the beginning of the chapter.

## Engineering the variables

We'll get started using the following steps:

- 1. Our first step is to load the necessary data to train our models. We will load the datasets, which are stored as example datasets in DBFS, but you can also get them from the UCI Machine Learning repository. The code is shown in the following snippet:
```
import pandas as pd
white_wine = pd.read_csv("/dbfs/databricks-datasets/wine-quality/winequality-white.csv", sep=",")
red_wine = pd.read_csv("/dbfs/databricks-datasets/wine-quality/winequality-red.csv", sep=";")
```
- 2. Next, we will merge these two pandas Data Frames into a single one, and add a new binary feature named is_red to distinguish between the two of them, as follows:
```
red_wine['is_red'] = 1
white_wine['is_red'] = 0
data = pd.concat([red_wine, white_wine], axis=0)
```
- 3. After this, we will remove spaces from the column names in order to be able to save this as a Spark data frame, as follows:
```
data.rename(columns=lambda x: x.replace(' ', '_'), inplace=True)
data.head()
```
- 4. Here, we can see the created Pandas Data frame:

```
data.rename(columns=lambda x: x.replace(' ', '_'), inplace=True)
data.head()
```

![](img/7e45bd08916c2a17d4e75cbb898f41e8_278_0.png)

Figure 8.10 – Renamed columns of the dataset

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

## Building the ML model

Here, we will use a classifier given the fact that what we are trying to predict is a binary output on a tabular dataset. The selected algorithm will be a Random Forest classifier available in the scikit-learn Python library. We will also make use of MLflow to keep track of the model performance, and to save the model for later use:

- 1. We will start by doing all the necessary imports, as follows:

```
import mlflow
import mlflow.pyfunc
import mlflow.sklearn
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import roc_auc_score
from mlflow.models.signature import infer_signature
```

2. We will create a wrapper class named SklearnModelWrapper around the model, inheriting from the mlflow.pyfunc.PythonModel class. This class will return the probability of an instance being part of a determined class. The code is shown in the following snippet:

```
class SklearnModelWrapper(mlflow.pyfunc.PythonModel):
    def __init__(self, model):
        self.model = model
    def predict(self, context, model_input):
        return self.model.predict_proba(model_input)[:,1]
```

3. After we have defined our model, we will use mlflow.start_run to create a new MLflow run that will allow us to keep up with the track performance. We can also call mlflow.log_param to show which parameters are being passed to the model. In the following code example, mlflow.log_metric will be recording the performance metrics that we have selected, such as the receiver operating characteristic (ROC) in this case:

```
with mlflow.start_run(run_name='untuned_random_forest'):
    n_estimators = 10
    model = RandomForestClassifier(n_estimators=n_estimators, random_state=np.random.RandomState(123))
    model.fit(X_train, y_train)
    predictions_test = model.predict_proba(X_test)[:,1]
    auc_score = roc_auc_score(y_test, predictions_test)
    mlflow.log_param('n_estimators', n_estimators)
    mlflow.log_metric('auc', auc_score)
    wrappedModel = SklearnModelWrapper(model)
    signature = infer_signature(X_train, wrappedModel.predict(None, X_train))
    mlflow.pyfunc.log_model("random_forest_model", python_model=wrappedModel, signature=signature)
```
