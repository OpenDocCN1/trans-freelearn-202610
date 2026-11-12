the data. In this analysis, we look at the distribution of errors for all four combinations of the following variables:

+   - Outcome
+   - Correct prediction flag
+   - Average error as grouped into categories via the errbin variable:

```
result2 <- sql("select outcome,correct,errbin, count(*) as k, avg
```

# Plotting the data

Once the query has completed, issue a display command so that we can perform some visualizations. Â

```
display(result2)
```

+   1. Once the display has run, select the "Plot Options" icon (third icon bottom left), and select a Stacked bar chart plot from the plot menu.
+   2. Drag k (counts) to the Values box.
+   3. Drag outcome and correct to the Series grouping box.

+   4. Click Apply to update the visualization:

+   - As can be seen from the plot below, The lowest (and best) error for a predictor class was for the non-diabetic patients (bars 1-5, Outcome 0, Prediction N).
+   - The highest error can be seen for cases in which diabetes was predicted, but the patient was, in fact, nondiabetic (bars 6-11, Outcome 0, Prediction Y).
+   - A critical category, undetected diabetes (bars 12-16, outcome=1, prediction N) also had a relatively low error: