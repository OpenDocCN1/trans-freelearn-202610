
## Splitting delimiter-separated strings

As mentioned previously, the Type_of_Loan feature is a comma-separated list of the types of loans the individual has. Although we could approach the problem in multiple ways, the most helpful technique is to parse the fields and build a Boolean array of columns indicating which loans an individual has.

There are eight unique types of loans and a specific category if the loan type is unspecified. These are Auto Loan, Credit-Builder Loan, Debt Consolidation Loan, Home Equity Loan, Mortgage Loan, Payday Loan, Personal Loan, and Student Loan.

Our encoding strategy would then process the feature as follows. We create nine new columns (one per loan type and unspecified) and set the column to true if the individual has that loan type. For example, here we have three conjoined loan descriptions:

```
"Home Equity Loan, and Payday Loan"
"Payday Loan, Personal Loan"
"Student Loan, Auto Loan, and Debt Consolidation Loan"
```

Table 6.3 shows the encoding results for these examples, where a true flag is set if the individual has that type of loan.

| Auto | Credit-Builder | Debt Cons. | Home Equity | Mortgage | Payday | Personal | Student | Unspecified |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| F | F | F | T | F | T | F | F | F |
| F | F | F | F | F | T | T | F | F |
| T | F | T | F | F | F | F | T | F |

Table 6.3 – Loan type columns encode customers’ types of loans

We can utilize pandas’ String utilities to accomplish the preceding encoding, as in this example:

```
frame["auto_loan"] = frame["Type_of_Loan"].str.lower().str.contains("auto loan").astype(bool)
```

## Outliers and missing values

We follow the same general strategy as before: we impute missing values using descriptive statistics and, where able, set outlier values to boundaries we define using domain knowledge.

Duplicate rows are dropped.

In terms of outliers, the age, number of bank accounts, and monthly balance features have outlying values (which we confirm using the mean and standard deviation and distribution plots). We set the outlying values to the upper bound for these features:

```
frame.loc[frame["Age"] > 65, "Age"] = 65
frame.loc[frame["Num_Bank_Accounts"] > 1000, "Num_Bank_Accounts"] = 1000
frame.loc[frame["Monthly_Balance"] > 1e6, "Monthly_Balance"] = np.nan
```

After our data cleaning, we can verify that all features have the correct type and that missing values have been handled:

```
train_df.info()
train_df.isnull().sum()
train_df[train_df.duplicated()]
```

We can proceed with a more thorough exploratory analysis with the clean dataset.

## EDA

Next, we highlight some of the patterns found during our EDA.

As described in the problem description, we need to validate whether any potential bias exists in the data.

We start by visualizing the customers’ ages:

```
sns.histplot(train_df["Age"], bins=20)
```

![](img/31b70e4a77c3afbe11032f68ab874b02_146_0.png)

Figure 6.8 – Histogram showing the count of customers by age

Figure 6.8 shows that all age groups are represented, with data mainly normally distributed around the middle-aged.

We also check monthly income. A lack of data in lower income brackets could indicate that minorities were excluded:

```
sns.histplot(train_df["Monthly_Inhand_Salary"], bins=30)
```

![](img/31b70e4a77c3afbe11032f68ab874b02_147_0.png)

Figure 6.9 – Histogram showing the count of customers by monthly in-hand salary

As seen in Figure 6.9, monthly income follows an expected distribution and lower income brackets are well represented.

We again visualize the correlation heatmap for the numeric features to highlight and direct correlations:

![](img/31b70e4a77c3afbe11032f68ab874b02_148_0.png)

Figure 6.10 – Correlation heatmap for the Credit Score dataset

Two strong correlations are notable: monthly balance and monthly salary, and outstanding debt and delay in due date. Further visualization of these correlations indicates that a customer’s monthly balance increases with salary and that poor credit scores are associated with lower balances and salaries.

A similar correlation exists between outstanding debt and credit score: an increase in debt is associated with poor credit scores, and vice versa. The analysis confirms that our model should be able to capture both of these correlations.

Finally, and importantly, we also have to check the class distribution for the dataset:

```
sns.histplot(train_df["Credit_Score"], bins=30)
```

![](img/31b70e4a77c3afbe11032f68ab874b02_149_0.png)

Figure 6.11 – Histogram of the class distribution for the credit scoring dataset

The class distribution for our dataset is shown in Figure 6.11. As we can see in the figure, there is a significant class imbalance. The imbalance has to be addressed before we can proceed with modeling.

## Modeling

As before, we proceed with model selection first.

### Model selection

Due to the dataset's size and complexity, a linear model is not expected to perform very well. As such, we use a regular decision tree as a baseline model and include a random forest for comparison.

### Model training and evaluation

We are almost ready to train our models, but one issue remains: we must address the class imbalance.

### Handling class imbalance

Class imbalance potentially biases any trained models to the majority class or classes. Although tree-based algorithms are better at dealing with imbalanced classes than most other learning algorithms, it's still best practice to address class imbalance before modeling.

Generally, the following strategies may be employed to deal with imbalanced classes:

- Resampling techniques:
  - **Oversampling**: This involves increasing the number of samples in the minority class to match the majority class. One common technique is the **Synthetic Minority Over-sampling Technique (SMOTE)**, where new samples are created based on the existing ones.
  - **Undersampling**: This involves reducing the number of samples in the majority class to match the minority class. One risk with this approach is the loss of potentially valuable data.
  - **Cost-sensitive learning**: This method assigns higher costs to the misclassification of the minority class during the training process. The idea is to make the model pay more attention to the minority class. LightGBM supports this through the `class_weight` (for multi-class) and `scale_pos_weight` (for binary classes) parameters. Examples of applying these parameters are given in *Chapter 4, Comparing LightGBM, XGBoost, and Deep Learning*.
  - **Data augmentation**: This involves creating new instances in the dataset by adding small perturbations to existing instances. This method is prevalent in image classification tasks.
  - **Use of appropriate evaluation metrics**: Accuracy is often misleading in class imbalance. Instead, metrics such as precision, recall, F1-score, **Area under the ROC Curve (AUC-ROC)**, and confusion matrix can provide a more comprehensive view of model performance.

In our case study, we are already employing robust evaluation metrics against imbalanced classes. For this problem, we use SMOTE, an oversampling technique, to balance our classes while preserving our data.

### SMOTE

SMOTE was developed to overcome some of the shortcomings of simple oversampling of the minority class, which can lead to overfitting due to the exact duplication of instances [1]. Instead of simply duplicating minority samples, SMOTE creates synthetic, or “fake,” samples that are similar, but not identical, to existing samples in the minority class.

The SMOTE algorithm proceeds as follows. New sample points, called synthetic samples, are synthesized by choosing points between samples close to minority class samples. Specifically, for each minority class sample, SMOTE calculates the k-nearest neighbors, chooses one of these neighbors, and then multiplies the difference between the feature vectors of the sample and its chosen neighbor by a random number between 0 and 1, adding this to the original sample to create a new, synthetic sample.

By creating synthetic examples, SMOTE presents a more robust solution to the imbalance problem, encouraging the model to draw more generalizable decision boundaries. It’s important to note, however, that while SMOTE can improve the performance of models on imbalanced datasets, it’s not always the best choice. For instance, it can introduce noise if the minority samples are not sufficiently close in the feature space, leading to overlapping classes. As with all sampling techniques, it’s essential to
