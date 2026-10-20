
- Improving the predictions produced by the models according to the performance metric we have selected for the problem at hand.

With feature engineering, we extract relevant features from the raw input data to be able to accurately represent it according to the way in which the problem to be solved has been modeled, resulting in an improvement in the performance model on novel data. It's good to keep in mind that the features used will influence the performance of the model more than everything else.

Feature engineering can be considered the process of transforming the input data into something that is easier to interpret for the machine learning algorithm by making key features or patterns more transparent, although we can sometimes generate new features to make the data visualization more interpretable or the pattern inference clearer for the model algorithm.

There are multiple strategies applied in feature engineering. Some of them are listed here:

- Missing data imputation and management
- Outlier handling
- Binning
- Logarithmic transformations
- One-hot encoding
- Normalization
- Grouping and aggregation operations
- Feature splitting
- Scaling

Some of these strategies have their application in just some algorithms or datasets, while others can be beneficial in all cases.

It is a common belief to think that the better the features you model, the better the performance you will achieve. While this is true in certain circumstances, it can also be misleading.

The performance of the model has many factors, with different degrees of importance. Among those factors are the selection of the algorithm, the available data, and the features extracted. You can also add to those factors the way in which the problem is modeled, and the objective metrics used to estimate performance. Nevertheless, great features are needed to effectively describe the structures inherent in your data and will make the job of the algorithm easier.

When working with large amounts of data, tools such as Spark SQL and MLlib can be very effective when used in feature engineering. Some third-party libraries are also included in Databricks Runtime ML, such as scikit-learn, which provides useful methods to extract features from data.

This section does not intend to go too deep into the mechanics of each algorithm, although some concepts will be reinforced to understand how they are applied in Azure Databricks.

## Tokenizer

Tokenization can be generally described as a process in which we transform a string of input characters into an array formed by the individual sections that form it. These sections are called tokens and are generally individual words, but can be a certain number of characters, or a combination of words called n-grams. We will use these resulting tokens later on for some other form of processing to extract or transform features. In this sense, the tokenization process can be considered a task of feature engineering. The tokens are identified based on the specific modes that are passed to the parser.

In PySpark, we can use the simple Tokenizer class to tokenize a sequence of string inputs. The following code example shows how we can split sentences into sequences of words:

```
from pyspark.ml.feature import Tokenizer
sentenceDataFrame = sqlContext.createDataFrame([
    (0, "Spark is great for Data Science"),
    (0, "Also for data engineering"),
    (1, "Logistic regression models are neat")
], ["label", "sentence"])
tokenizer = Tokenizer(inputCol="sentence", outputCol="words")
wordsDataFrame = tokenizer.transform(sentenceDataFrame)
for words_label in wordsDataFrame.select("words", "label").take(3):
    print(words_label)
```

Here, we can see the encoded tokens:

```
from pyspark.ml.feature import Tokenizer
sentenceDataFrame = sqlContext.createDataFrame([
(0, "Spark is great for Data Science"),
(0, "Also for data engineering"),
(1, "Logistic regression models are neat")
], ["label", "sentence"])
tokenizer = Tokenizer(inputCol="sentence", outputCol="words")
wordsDataFrame = tokenizer.transform(sentenceDataFrame)
for words_label in wordsDataFrame.select("words", "label").take(3):
    print(words_label)
```

Row(words=['spark', 'is', 'great', 'for', 'data', 'science'], label=0)
Row(words=['also', 'for', 'data', 'engineering'], label=0)
Row(words=['logistic', 'regression', 'models', 'are', 'neat'], label=1)

Figure 8.3 – Output of the word tokenization

We can also use the more advanced RegexTokenizer tokenizer to specify token boundaries using regular expressions.

## Binarizer

Binarization is a process in which you establish a numerical threshold on your features in order to transform them into binary features. Binarization is really useful to preprocess input data that holds continuous numerical features if you can assume that our data has a probabilistic normal distribution. This process makes the data easier to handle for the algorithms used in machine learning and deep learning because they describe the data into a more defined structure.

