
"occupation": sort_none_last(list(train_data["occupation"].unique())),
"relationship": sort_none_last(list(train_data["relationship"].unique())),
"race": sort_none_last(list(train_data["race"].unique())),
"gender": sort_none_last(list(train_data["gender"].unique())),
"native_country": sort_none_last(list(train_data["native_country"].unique())),
"income_bracket": sort_none_last(list(train_data["income_bracket"].unique())),
}

The preceding code extracts unique values for each column into a list and sorts the list, putting null values last. When working with pandas DataFrames, it’s also useful to explicitly set the data type for categorical columns to category:

```
for c in CATEGORICAL_FEATURES_WITH_VOCABULARY.keys():
    for dataset in [train_data, test_data]:
        dataset[c] = dataset[c].astype('category')
        dataset[c] = dataset[c].astype('category')
```

Using our vocabulary, we can now update the values in each column to numbers representing their category (using the index in the vocabulary list as the numeric value):

```
def map_to_index(val, vocab):
    if val is None:
        return None
    return vocab.index(val)

for dataset in (train_data, test_data):
    for feature, vocab in CATEGORICAL_FEATURES_WITH_VOCABULARY.items():
        dataset[feature] = dataset[feature].map(lambda val: map_to_index(val, vocab))
```

The result is a DataFrame where all features are now numeric:

| | age | education | marital_status | hours_per_week | income_bracket |
|---|---|---|---|---|---|
| 18545 | 37 | 11 | 2 | 40 | 0 |
| 26110 | 51 | 14 | 0 | 60 | 1 |
| 21905 | 36 | 11 | 5 | 32 | 0 |
| 1496 | 32 | 1 | 0 | 43 | 1 |
| 3148 | 47 | 15 | 2 | 40 | 0 |

Our categorical features are now encoded, and we can proceed with further data cleaning.

## Missing values and duplicates

We need to check for missing values, duplicates, and outliers. We can use the following code:

```
train_data.isnull().sum()
train_data.drop_duplicates(inplace=True)
train_data.describe()
```

We drop the duplicate data, and reviewing the output of `describe` shows us there are no significant outliers. However, there are **missing values** in the dataset. LightGBM and XGBoost can deal with missing values automatically, a significant advantage of tree-based algorithms. However, for TabTransformers, we need to implement particular logic to deal with the missing values, as we’ll see next.

## Unbalanced data

The dataset is also skewed: the number of examples of each class is not balanced. We can calculate the skew using the following code:

```
counts = np.bincount(train_data["income_bracket"])
class_weight = {
    0: counts[0] / train_data.shape[0],
    1: counts[1] / train_data.shape[0]
}
```

The output shows a roughly 75%/25% skew toward the negative (0) class. One of the simplest ways of dealing with unbalanced classes (if we have binary classes) is to weigh the positive class more strongly than the negative class. Therefore, when calculating the loss, a prediction that misses the positive class has a more significant impact.

LightGBM and XGBoost both support this through the `scale_pos_weight` parameter, which can be calculated as follows:

```
scale_pos_weight = class_weight[0]/class_weight[1]
```

## Training LightGBM and XGBoost models

With the data cleaned and prepared, we can now train our models. Training the LightGBM and XGBoost models is straightforward. For LightGBM, we have the following:

```
model = lgb.LGBMClassifier(force_row_wise=True, boosting_type="gbdt",
                         scale_pos_weight=scale_pos_weight)
model = model.fit(X_train, y_train)
```

And for XGBoost, we can run the following code:

```
model = xgb.XGBClassifier(scale_pos_weight=scale_pos_weight)
model = model.fit(X_train, y_train)
```

The preceding code highlights the simplicity of working with both libraries.

## Training a TabTransformer model

We'll now build a TabTransformer model. We'll use TensorFlow's Keras to define the model based on the example code: https://keras.io/examples/structured_data/tabtransformer/.

Our dataset preparation remains mostly the same, with two key differences: we don't encode the categorical features and must handle the missing values explicitly.

We don't encode the categorical features because Keras provides a special layer to perform string lookup and conversion to a numerical value. However, we must still supply the vocabulary. The following code illustrates creating a lookup layer:

```
lookup = layers.StringLookup(
    vocabulary=vocabulary,
    mask_token=None,
    num_oov_indices=0,
    output_mode="int",
)
```

