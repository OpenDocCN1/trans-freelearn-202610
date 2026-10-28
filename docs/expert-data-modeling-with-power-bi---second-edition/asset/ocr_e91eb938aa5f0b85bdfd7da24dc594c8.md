the low-cardinality data. Therefore, the columns with lower cardinality have fewer (or no) unique values.

By using the **Column distribution** feature, we can decide whether we load a column into the model or not. Removing unnecessary columns can potentially help us with file-size optimization and performance tuning.

To enable the **Column distribution** feature, click the corresponding feature from the **View** tab, as shown in the following image:

![](bbox=[0.178, 0.326, 0.823, 0.41])

Figure 3.30: Enabling the Column distribution feature from Power Query Editor

After enabling this feature, a new box is added under the **Column quality** box visualizing the column distribution. If you hover over the **Distribution Box**, a flyout menu shows some more information about the column distribution, as depicted in the following image:

![](bbox=[0.178, 0.603, 0.823, 0.876])