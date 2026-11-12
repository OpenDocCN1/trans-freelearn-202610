
nationalities = df.groupby(['Nacionality', 'Target']).size().reset_index().pivot(columns='Target', index='Nacionality', values=0)
nationalities_total = nationalities.sum(axis=1)
nationalities_total = nationalities_total.sort_values(ascending=True)
nationalities.loc[nationalities_total.index].plot(kind='barh', stacked=True)

```
nationalities = df.groupby(['Nacionality', 'Target']).size().reset_index().pivot(columns='Target', index='Nacionality', values=0)
nationalities_total = nationalities.sum(axis=1)
nationalities_total = nationalities_total.sort_values(ascending=True)
nationalities.loc[nationalities_total.index].plot(kind='barh', stacked=True)
```

The following screenshot shows a stacked bar plot of the nationalities:

![](img/31b70e4a77c3afbe11032f68ab874b02_79_0.png)

Figure 3.7 – Distribution of the 'Nacionality' feature, showing almost all rows have a single value in the academic success dataset

The distribution of 'Nacionality' and 'International' means that they are not very informative (nearly all rows have the same value), so we can drop them from the dataset.

Finally, we notice the 'Gender' feature. When working with gender, it's always good to check for bias. We can visualize the distribution of the 'Gender' feature relative to the target classes using a histogram. The results are shown in the screenshot that follows this code snippet:

```
sns.countplot(data=df, x='Gender', hue='Target', hue_order=['Dropout',
'Enrolled', 'Graduate'])
plt.xticks(ticks=[0, 1], labels=['Female', 'Male'])
```

![](img/31b70e4a77c3afbe11032f68ab874b02_80_0.png)

Figure 3.8 – Distribution of the 'Gender' feature in the academic success dataset

There is a slight bias toward female students, but not enough to warrant concern.

## Modeling

We can now prepare our dataset for modeling. We must map our Target values to integers and drop the Nacionality and International features. We also need to remove the spaces in the feature names. LightGBM cannot work with spaces in the names; we can replace them with underscores:

```
df.columns = df.columns.str.strip().str.replace(' ', '_')
df = df.drop(columns=["Nacionality", "International"], axis=1)
df["Target"]=df["Target"].map({
    "Dropout":0,
    "Enrolled":1,
    "Graduate":2
    })
X = df.drop(columns=["Target"], axis=1)
y = df["Target"]
```

We train and compare four models: a LightGBM GBDT, a LightGBM DART tree, a LightGBM DART tree with GOSS, and a scikit-learn random forest.

We'll perform parameter optimization with 5-fold cross-validation using GridSearchCV to ensure good performance for the models.

The following code sets up the parameter optimization for the GBDT. A similar pattern is followed for the other models, which can be seen in the source code:

```
def gbdt_parameter_optimization():
    params = {
        "max_depth": [-1, 32, 128],
        "n_estimators": [50, 100, 150],
        "min_child_samples": [10, 20, 30],
        "learning_rate": [0.001, 0.01, 0.1],
        "num_leaves": [32, 64, 128]
    }

    model = lgb.LGBMClassifier(force_row_wise=True, boosting_type="gbdt", verbose=-1)
    grid_search = GridSearchCV(estimator=model, param_grid=params,
                               cv=5, verbose=10)
    grid_search.fit(X, y)
    return grid_search

results = gbdt_parameter_optimization()
print(results.best_params_)
print(results.best_score_)
```

Running the preceding code takes some time, but once completed, it prints the best parameters found along with the score of the best model.

After all the models are trained, we can evaluate each using F1-scoring, taking the mean of 5-fold cross-validation, using the best parameters found. The following code illustrates how to do this for the GBDT model:

```
model = lgb.LGBMClassifier(force_row_wise=True, boosting_type="gbdt",
    learning_rate=0.1, max_depth=-1, min_child_samples=10, n_estimators=100, num_leaves=32, verbose=-1)
scores = cross_val_score(model, X, y, scoring="f1_macro")
scores.mean()
```

Jupyter notebooks for the parameter optimization for each model are available in the GitHub repository: https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-3.

The following table summarizes the best parameter values found and the cross-validated F1 scores for each model:

