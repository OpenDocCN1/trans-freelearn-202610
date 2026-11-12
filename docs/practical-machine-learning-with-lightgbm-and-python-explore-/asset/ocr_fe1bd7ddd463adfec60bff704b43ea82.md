Finally, we can add FLAML's AutoML estimator:

```
automl = flaml.AutoML()
pipeline = Pipeline(
    steps=[("preprocessor", preprocessor),
           ("automl", automl)]
)
```

The final pipeline is shown in the following figure:

![](bbox=[0.195, 0.26, 0.77, 0.635])

Figure 8.1 – Final ML pipeline for Customer Churn prediction

Figure 8.1 shows a ColumnTransformer that consists of two sub-transformers, feeding into the AutoML estimator.

# Model training and validation

We are now ready to fit the pipeline to our data, passing the pipeline settings we set up earlier. Pipelines support the standard scikit-learn API so that we can call on the pipeline itself:

```
pipeline.fit(X, y, **pipeline_settings)
```

Running fit executes all the preprocessing steps and then passes the data for AutoML modeling and tuning. The single Pipeline object illustrates the power of an ML pipeline: the entire end-to-end