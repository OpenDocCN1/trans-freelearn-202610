# # Pipelines

The pipeline API is part of the `spark.ml` package and it consists of a sequence of stages such as data cleaning, Feature extraction, model training, model validation, model testing, and so on. It provides the ability to chain all these different stages in a sequence for developing any machine learning algorithm. First let's get acquainted with common terminologies used in ML workflow using pipelines:

+   * **Dataframe**: Unlike the `spark.mllib` package, where the datatype was RDD based, in `spark.ml` the datatype is dataframe. Every pipeline expects input data to be dataframe, which can again be created from various Data sources discussed in Chapter 8, Working with Spark SQL, such as from filesystems, external databases, and other data generation sources.

+   * **Transformers**: A transformer transforms a dataframe into another dataframe by either adding a few more columns in the existing dataset or changing the dataframe values itself and thereby giving the resultant in a new dataframe. Some examples of transformers are:

+   * Binarizer transformer converts input columns of a dataframe into `0` or `1` depending on a threshold value

+   * A machine learning model accepts feature vector from dataframe and then appends the predicted label as a new column to the resultant dataframe

A transformer applies the `transform()` method to convert dataframes:

+   * **Estimator**: An estimator is any machine learning algorithm that trains or fits on the training data to produce a model, which again is a transformer. The model then transforms the test data producing resultant value(s) in the output dataframe. For example:

+   * K-means is an estimator that accepts training dataframe to produce K-means model

+   * Decision tree is an estimator that produces a decision tree model