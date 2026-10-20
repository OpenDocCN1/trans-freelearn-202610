4. We can see the entire process of generating the variables, as shown in the following screenshot:

```
from pyspark.ml.feature import HashingTF, IDF, Tokenizer
sentenceData = sqlContext.createDataFrame([
(0, "Hi I heard about Spark"),
(0, "I wish Java could use case classes"),
1, ("Logistic regression models are neat")
], ["label", "sentence"])
tokenizer = Tokenizer(inputCol="sentence", outputCol="words")
wordsData = tokenizer.transform(sentenceData)
hashingTF = HashingTF(inputCol="words", outputCol="rawFeatures", numFeatures=20)
featurizedData = hashingTF.transform(wordsData)
idF = IDF(inputCol="rawFeatures", outputCol="features")
idFModel = idf.fit(featurizedData)
rescaledData = idfModel.transform(featurizedData)
for features_label in rescaledData.select("features","label").take(3):
    print(features_label)
```

Figure 8.8 – Output features of the TF-IDF operation

In this way, we have our features extracted from the text and converted into scaled numerical features that can be fed into any learning algorithm.

# Word2vec

Word2vec is an embedding technique that is used in feature engineering for NLP. It uses a neural network to learn the associations between words from a large corpus of text and it produces vectors that represent each token. This vector holds properties that allow us to indicate the semantic similarity between two words just by measuring the distance between the vectors that represent them, using (for example) cosine similarity. These vectors are often called word embeddings and they commonly have several hundreds of dimensions used to represent them.

In PySpark, Word2vec is an Estimator class that takes as input an array of and trains a Word2VecModel, which is a model that maps each word into a single fixed-sized vector.