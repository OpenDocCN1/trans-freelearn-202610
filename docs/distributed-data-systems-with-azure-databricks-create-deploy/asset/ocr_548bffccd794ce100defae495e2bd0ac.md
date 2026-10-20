This categorical data transformed into continuous can be then be passed to algorithms such as DecisionTreeRegressor in order to be able to handle categorical features.

# Normalizer

Another common feature engineering method is to bring the data into a given interval. A first reason to do this is to limit the computations on a fixed range of values to prevent numerical inaccuracies and limit the computational power required that might arise when dealing with numbers that are either too big or too small. A second reason to normalize the numerical values of a feature is that some machine learning algorithms will handle data better when it has been normalized. There are several approaches that we can take in order to normalize our data.

These different approaches are due to some machine learning algorithms requiring different normalization strategies in order to perform efficiently. For example, in the case of k-nearest neighbors (KNN), the range of values in a particular feature impacts the weight of that feature in the model. Therefore, the bigger the values, the more importance the feature will have. In the case of the neural networks, this normalization might not impact the final performance results per se, but it will speed up the training and will avoid the exploiting and vanishing gradient problem that is caused because of the propagation of values that are either too big or too small to subsequent layers in the model. On the other hand, the decision tree-based machine learning algorithms neither benefit nor get hurt by the normalization.

The right normalization strategy will depend on the problem and selected algorithm, rather than some general statistical or computational considerations, and the right selection will always be closely tied to the domain knowledge.

In PySpark, the Normalizer class is a Transformer that transforms a dataset of Vector rows, normalizing each Vector to have the norm of the vector transformed into a unit. It takes as a parameter the value p, which specifies the p-norm used for normalization. This p value is set to 2 by default, which is equal to saying that we are transforming into the Euclidean norm.

The following code example demonstrates how to load a dataset in libsvm format and then normalize each row to have a unit norm:

```
from pyspark.mllib.util import MLUtils
from pyspark.ml.feature import Normalizer
data = MLUtils.loadLibSVMFile(sc, "data/mllib/sample_libsvm_data.txt")
dataFrame = sqlContext.createDataFrame(data)
```