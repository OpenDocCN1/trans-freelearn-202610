Using the relationship

```
$$AUC = \frac{D + 1}{2}$$
```

you can compute the area under the curve to be 0.8713, identical to the LOGISTIC output, which was reported with three significant digits. Also using the relationship

```
$$se(AUC) = \frac{se(D)}{2}$$
```

the standard error of AUC is computed to be 0.026. One can compute a confidence interval for AUC using asymptotic normal theory

```
$$AUC + 1.96 * se(AUC)$$
```

which turns out to be (0.8203, 0.9222). This is a more complete analysis than the one afforded by PROC LOGISTIC because you can judge the effects of sampling variability on the estimate of AUC.

# 3.4.3 The %ROC Macro

While PROC FREQ is sufficient for our purposes to compute the area under the curve and its standard error, now is a good time to introduce a SAS macro primarily used to compare the areas under several ROC curves; however, it can also handle a single ROC curve as a special case. The %ROC macro, currently in Version 1.5, is available for download at the following URL:

support.sas.com/samples_app/00/sample00520_6_roc.sas.txt

Following is the code that can be used for this example:

```
%roc(data=HeadNeck,var=SUV,response=gold,contrast=1,details=no,
    alpha=.05);
```

The macro variables used for invoking %ROC are

+   - Response: This is the gold standard, or the outcome. You need to create a data set variable that takes on values of 0 or 1 only; the macro can not recode the observed binary classes. This variable must be numeric; a character variable with only two distinct values (0 and 1) will pass the macro’s error-checking facility but produce a PROC IML error, which is difficult to debug.

+   - Var specifies the marker.

+   - Contrast is used primarily to compare several curves, as we will see in later chapters. The value of Contrast is always 1 for a single ROC curve.

+   - Alpha pertains to confidence limit estimation.

+   - Details controls the amount of output printed.

Output 3.3 shows the results of this call. The area under the curve is 87.13%, the same as the concordance from PROC LOGISTC, and also the same as derived from Somers’ D in PROC FREQ. The standard error is also the same as the standard error derived from Somers’ D in PROC FREQ. Confidence intervals also match up, up to rounding error.