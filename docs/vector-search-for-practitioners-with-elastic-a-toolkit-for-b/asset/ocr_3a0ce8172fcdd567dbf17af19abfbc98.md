The dot product is interesting when working with data that has both positive and negative values, and where the angle between vectors is not important. Dot product can be positive, negative, or zero. It is not normalized, so the magnitudes of the vectors can impact the results. In addition, if the vectors are normalized (i.e., their magnitudes are 1), the cosine similarity of normalized vectors is equivalent to the dot product:

cos similarity = A . B = dot product

This is valuable information, as vector search on Elasticsearch is faster with the dot product.

In the context of a vector representing text, vectors can have both negative and positive values. These values are derived from the training process and the algorithms used to create the embeddings. The positive or negative values in these vectors do not necessarily have a specific meaning by themselves. So, two vectors with high magnitudes but very different directions will have a large dot product, even though they are not semantically similar.

In contrast, cosine similarity, derived from the dot product, normalizes the magnitude of vectors and focuses on the angle between them, which makes them much more suitable for text data. It measures the cosine of the angle between vectors, capturing their semantic similarity. Since it’s normalized, ranging from −1 to 1, cosine similarity that is less sensitive to the scale of the features is also less sensitive to the magnitude.

Note the two important components to understand here:

+   - The direction which is defined by the angle between vectors
+   - The magnitude

Let’s look at these components in detail.

# Directions and magnitude

We have seen that, in the context of text data, vectors are typically created using word embeddings or document embeddings, which are dense vector representations that capture the semantic meaning of words or documents in a continuous vector space.

The direction and magnitude of these vectors are related to the actual text through the relationship between words in the dataset. The direction of a vector representation of text indicates the semantic orientation of the text in the high-dimensional vector space. Vectors with similar directions represent semantically similar texts. They share similar context or meanings. In other words, the angle between the vectors of two texts is small when the texts are semantically related.

In contrast, if the angle is large, it indicates that the texts have different meanings or contexts. This is why cosine similarity, which focuses on the angle between vectors, is a popular choice to measure semantic similarity in text data.