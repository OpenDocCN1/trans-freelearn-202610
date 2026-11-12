
in the dataset. The transformer accepts a list of tuples in the form (name, transformer, columns). It applies the sub-transformers to the specified columns and concatenates the results such that all resultant features form part of the same result set.

For example, consider the following DataFrame and ColumnTransformer:

```python
df = pd.DataFrame({
    "price": [29.99, 99.99, 19.99],
    "description": ["Luxury goods", "Outdoor goods",
    "Sports equipment"],
})

ct = ColumnTransformer(
    [("scaling", MinMaxScaler(), ["price"]),
    ("vectorize", TfidfVectorizer(), "description")])

transformed = ct.fit_transform(df)
```

Here, we have a DataFrame with two columns: price and description. A column transformer is created with two sub-transformers: a scaling transformer and a vectorizer. The scaling transformer applies min-max scaling only to the price column. The vectorizer applies TF-IDF vectorization only to the description column. When fit_transform is called, a single array is returned with a column for the scaled price and the columns representing the word vectors.

> **Note**
>
> TF-IDF, or term frequency-inverse document frequency, is just one way of extracting a feature from text. Analyzing and extracting features from text, and natural language processing in general, is a broad field within ML that we won't be delving into deeply here. You are encouraged to read further on the topic at https://scikit-learn.org/stable/modules/feature_extraction.html#text-feature-extraction.

We can set up our preprocessing for the Customer Churn dataset as a single ColumnTransformer. We first define the two individual transformers, id_transformer and encode_transformer, that apply to the ID columns and the categorical features:

```python
id_transformer = (
    "customer_id",
    CustomerIdTransformer(id_columns),
    id_columns
)

encode_transformer = (
    "encoder",
    OneHotEncoder(sparse_output=False),
    categorical_features
)
```

And then combine the separate transformers into ColumnTransformer:

```python
preprocessor = ColumnTransformer(
    transformers=[
        id_transformer,
        encode_transformer,
    ],
    remainder='passthrough'
)
```

ColumnTransformer is defined with the remainder='passthrough' parameter. The remainder parameter specifies what happens to the columns that ColumnTransformer does not transform. These columns are dropped by default, but we would like to pass them through, untouched, to include them in the dataset.

The encoding transformer creates and applies OneHotEncoder to the categorical features.

For illustrative purposes, we have created a custom transformer class to maintain the list of ID columns and drop them from the data during transformation.

The class is shown here:

```python
class CustomerIdTransformer(BaseEstimator, TransformerMixin):
    def __init__(self, id_columns):
        self.id_columns = id_columns

    def fit(self, X, y=None):
        return self

    def transform(self, X, y=None):
        return X.drop(columns=self.id_columns, axis=1)
```

As we can see, the class extends BaseEstimator and TransformerMixin from the scikit-learn base classes and must implement fit and transform. Implementing these methods also makes it suitable as a transformer in a pipeline. Implementing fit is optional if required; in our case, nothing is done during fit. Our transformation step drops the relevant columns.

> **Note**
>
> It's important to encapsulate the functionality to drop irrelevant columns (in this case, the ID column) within the pipeline itself. When deploying the pipeline for production use, we expect these columns to be passed to the pipeline when making a prediction. Removing them as part of the pipeline simplifies our pipeline's usage for our model's consumers and reduces the chance of user mistakes.

This completes the transformations required for preprocessing, and we are ready to move on to the following steps: fitting and tuning the models.

## Pipeline modeling steps

For the pipeline modeling part, we'll use FLAML. We'll also use the opportunity to show how parameters may be passed to steps within a pipeline. First, we define the settings for our AutoML model:

```python
automl_settings = {
    "time_budget": 120,
    "metric": "accuracy",
    "task": "classification",
    "estimator_list": ["lgbm", "rf"],
    "custom_hp": {
        "n_estimators": {
            "domain": flaml.tune.uniform(20, 500)
        }
    },
    "verbose": -1
}
```

The preceding code sets our time budget, optimization metric, and classification task for AutoML. We also limit the estimators to LightGBM and a random forest model. Finally, we customize the search space by specifying that n_estimators should be uniformly sampled between 20 and 500.

The pipeline requires the parameter for constituent steps to be prefixed with the step's name and a double underscore. We can set up a dictionary to pass these parameters to the AutoML class within our pipeline:

```python
pipeline_settings = {
    f"automl__{key}": value for key, value in
    automl_settings.items()
}
```

Here, automl is the name of the step in the pipeline. As such, for example, the parameters for time budget and metric are set as automl__time_budget: 120 and automl__metric: accuracy, respectively.

Finally, we can add FLAML's AutoML estimator:

```python
automl = flaml.AutoML()
pipeline = Pipeline(
    steps=[("preprocessor", preprocessor),
           ("automl", automl)]
)
```

The final pipeline is shown in the following figure:

![](img/31b70e4a77c3afbe11032f68ab874b02_177_0.png)

Figure 8.1 – Final ML pipeline for Customer Churn prediction

Figure 8.1 shows a ColumnTransformer that consists of two sub-transformers, feeding into the AutoML estimator.

## Model training and validation

We are now ready to fit the pipeline to our data, passing the pipeline settings we set up earlier. Pipelines support the standard scikit-learn API so that we can call on the pipeline itself:

```python
pipeline.fit(X, y, **pipeline_settings)
```

Running fit executes all the preprocessing steps and then passes the data for AutoML modeling and tuning. The single Pipeline object illustrates the power of an ML pipeline: the entire end-to-end process, including a trained and tuned model, is encapsulated and portable, and we can utilize the pipeline as we could a single model. For example, the following code performs F1 scoring for the training data:

```python
print(f"F1: {f1_score(pipeline.predict(X), y,
pos_label='Yes')}")
```

To export the pipeline, we use joblib to serialize the model to a file:

```python
joblib.dump(pipeline, "churn_pipeline.pkl")
```

Exporting the pipeline allows us to re-instantiate and use it within our production code. Next, we'll look at building an API for our model.

At this stage, our pipeline (which encapsulates preprocessing, training, optimization, and validation) is defined, and we are ready to deploy it to a system. We'll accomplish this by wrapping our model in an API with FastAPI.

## Building an ML API using FastAPI

We will now look at building a REST API around our pipeline, enabling consumers of our pipeline to get predictions via web requests. Building a web API for a model also simplifies integration with other systems and services and is the standard method for integration in a microservices architecture.

To build the API, we use the Python library FastAPI.

### FastAPI

FastAPI is a modern, high-performance web framework for building APIs with Python 3.6+. It was designed from the ground up to be easy to use and enable high-performance API development. The key features of FastAPI are its speed and ease of use, making it an excellent choice for developing robust, production-ready APIs. FastAPI widely adopts Python's type checking, which aids in catching errors early in the development process. It also uses these type hints to provide data validation, serialization, and documentation, reducing the boilerplate code developers need to write.

The performance of FastAPI is one of its defining features. It is on par with Node.js and significantly faster than traditional Python frameworks. This speed is achieved due to its use of Starlette for the web parts and Pydantic for the data parts, and its non-blocking nature makes it suitable for handling many concurrent requests.

FastAPI provides automatic interactive API documentation, a considerable advantage while developing complex APIs. Using FastAPI, developers gain access to automatically generated interactive API docs via Swagger UI. Swagger UI also provides functionality to interact with the REST resources without writing code or using external tooling. This feature makes FastAPI very developer-friendly and accelerates the development process.
