
## Datatypes

Though dataframes have become the de facto way of doing ML operations, we will still go over basic knowledge of traditional RDD-based Spark MLlib datatypes as follows:

- Local vector: Local vectors are stored on a single machine having values as double and indices of each such values are stored as an integer starting with zero. Local vectors can further be sub classified into dense and sparse vectors. A double array representing its values is called dense vector while sparse vector contains two arrays representing indices and values separately.

For example, a vector ( 2.0, 0.0, 5.0, 3.0 ) can be represented as a:

- Dense vector as [ 2.0, 0.0, 5.0, 3.0]
- Sparse vector as [4, (0,2,3), (2,0,5.0,3.0) ]
- Labeled point: A dense or sparse local vector having a label or a response value is called a labeled point. They are used in supervised learning more prominently in classification and regression-based algorithms. Labeled point values are stored in a double datatype.

| Algorithm | Values |
|---|---|
| Binary classification | 0 or 1 |
| Multiclass classification | 0, 1, 2... |
| Regression | Double |

- Local matrix: A matrix that is stored on a single machine having values as a double datatype and row and column indices being represented as integer is called local matrix. It can further be segregated into the dense matrix whose values are stored in a single double array in column major order while sparse matrix stores non zero values in Compressed Sparse Column (CSC) format in column major order.

- Distributed matrix: A distributed matrix is stored in one or more RDDs and hence is distributed in nature. Also the row and column indices of distributed matrix are of type long while the values are of double type. Conversion of a distributed matrix to any other type may require shuffling and hence is an expensive operation. Distributed matrices have been further classified into the following sub categories:
  - Row matrix: Row-oriented distributed matrix without meaningful row indices-RDD of sequence of vector without rows indices
  - Indexed row matrix: like a row matrix, but with meaningful row indices
  - Coordinate matrix: elements values are explicit defined by using IndexedRow(row_index, col_index, value)
  - Block matrix: included set of matrix block (row_index, col_index, matrix)

![](img/a77bd9f099223ea139d6d612f2b90712_423_0.png)

Spark 2.0 introduced the paradigm shift in the way machine learning algorithms are implemented in Spark. They not only changed the RDD-based operation to dataframe based operations by introducing a new package spark.ml, but also focused on solving problems using pipelines. Some of the important concepts surrounding pipelines and its associated terminologies are discussed in the following sections.

## Machine learning work flow

Machine learning work flow in general involves two set of operations: one involving training the model and the other testing the trained model with newer datasets.

| Training | Test Model |
|---|---|
| Load data | Load test data |
| Extract features | Extract features |
| Train model | Predict using model |
| Evaluate | Result |

The ML workflow requires iterations over the same set of training data over and over again while only changing the tuning parameters. The objective of such an iterative process is to achieve minima in error while evaluating the values on training data and only then the model is fed with test data to give resultant output. In order to combine all these sequences of data pre-processing, feature extraction, model fitting, and validation stages in a ML workflow, spark introduced Pipelines.

## Pipelines

The pipeline API is part of the `spark.ml` package and it consists of a sequence of stages such as data cleaning, Feature extraction, model training, model validation, model testing, and so on. It provides the ability to chain all these different stages in a sequence for developing any machine learning algorithm. First let's get acquainted with common terminologies used in ML workflow using pipelines:

- Dataframe: Unlike the `spark.mllib` package, where the datatype was RDD based, in `spark.ml` the datatype is dataframe. Every pipeline expects input data to be dataframe, which can again be created from various Data sources discussed in Chapter 8, Working with Spark SQL, such as from filesystems, external databases, and other data generation sources.

- Transformers: A transformer transforms a dataframe into another dataframe by either adding a few more columns in the existing dataset or changing the dataframe values itself and thereby giving the resultant in a new dataframe. Some examples of transformers are:
  - Binarizer transformer converts input columns of a dataframe into `0` or `1` depending on a threshold value
  - A machine learning model accepts feature vector from dataframe and then appends the predicted label as a new column to the resultant dataframe

A transformer applies the `transform()` method to convert dataframes:

- Estimator: An estimator is any machine learning algorithm that trains or fits on the training data to produce a model, which again is a transformer. The model then transforms the test data producing resultant value(s) in the output dataframe. For example:
  - K-means is an estimator that accepts training dataframe to produce K-means model
  - Decision tree is an estimator that produces a decision tree model

An estimator has a fit() method that is to be invoked on learning dataframe in order to generate a model that can transform the input data.

- Pipeline: A pipeline chains together multiple stages of machine learning workflow each comprising of a transformer or an estimator. The sequence of job chaining is specific to an algorithm or use case and hence the order of PipelineStages, that is, transformer and estimator, as being passed to a pipeline is important.

Now that we understand these common terminologies, let's write our first Machine Learning algorithm and understand how these parameters are related to each other. For simplicity we will try and understand a very basic regression model called linear regression. Please be forewarned that the results will not be perfect as our intention here is to only understand major components in an ML workflow.

A simple linear regression model can be represented as an equation of the form:

```
y = ax + b
```

Where x is a variable or in ML parlance a feature vector and a is the slope of the line. b is a constant that represents the intercept on the y axis when x=0.

![](img/a77bd9f099223ea139d6d612f2b90712_427_0.png)

Using this model of linear regression, we will try and predict the values of **total impression**, which is sum total of like, comment, and share that a Facebook post will get depending on feature vector parameters such as Post Month, Post Weekday, Post Hour, and so on.

The dataset can be downloaded from http://archive.ics.uci.edu/ml/datasets/Facebook+metrics and it has around 500 records. To keep the model simple, we will select only a few columns from the preceding dataset and call it formattedDF. From the feature vector formattedDF we will split the dataset into two, one being train dataset and the other being test dataset. Also since, the linear regression model accepts single feature vectors we will use VectorAssembler, a transformer that combines multiple raw features into a single feature vector. We will then pass the instance of VectorAssembler to a pipeline along with the instance of LinearRegression, which together returns a transformer instance lrPipeline. The pipeline instance lrPipeline is then trained using the fit() method on the training data. The fit() method returns a PipelineModel on which test data can be tested:

```
Dataset<Row> formattedDF = ds.select(col("Page total likes"),col("Category"),col("Post Month"),col("Post Weekday"),col("Post Hour"),col("Paid"),col("Lifetime People who have liked your Page and engaged with your post"),col("Total Interactions"));
formattedDF.show();
//Split the sample into Training and Test data
Dataset<Row>[] data = formattedDF.na().drop().randomSplit(new double[]{0.7,0.3});
System.out.println("We have training examples count ::"+data[0].count() +
" and test examples count ::"+data[1].count()));

String[] featuresCols = formattedDF.drop("Total Interactions").columns();

//This concatenates all feature columns into a single feature vector in a new column "rawFeatures".
VectorAssembler vectorAssembler = new VectorAssembler().setInputCols(featuresCols).setOutputCol("rawFeatures");

//StandardScaler scaler = new StandardScaler().setInputCol("rawFeatures").setOutputCol("features").setWithStd(true).setWithMean(false);

LinearRegression lr = new LinearRegression().setLabelCol("Total Interactions").setFeaturesCol("rawFeatures").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8);

// Chain vectorAssembler and lr in a Pipeline
```
