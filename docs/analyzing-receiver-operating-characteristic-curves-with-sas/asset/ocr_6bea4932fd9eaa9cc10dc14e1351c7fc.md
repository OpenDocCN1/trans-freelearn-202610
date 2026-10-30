The term ROC points is used intentionally. Figure 5.1 does not have an ROC curve, only points each of which is a feasible operating point for Moody’s ratings. It is possible to connect these points, just as we did for a continuous predictor, ending up with an ROC curve. You could argue that because unobserved intermediate thresholds are not possible by definition of the ordinal predictor, the resulting curve is not meaningful. Imagine that you connected the dots in Figure 5.1. There is no Moody’s score between B2 and C, so the points on the line segment connecting the two leftmost points in the figure represent the operating characteristic (i.e., sensitivity and specificity) of thresholds that do not exist. The counterargument holds that the levels of most ordinal scales are arbitrarily defined and can be changed. If the ordinal predictions are obtained by categorizing a continuous predictor, then it is possible to explicitly revisit the thresholds. In the case of subjective opinions, experts can be asked to refine their predictions by using a scale with more levels. In our example, Moody’s might decide, after examining Figure 5.1, that the gap between Aa1 and Aa2 (the second and third points from the right) is too wide. This could be resolved by introducing a new threshold between these two.

There is also a practical consequence of connecting the points: It makes it possible to unambiguously define the area under the curve for an ordinal predictor. Thus, I recommend using the ROC curve rather than ROC points for an ordinal predictor.

# 5.4 Area under the Empirical ROC Curve

The area under the empirical ROC curve is computed using the trapezoidal rule, the same way it was done for a continuous marker. The %ROC macro can be used for this purpose since the empirical ROC curve and the area under it are rank-based functions. The macro call is identical to the ones used in Chapter 3:

```
%roc(data=moody,var=moody,response=def,contrast=1);
```

The familiar %ROC output (see Output 5.1) indicates an AUC of 0.915 with a 95% confidence interval ranging from 0.891 to 0.940.

# Output 5.1

```
The ROC Macro

ROC Curve Areas and 95%
Confidence Intervals
ROC Area Std Error Confidence Limits
moody 0.9153 0.0124 0.8909 0.9397
```

Similar to using the %ROC macro, you can use the %BOOT1AUC macro to compute bootstrap-based estimates and standard errors for the area under the curve. Output 5.2 shows the results from the bootstrap, which are similar to those produced by the %ROC macro.