The num_oov_indices parameter is set to 0, meaning no indices are used if an out-of-vocabulary (OOV) value is encountered. Since our vocabulary is exhaustive, this is not needed. The mask_token parameter is also set to None, as we aren't masking any string inputs.

We need to supply a default value for each column in the dataset to handle missing values. Our strategy is to replace string values with a default string value, NA. We use the statistical mean for the numeric columns to fill in the missing value. The following code creates a list of the default values:

```
train_data_description = train_data.describe()
COLUMN_DEFAULTS = [
    train_data_description[feature_name]["mean"] if feature_name in
    NUMERIC_FEATURE_NAMES else ["NA"]
    for feature_name in HEADERS
]
```

The Keras code for implementing a TabTransformer model is roughly 100 lines long and is available in our GitHub repository: https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/blob/main/chapter-4/tabtransformer-census-income.ipynb.

The following code sets up the gradient optimizer and data that we can use with the TabTransformer model:

```
optimizer = tfa.optimizers.AdamW(
    learning_rate=learning_rate,
    weight_decay=weight_decay
)
model.compile(
    optimizer=optimizer,
    loss=keras.losses.BinaryCrossentropy(),
    metrics=[keras.metrics.BinaryAccuracy(name="accuracy"),
    f1_metric,
    precision_metric,
    recall_metric],
)
train_dataset = get_dataset_from_csv(
    train_data_file, batch_size, shuffle=True
)
validation_dataset = get_dataset_from_csv(
    test_data_file, batch_size
)
```

We use an AdamW optimizer with weight decay [7] with a binary cross-entropy loss function to fit the binary classification problem. We can then train and evaluate our model with the following code:

```
callback = keras.callbacks.EarlyStopping(
    monitor='loss', patience=3
)
history = model.fit(
    train_dataset,
    epochs=num_epochs,
    validation_data=validation_dataset,
    class_weight=class_weight,
    callbacks=[callback]
)
model.evaluate(validation_dataset, verbose=0)
```

We also add early stopping via the Keras callback with a patience of 3 epochs. During training and for validation, we track the accuracy and F1 score.

Training takes significantly longer than either gradient-boosting framework and requires a GPU (training on a CPU is technically possible but takes an inordinate amount of time).

We can now look at the results of the three algorithms on the Census Income dataset.

## Results

Parameter optimization was performed for all three algorithms using the grid search technique discussed in the previous chapter. The learning rate, bin size, and number of trees were optimized for the two boosting algorithms. For the TabTransformer, both parameters and aspects of the architecture must be optimized. In terms of parameters, the learning rate, weight decay, and dropout rate were optimized, while for the architecture, the number of transformer blocks and hidden layers (in the MLP) had to be chosen. The optimized parameters are available in the source code.

The following table shows the results of the validation set for the algorithms.

| Model | Training Time | Accuracy | F1 score |
|---|---|---|---|
| LightGBM GBDT | 1.05s | 84.46% | 0.71 |
| XGBoost GBDT | 5.5s | 84.44% | 0.72 |
| TabTransformer | 113.63s | 77.00% | 0.64 |

Table 4.3 – Results from training the three models on the Census Income dataset

XGBoost and LightGBM performed similarly on the dataset, reaching an accuracy of 84% and an F1 score of 0.7. The TabTransformer model performed worse, with a lower accuracy and F1 score.

Regarding training time, LightGBM was much faster than the other approaches. The LightGBM model was trained 5.23 times faster than XGBoost and 108.22 times faster than the TabTransformer. The TabTransformer was trained for 15 epochs on an 8-core P4000 GPU.

For another point of comparison and to illustrate how the TabTransformer architecture can be adapted when categorical features aren’t present, we look at solving a second problem using the three algorithms.

## Detecting credit card fraud

Our second task is detecting fraudulent transactions in a credit card transaction dataset [8]. The dataset is available at https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud. The task is a binary classification problem, with the training data transactions labeled non-fraudulent (0) and fraudulent (1). The dataset consists only of numerical features that have been anonymized for confidentiality. Notably, the dataset is highly unbalanced, with fraudulent transactions making up only 0.17% of the data.

## Training LightGBM and XGBoost models

Since the values are all numeric, very little data preparation is required for the gradient-boosting models. To counteract the imbalance in the dataset, we again calculate `scale_pos_weight` and pass it to the model as a parameter. We perform a grid search with cross-validation to find good
