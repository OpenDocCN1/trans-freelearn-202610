Running the preceding code shows us there are no missing values for this dataset.

To locate duplicates, we can run the following code:

```
df.loc[df.duplicated()]
```

There are also no duplicates in the dataset. If there were any duplicated data, we would drop the extra rows.

We also need to check the distribution of the target class to ensure it is balanced. Here, we show a histogram that indicates the target class distribution. We create the histogram using Seaborn's countplot() method, like so:

```
sns.countplot(data=df, x='Target')
```

![](bbox=[0.285, 0.352, 0.745, 0.652])

Figure 3.5 – Distribution of target class in the academic success dataset

Although not perfectly balanced, the target distribution is not overly skewed to any one class, and we don't have to perform any compensating action.

So far, we have found that our dataset is suitable for modeling (we still need to remap Target) and clean (it does not contain missing or duplicated values and is well balanced). We can now take a deeper look at some features, starting with feature correlation. The following code plots a correlation heatmap. Pairwise Pearson correlations are calculated using df.corr(). The screenshot that follows the snippet shows a correlation heatmap built using pairwise Pearson correlations:

```
sns.heatmap(df.corr(), cmap='coolwarm')
```