10. Next, we define another function, which in turn will be the one in charge of featurizing a pd.Series array of raw images using the previously defined function to decode them, applying the pre-trained model to it, and returning a pd.Series of image features. Here, we have flattened the output multidimensional tensors to better handle them when loading them to a Spark DataFrame:

```
def featurize_series(model, content_series):
    input = np.stack(content_series.map(preprocess))
    preds = model.predict(input)
    output = [p.flatten() for p in preds]
    return pd.Series(output)
```

11. The next step is to define a Scalar Iterator pandas UDF wrapping for our featurization function. The decorator used here will specify that the output of this process will be the Spark DataFrame column of type ArrayType(FloatType). The content_series_iter argument specifies to iterate over batches of data, where each batch is a pandas Series array of images. Using the Scalar Iterator pandas UDF allows us to load the model once and use it several times over different batches of data, improving the performance of the process of loading big models:

```
@pandas_udf('array<float>', PandasUDFType.SCALAR_ITER)
def featurize_udf(content_series_iter):
    model = model_fn()
    for content_series in content_series_iter:
        yield featurize_series(model, content_series)
```

This way we have successfully registered our function that later can be used to obtain our features based on our pre-trained model.

# Applying featurization to the DataFrame of images

The pandas UDF allows us to manage a large set of unstructured data records, which could be, for example, large images but can give rise to problems such as Out of Memory (OOM) errors:

+   1. We can give a hit that allows such errors in the cell below, to be handled in a more efficient way by reducing the arrow batch size using the maxRecordsPerBatch option:

```
spark.conf.set("spark.sql.execution.arrow.maxRecordsPerBatch", "1024")
```