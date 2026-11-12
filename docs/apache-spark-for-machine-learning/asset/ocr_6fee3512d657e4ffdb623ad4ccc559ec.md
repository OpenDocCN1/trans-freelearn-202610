+   - **StringIndexer**: Converts a column of string labels into a column of label indices. It’s often used to transform categorical text data into numerical indices that machine learning models can process.

+   - **OneHotEncoder**: Maps a column of category indices to a column of binary vectors. This is particularly useful after using StringIndexer to further transform categorical variables into a format that machine learning algorithms can utilize.

+   - **Tokenizer and RegexTokenizer**: Splits text into individual words or tokens. These are fundamental steps in text analysis and NLP tasks.

+   - **StopWordsRemover**: Removes common, uninformative words from a set of tokenized words. This helps to reduce the dimensionality of text data and focus on more meaningful words.

+   - **Bucketizer**: Transforms a column of continuous features into a column of feature buckets, based on user-specified splits. This is useful for converting continuous variables into categorical ones.

+   - **StandardScaler**: Standardizes features by removing the mean and scaling to unit variance. This is crucial for many machine learning models that assume that input features are on similar scales.

+   - **MinMaxScaler**: Transforms each feature to a given range, often [0, 1]. This is useful when you need to normalize the features to a specific range without distorting differences in the ranges of values.

+   - **Normalizer**: Transforms a dataset of vector rows, normalizing each vector to have a unit norm. This is used when you want to scale the vector observations to be of unit norm, which can be useful in clustering algorithms.

+   - **PCA**: Reduces the dimensionality of data by projecting it onto a smaller set of orthogonal features. PCA is used in exploratory data analysis and to make predictive models more efficient.

+   - **PolynomialExpansion**: Expands your feature vectors into a polynomial space, which is formulated by an n-degree combination of original dimensions. This can help to capture interactions between original features.

# ## Code examples for feature transformers

Now, let's look at feature transformers through some code examples.

# ### VectorAssembler

The following code snippet creates a sample Spark DataFrame with four columns, also called features:

```
from pyspark.ml.feature import VectorAssembler
data = [
(1, 2, 3, 4),
(5, 6, 7, 8),
(9, 10, 11, 12)
]
```