
![](img/e28602b44ec813a47df57c96b204412c_365_0.png)

Figure 10.23: Data source data types

You can use the following steps to do this:

- Go to the data flow screen and double-click on the **Data types** box. This will open a section that lists all your features, along with the automatically inferred data type for each of those features. The right side of the screen will also display a preview of your dataset in real time.
- You can use the **Type** dropdown to change the datatype of a particular feature. Let’s do some of these before moving ahead. Here, we will change the data type of `age`, the siblings (**sibsp**), and the parents (**parch**) to Long and the `age` feature to `Float`. In my case, Data Wrangler automatically classified all these features as **String**. If you try this in your account, you may have a slightly different result. Once you pick the data type you want for the features, you can preview it using the **Preview** button.
- Once you are satisfied with the changes, you can click on the **Update** button to apply the changes.

## Categorical encoding

After dropping the features in the preceding section, we still have two features in our example dataset that have text data representing categories, the **sex** and **boat** features. We will use categorical encoding to transform the **sex** feature. Categorical encoding is a common feature engineering technique that converts categorical data, which represents a set of discrete values, into numerical data that can be used for machine learning models. Categorical encoding is necessary because most machine learning algorithms work with numerical data, and categorical data cannot be directly used for many machine learning models. There are several common techniques for categorical encoding, such as “one-hot encoding,” where each category value is converted into a new column and assigned 1 or 0 to the column. For example, the **sex** feature could be one-hot encoded to two new columns, `sex_male` and `sex_female`, with each column containing a 1 or 0 depending on the original value of the **sex** feature. Label encoding is another commonly used categorical encoding method that assigns a unique integer value to each category. For example, in our case, if we apply label encoding to the **sex** feature,

we would end up with a value of 1 and 2 for `female` and `male` values, respectively. Since we are working with numbers, a higher value can be associated with the machine learning model that has a higher positive or negative correlation to our target, which we do not want for this feature. Besides one-hot encoding and label encoding, other categorical encoding methods include count encoding, which replaces each category with the number of times it appears in the dataset, and target encoding, which replaces each category with the mean of the target variable for that category. For now, let’s apply one-hot encoding to the sex feature. We will start by adding a new transform step by clicking on the + sign next to the **Drop column** button in the data flow.

![](img/e28602b44ec813a47df57c96b204412c_366_0.png)

Figure 10.24: Data transform – Add step

Next, click the **Add step** button and choose the **ENCODE CATEGORICAL** section. Here, you can choose the type of encoding from the **Transform** dropdown. We will choose **One-hot encode**. We will specify the input column, which in our case is `sex`. Next, specify how you want to handle missing values in that column. You can either choose to keep them, skip them, or end the transform process with an error if it encounters missing values. You can preview the transformation, and once you are satisfied, you can click on the **Add** button to add the new transformation step to your flow.

![](img/e28602b44ec813a47df57c96b204412c_367_0.png)

Figure 10.25: Data Transform – one-hot encoding

Check the data flow in the visualizer to make sure everything looks as expected. You should see an additional step now for one-hot encoding.

## Custom transformation

Data Wrangler has several commonly used built-in transformations that are available out of the box, without the need to write code from scratch. We covered two of these in the previous sections – first, when we dropped redundant or irrelevant features from our dataset using the drop column transformation. The other built-in transformation we used was to apply one-hot encoding, using the categorical encoding transformation on the sex feature of the DataFrame. Besides these, there are other built-in transformations to work with text data and time-series data, handle missing and outlier data, and apply common data engineering tasks such as doing dimensionality reduction and working within balanced datasets. However, there might be scenarios where you would need to do transformations on your data outside of the built-in transformations. Data Wrangler provides you with

the ability to do this via **Custom Transformations**, where you can provide your own custom code in PySpark, PySparSQL, and Python pandas and as Python user-defined functions. It is important to note the dataset size before choosing the specific type of custom transformation code that you want to use. For example, if you choose Python (pandas), your dataset should fit in the memory of a single instance. If you have a larger dataset, choose to provide PySpark code instead, which enables you to split your data across multiple instances and aggregate the results. In our example, the Titanic dataset has a **boat** feature that contains the lifeboat number of a passenger, if they could get on one. While the lifeboat number in itself does not hold significant information, a passenger having access to a lifeboat significantly increased their chances of survival. We will use a custom transformation to generate a new feature for our dataset that specifies whether a passenger had access to a lifeboat or not. The following custom Python code snippet does exactly that. It creates a new feature, **Lifeboat Exists**, and checks whether the **boat** feature has a value, and if it does, it adds a value of 1, and if it does not, it inserts 0. Finally, it drops the existing **boat** feature, as we no longer need it:

```
```python
# Table is available as variable `df`
# Assign a target value to check, in our case '?' and use an escape as
# this is a special character
target_value = '\\?'
# Create a new column that will check if value exists for boat number
df['lifeboat_exists'] = (~df['boat'].str.contains(target_value,
na=False)).astype(int)
# Drop the existing "boat" column
df.drop('boat', axis=1, inplace=True)
```
```

Add a custom transformation in Data Wrangler using the following steps:

- 1. Go to the Data Wrangler flow to add a new transform step by clicking on the + sign next to the **Drop column** step in the data flow.
- 2. Next, click the **Add step** button and choose the **Custom Transformation** section. Here, we will give an optional name for your custom transformation step; we will call it `Lifeboat_check`.
- 3. Then, we will choose the type; since we have a relatively small dataset, we will choose Python (pandas). You will get a warning message reminding you to check the dataset size and make sure you can fit it into the memory of a single machine.
- 4. You can then paste the code from the previous section. To make sure you do not have any errors, click on the **Preview** button and ensure everything looks alright. You should see a new column created, `lifeboat_exists`, and the previous `boat` column should no longer exist.
- 5. Once you are satisfied with the results, click on the **Add** button to add this transformation as an additional step in your data flow. You can also review your data flow in the visualizer as before.

```
df['cabin_bin'] = pd.cut(pd.factorize(df['cabin'])[0], num_bins,
labels=False)
# Drop the existing "cabin" column
df.drop('cabin', axis=1, inplace=True)
```

![](img/e28602b44ec813a47df57c96b204412c_370_0.png)

You can add this transformation step by choosing a new custom transformation, similar to how we did for the lifeboat check.

## Numeric scaling

Machine learning algorithms such as linear regression, logistic regression, neural networks, and so on that use gradient descent as an optimization technique require data to be scaled. There are two ways you can scale the features in your dataset. You can either apply normalization or you can choose standardization. The decision to use normalization or standardization depends on the normalization
