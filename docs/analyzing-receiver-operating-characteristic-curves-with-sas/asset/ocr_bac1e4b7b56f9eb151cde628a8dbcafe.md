# Output 8.2

```
The ROC Macro

        ROC Curve Areas and 95%
            Confidence Intervals
    ROC Area Std Error Confidence Limits

P_1    0.6767    0.0414    0.5957    0.7578
```

Note that P_1 is the variable name assigned to the predictions by the SCORE statement.

It is also possible to plot the split-sample ROC curve using the %PLOTROC macro introduced in Chapter 3. Since P_1 is given to several significant digits, a new variable P1 is created in the data set TESTSCORE to facilitate printing of thresholds. P1 is created as follows:

```
P1 = ROUND(P_1,0.01);
```

Then the following invocation of %PLOTROC is used:

```
%PLOTROC (dsn=testscore, makrer=p1, gold=complications, anno=4,
                tlist=0.45 0.51 0.58);
```

# Figure 8.2 Split-Sample Estimate of the ROC Curve Using the %PLOTROC Macro

![](bbox=[0.17, 0.49, 0.63, 0.77])

Note that thresholds in Figure 8.2 are described in terms of predicted probabilities because the marker plotted is the predicted probability from the logistic model (see MARKER= in the %PLOTROC macro call).

As an alternative to the %PLOTROC macro, you can use the OUTROC= option of the SCORE statement, which generates a data set that is identical in structure to the OUTROC= option in the MODEL statement. Specifically, it contains the variables _SENSIT_ and _1MSPEC_, which can be used in a PLOT statement of a call to PROC GPLOT.