# 5.6.2 Comparing the Areas under the Empirical ROC Curves

Just as empirical curves for an ordinal predictor can be constructed using the %ROC macro, areas under multiple empirical ROC curves can also be compared. The following call to the %ROC macro generates the desired comparison:

```
%roc(version,data=modif,var=pet ct,response=surg,contrast=1 -1);
```

Output 5.4 shows the results.

# Output 5.4

The ROC Macro

ROC Curve Areas and 95%
Confidence Intervals
ROC Area   Std Error   Confidence Limits
pet   0.6744   0.2349   0.2141   1.1347
ct    0.5526   0.2162   0.1289   0.9763

Contrast Coefficients
          pet   ct
Row1          1   -1

Tests and 95% Confidence Intervals for Contrast Rows
Estimate   Std Error   Confidence Limits      Chi-square   Pr > ChiSq
0.6744     0.2349      0.2141      1.1347      8.2461      0.0041
0.5526     0.2162      0.1289      0.9763      6.5346      0.0106

Contrast Test Results
Chi-Square   DF   Pr > ChiSq
11.6473      2    0.0030