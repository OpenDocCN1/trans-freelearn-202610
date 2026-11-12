```
algorithm => 'lightgbm',
search => 'random',
search_args => '{ "n_iter": 100 }',
search_params => '{
	"learning_rate": [ 0.001, 0.1, 0.5 ],
	"n_estimators": [ 20, 100, 200 ]
}',
);
```

# Preprocessing

PostgresML also supports performing certain types of preprocessing when training a model. As with the training data and configuration, the preprocessing is also stored with the project, so the same preprocessing can be applied when using a model for predictions.

Regarding pre-processing, PostgresML supports encoding categorical variables, imputing missing values, and scaling numerical values. Preprocessing rules are set using a JSON object via the preprocess parameter, as follows:

```
SELECT pgml.train(
...
preprocess => '{
	"model": {"encode": {"ordinal": ["Ford", "Kia",
	"Volkswagen"]}},
	"price": {"impute": "mean", scale: "standard"}
	"fuel_economy": {"scale": "standard"}
}',
);
```

Here, we applied ordinal encoding for the model feature. Alternatively, PostgresML also supports one-hot encoding and target encoding. We also imported missing values (as indicated by NULL in the column) using the mean of the price and applied standard (normal) scaling to the price and the fuel economy features.

# Deploying and prediction

PostgresML automatically calculates appropriate metrics on the test set after training, including R², the F1 score, precision, recall, ROC_AUC, accuracy and log loss. PostgresML will then automatically deploy the model after training if the key metric for the model (R² for regression and F1 for classification) improves over the currently deployed model.

However, deployments can also be managed manually for a project with the pgml.deploy function:

```
pgml.deploy(
project_name TEXT,
```