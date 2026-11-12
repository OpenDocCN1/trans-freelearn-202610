the p-value. This can be often very subtle, and can manifest itself through non-random sampling, data transformations from one level to another, keeping missing values, or deleting or imputing them. All of the decisions made by the modeler prior to applying the statistical test will affect the p-value.

Having said that, p-values are still useful for identifying possible significant predictors, and for understanding if the data fits the model. But it should be treated as only one tool, just as each algorithm you use is only one tool of many which you will use to help reveal the truth. Other tools we have demonstrated (such as confidence intervals, random sampling, and variable influence plots) should be used along with the calculation of p-values to present a complete picture of the data.

Here is an important statement from the American Statistical Association on the usage of p-values (AMERICAN STATISTICAL ASSOCIATION RELEASES STATEMENT ON, 2016):

+   - P-values can indicate how incompatible the data is with a specified statistical model
+   - P-values do not measure the probability that the studied hypothesis is true, or the probability that the data was produced by random chance alone
+   - Scientific conclusions and business or policy decisions should not be based only on whether a p-value passes a specific threshold
+   - Proper inference requires full reporting and transparency
+   - A p-value, or statistical significance, does not measure the size of an effect or the importance of a result
+   - By itself, a p-value does not provide a good measure of evidence regarding a model or hypothesis

# P-values and effect sizes

There has always been this concern regarding p-values with small samples, however using data with large samples can also yield bad results, in that the p-value may be correct, but the magnitude of the change (the effect) is very small.