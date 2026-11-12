| id | features |
|---|---|
| 0 | [1.0, 2.0] |
| 1 | [2.0, 3.0] |
| 2 | [3.0, 4.0] |

Figure 3.29 – Data in a Spark DataFrame

The following code snippet applies the PolynomialExpansion function to transform the input column into polynomial features:

```
poly_expansion = PolynomialExpansion(inputCol="features",
                                    outputCol="expanded_features", degree=2)
expanded_df = poly_expansion.transform(df)
expanded_df.select("id", "expanded_features").show(truncate=False)
```

| id | expanded_features |
|---|---|
| 0 | [1.0,1.0,2.0,2.0,4.0] |
| 1 | [2.0,4.0,3.0,6.0,9.0] |
| 2 | [3.0,9.0,4.0,12.0,16.0] |

Figure 3.30 – Polynomial features in a Spark DataFrame

The PolynomialExpansion generates new features by creating polynomial combinations of the original features. For example, the first row has [1.0, 2.0] features, and the expanded features include [1.0, 2.0, 1.0, 4.0, 0.0, 0.0].

In the next section, we will explore more about feature selectors.

# Exploring feature selectors

In this section, we will see what a feature selector is, why it is very important in machine learning, and its various techniques. A feature selector in machine learning is a tool or method used to automatically select a subset of relevant features (variables and predictors) for use in model construction. This process is an essential part of the feature engineering phase and significantly impacts the performance of machine learning models.