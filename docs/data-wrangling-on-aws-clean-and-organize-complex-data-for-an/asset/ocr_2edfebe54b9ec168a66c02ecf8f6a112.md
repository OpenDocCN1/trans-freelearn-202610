![](bbox=[0.14, 0.07, 0.89, 0.44])

Figure 10.11: The data insights summary

Let's go over the different sections of the report, starting with the top summary. The summary includes key statistics of your dataset and the feature types. Here, we can see the total number of rows in your dataset, the number of missing rows, and the number of duplicate rows, if any. In the SUMMARY section, you can also review the number of features in your dataset and their count. If your dataset has critical errors that will significantly affect the quality of your machine learning models, they will display in the **High Priority Warnings** section of the report. An example of a high-priority warning is target leakage. Target leakage occurs when a feature is directly influenced by the target, resulting in high accuracy on the training set but poor performance on test data. To prevent target leakage, it's important to keep the target out of the training process, ensuring that we train the model only on independent features.

![](bbox=[0.14, 0.68, 0.89, 0.78])

Figure 10.12: Data insights anomalies