The next code snippet imports all the required libraries, creates a Spark session, fits the data to the model, and shows the prediction output:

```
from pyspark.ml.clustering import GaussianMixture
from pyspark.ml.linalg import Vectors
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName(
    "GMMClustering"
).getOrCreate()
data = [Vectors.dense(X[i],) for i in range(len(X))]
df = spark.createDataFrame(data, ["features"])
gm = GaussianMixture(k=3, tol=0.0001, seed=10)
model = gm.fit(df)
transformed = model.transform(df).select("features", "prediction")
transformed.show()
```

The output is as shown:

| features | prediction |
| --- | --- |
| [-7.3389880906915... | 1 |
| [-7.7400405564352... | 1 |
| [-1.6866527109495... | 2 |
| [4.42219763300088... | 0 |
| [-8.9177517263291... | 1 |
| [5.49753845943012... | 0 |
| [-2.3360166972015... | 2 |

# Figure 6.15 – GMM prediction output along with features

The next code snippet evaluates the model performance and prints the metrics output:

```
from pyspark.ml.evaluation import ClusteringEvaluator
evaluator = ClusteringEvaluator()
silhouette_score = evaluator.evaluate(transformed)
print(f"Silhouette Score: {silhouette_score}")
```

We get the following output: Silhouette Score: 0.9696628776463786.

The following code snippet plots the cluster:

```
transformed_pd = transformed.toPandas()
plt.scatter(transformed_pd["features"].apply(lambda x: x[0]),
           transformed_pd["features"].apply(lambda x: x[1]),
           c=transformed_pd["prediction"], cmap='viridis')
plt.xlabel('Feature 1')
```