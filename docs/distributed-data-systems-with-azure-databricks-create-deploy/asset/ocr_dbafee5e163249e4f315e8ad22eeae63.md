```
df = spark.read.format("libsvm") \
.option("numFeatures", "784") \
.load(libsvm_path)
```

4. The Petastorm library allows us to work with scalar and array columns in the Spark DataFrame where we load our data. Here, our data is represented using MLlib vectors, which is a user-defined type that requires special handling. We will first need to register a user-defined function that will transform MLlib vectors into dense arrays. We can do this by running the following Scala code in a cell in our Azure Databricks notebook:

```
%scala
import org.apache.spark.ml.linalg.Vector
val toArray = udf {v: Vector => v.toArray }
spark.sqlContext.udf.register("toArray", toArray)
```

5. We will use this user-defined function to transform MLlib sparse vectors into dense arrays that we will then write as files in the Parquet format. The Petastorm library will sample groups of rows of the Parquet file into batches. As mentioned previously, the batch size is an important parameter for the utilization of I/O and computing performance and will yield better results if the optimized FUSE mount in the dfs:/ml directory is used. The parameter that controls the batch size is the parquet.block.size option, which is passed to the parser:

```
parquet_path = os.path.join(work_dir, "parquet")
df.selectExpr("toArray(features) AS features", "int(label) AS label") \
.repartition(10) \
.write.mode("overwrite") \
.option("parquet.block.size", 1024 * 1024) \
.parquet(parquet_path)
```

We can use the Petastorm library to load data in the Parquet format to create, for example, a tf.data.Dataset class instance that we can then feed into our deep learning framework for training our models. Here, we will demonstrate how we can do this by creating a simple convolutional neural network model using Keras, which is a deep learning framework included in TensorFlow.