In PySpark, we can use the simple Binarizer class, which allows us to binarize continuous numerical features. Besides the common parameters of inputCol and outputCol, the simple Binarizer class has a parameter threshold that is used to establish the threshold to binarize the continuous numerical features. The features greater than this threshold are binarized to 1.0, and the ones equal or less than this threshold are binarized to 0.0. The following code example shows how we can use the Binarizer class to binarize numerical features. First, we import the Binarizer class and create an example DataFrame with label and feature columns:

```
from pyspark.ml.feature import Binarizer
continuousDataFrame = sqlContext.createDataFrame([
```

```
(0, 0.345),
(1, 0.826),
(2, 0.142)
], ["label", "feature"] )
```

Next, we instantiate the Binarizer class, specifying a threshold and the input and output columns. Then, we transform the example DataFrame and store and display the results. The code to do this is illustrated in the following snippet:

```
binarizer = Binarizer(threshold=0.5, inputCol="feature",
                        outputCol="binarized_feature")
binarizedDataFrame = binarizer.transform(continuousDataFrame)
binarizedFeatures = binarizedDataFrame.select("binarized_feature")
for binarized_feature, in binarizedFeatures.collect():
    print(binarized_feature)
```

We should see printed the binarized features of the transformed DataFrame according to the threshold that we have established. Next, we will learn how to apply a polynomial expansion to express features in a higher dimensional space.

## Polynomial expansion

In mathematics, a polynomial expansion is a mathematical operation used to express a feature as a product of other higher-level features. Therefore, it can be used to expand features passed to express them as a set of features in a higher dimension.

A polynomial expansion can be considered a process of expanding your features into a polynomial space, in order to try to find patterns in that expanded space that otherwise might be difficult to find in a lower-dimension space. This is common when dealing with algorithms that make simple assumptions, such as linear regression, that might otherwise be unable to capture relevant patterns. This polynomial space in which we transform our features is formed by an n-degree combination of original dimensions. The PySpark PolynomialExpansion class provides us with this functionality. The following code example shows how you can use it to expand your features into a 3-degree polynomial space:

```
from pyspark.ml.feature import PolynomialExpansion
from pyspark.ml.linalg import Vectors
df = spark.createDataFrame([
    (Vectors.dense([2.0, 1.0])),
```

```
(Vectors.dense([0.0, 0.0])), 
(Vectors.dense([3.0, -1.0])), 
], ["features"])
polyExpansion = PolynomialExpansion(degree=3,
inputCol="features", outputCol="polyFeatures")
polyDF = polyExpansion.transform(df)
polyDF.show(truncate=False)
```

Here, we can see the variables that were created in the polynomial expansion:

```
from pyspark.ml.feature import PolynomialExpansion
from pyspark.ml.linalg import Vectors

df = spark.createDataFrame([
    (Vectors.dense([2.0, 1.0])),
    (Vectors.dense([0.0, 0.0])),
    (Vectors.dense([3.0, -1.0]))
], ["features"])

polyExpansion = PolynomialExpansion(degree=3, inputCol="features", outputCol="polyFeatures")
polyDF = polyExpansion.transform(df)

polyDF.show(truncate=False)
```

(2) Spark Jobs
df: pyspark.sql.dataframe.DataFrame = [features: udt]
polyDF: pyspark.sql.dataframe.DataFrame = [features: udt, polyFeatures: udt]
+---------------+---------------------------------------------------------------+
|features       |polyFeatures                                                   |
+---------------+---------------------------------------------------------------+
|[2.0,1.0]      |[2.0,4.0,8.0,1.0,2.0,4.0,1.0,2.0,1.0]                            |
|[0.0,0.0]      |[0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0]                |
|[3.0,-1.0]     |[3.0,9.0,27.0,-1.0,-3.0,-9.0,1.0,3.0,-1.0]                      |
+---------------+---------------------------------------------------------------+

Figure 8.4 – Output features of the polynomial expansion

Having our features transformed into a higher-dimension space is a great tool to use in feature engineering. It allows us to describe data in different terms and increase the sensitivity of the machine learning algorithm to more complex patterns that might be hidden in the data.
