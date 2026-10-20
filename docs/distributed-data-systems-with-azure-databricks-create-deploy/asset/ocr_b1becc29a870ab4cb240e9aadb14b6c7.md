When working with large amounts of data, tools such as Spark SQL and MLlib can be very effective when used in feature engineering. Some third-party libraries are also included in Databricks Runtime ML, such as scikit-learn, which provides useful methods to extract features from data.

This section does not intend to go too deep into the mechanics of each algorithm, although some concepts will be reinforced to understand how they are applied in Azure Databricks.

# Tokenizer

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