| Model | Learning Rate | Max Depth | Min Child Samples | N Estimators | Num Leaves | Min Samples Leaf | Min Samples Split | F1 score |
|---|---|---|---|---|---|---|---|---|
| GBDT | 0.1 | - | 10 | 100 | 32 | N/A | N/A | 0.716 |
| DART | 0.1 | 128 | 30 | 150 | 128 | N/A | N/A | 0.703 |
| DART (GOSS) | 0.1 | 128 | 30 | 150 | 128 | N/A | N/A | 0.703 |
| Random Forest | N/A | N/A | N/A | 150 | N/A | 10 | 20 | 0.665 |

Table 3.1 – Summary of best parameters found for each model with the corresponding F1 scores

As we can see from the table, the LightGBM models performed much better than the scikit-learn random forest. Both DART models achieved nearly the same F1 score, with GOSS having a slightly lower F1 score (the table values are rounded to 3 digits).

This concludes our end-to-end example of exploring a dataset and building an optimized model for the dataset (using parameter grid search). We look at more complicated datasets in the coming chapters and delve deeper into analyzing model performance.

## Summary

This chapter introduced LightGBM as a library to train boosted machines efficiently. We looked at where the complexity of building GBDTs comes from and the features in LightGBM that address them, such as histogram-based sampling, feature bundling, and GOSS. We also reviewed LightGBM's most important hyperparameters.

We also gave a detailed overview of using LightGBM in Python, covering both the LightGBM Python API and the scikit-learn API. We then built our first tuned models using LightGBM to predict student academic performance, utilizing cross-validation and grid-search-based parameter optimization.

In the next chapter, we compare LightGBM against another popular gradient-boosting library, XGBoost, and DL techniques for tabular data.

## References

- [1] G. Ke, Q. Meng, T. Finley, T. Wang. W. Chen, W. Ma, Q. Ye and T.-Y. Liu, “LightGBM: A Highly Efficient Gradient Boosting Decision Tree,” in Advances in Neural Information Processing Systems, 2017.

- [2] M. Mehta, R. Agrawal and J. Rissanen, “SLIQ: A fast scalable classifier for data mining,” in Advances in Database Technology—EDBT’96: 5th International Conference on Extending Database Technology Avignon, France, March 25-29, 1996 Proceedings 5, 1996.

- [3] J. Shafer, R. Agrawal, M. Mehta and others, “SPRINT: A scalable parallel classifier for data mining,” in Vldb, 1996.

- [4] S. Ranka and V. Singh, “CLOUDS: A decision tree classifier for large datasets,” in Proceedings of the 4th Knowledge Discovery and Data Mining Conference, 1998.

- [5] H. Shi, “Best-first decision tree learning,” 2007.

- [6] R. Kleiman and D. Page, “Aucu: A performance metric for multi-class machine learning models,” in International Conference on Machine Learning, 2019.

- [7] V. Realinho, J. Machado, L. Baptista and M. V. Martins, Predicting student dropout and academic success, Zenodo, 2021.

## Comparing LightGBM, XGBoost, and Deep Learning

The previous chapter introduced LightGBM for building gradient-boosted decision trees (GBDTs). In this chapter, we compare LightGBM against two other methods for modeling tabular data: XGBoost, another library for building gradient-boosted trees, and deep neural networks (DNNs), a state-of-the-art machine learning technique.

We compare LightGBM, XGBoost, and DNNs on two datasets, focusing on complexity, dataset preparation, model performance, and training time.

This chapter is aimed at advanced readers, and some understanding of deep learning is required. However, the primary purpose of the chapter is not to understand XGBoost or DNNs in detail (neither technique is used in subsequent chapters). Instead, by the end of the chapter, you should have some understanding of how competitive LightGBM is within the machine-learning landscape.

The main topics are as follows:

- An overview of XGBoost
- Deep learning and TabTransformers
- Comparing LightGBM, XGBoost, and TabTransformers

## Technical requirements

The chapter includes examples and code excerpts illustrating how to train LightGBM, XGBoost, and TabTransformer models in Python. Complete examples and instructions for setting up a suitable environment for this chapter are available at https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-4.
