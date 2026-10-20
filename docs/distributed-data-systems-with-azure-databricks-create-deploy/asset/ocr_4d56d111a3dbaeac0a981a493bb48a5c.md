```
result_bucketizer = Bucketizer(splits=splits,
inputCol="hour",
outputCol="result").transform(df)
result_bucketizer.show()
```

Here the QuantileDiscretizer class will determine the bucket splits based on the data, while the Bucketizer class will arrange the data into buckets based on what you have specified via splits.

Therefore, it is better to use Bucketizer when you already have knowledge of which buckets you expect, while the QuantileDiscretizer class will estimate the splits for you.

In the preceding example, the outputs of both processes are similar because of the input data and the splits selected. Otherwise, results may vary significantly in different scenarios.

# Element-wise product

An element-wise product is a mathematical operation very commonly used in feature engineering when working with data that is arranged as a matrix. The PySpark ElementwiseProduct class will multiply each input vector by a previously specified weight vector. To do this, it will use an element-wise multiplication that will scale each column of the input vector by a scalar multiplier defined as the weight.

The ElementwiseProduct class takes scalingVec as the main parameter, which is the transforming vector of this operation.

The following example will exemplify how we can transform vectors using the ElementwiseProduct class and a transforming vector value:

```
from pyspark.mllib.linalg.distributed import RowMatrix
v1 = sc.parallelize([[2.0, 2.0, 2.0], [3.0, 3.0, 3.0]])
mat = RowMatrix(v1)
v2 = Vectors.dense([0.0, 1.0, 2.0])
transformer = ElementwiseProduct(v2)
transformedData = transformer.transform(mat.rows)
print transformedData.collect()
```

This operation can then be understood as the Hadamard product, which is also known as the element-wise product between the input array and the weight vector, which in turn will give as a result another vector.