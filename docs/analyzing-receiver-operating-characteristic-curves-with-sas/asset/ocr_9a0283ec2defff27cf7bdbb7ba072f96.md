# Output 6.4

Analysis of Maximum Likelihood Estimates
Variable DF Parameter Estimate Standard Error StdErr Ratio Chi-Square Pr > ChiSq Hazard Ratio 95% Hazard Ratio Confidence Limits
grp 1 1.28789 0.89378 2.003 2.0763 0.1496 3.625 0.629 20.898
Age 1 -0.00297 0.00881 2.758 0.1137 0.7359 0.997 0.980 1.014
grpage 1 -0.01432 0.01337 1.894 1.1459 0.2844 0.986 0.960 1.012

There is an increase of 89.4% in the standard error of β₃. This results in a p-value of 0.284, much larger when compared with the p-value from the partial likelihood analysis. We conclude that, after adjusting for clustering, there is no need for further adjustment on age.

# 6.6 Comparing Markers Using the Lehmann Family of ROC Curves

Chapter 5 explored the idea of using a regression model to compare two or more ROC curves. The principal idea was to include an indicator variable for the type of marker in a regression model. For the case of the binormal model, the appropriate regression model was a heteroscedastic normal regression. For the Lehmann family, the appropriate regression model is a proportional hazards regression, as we have already seen in Section 6.3.

Specifically, let U be an indicator variable that takes on values of 0 and 1 for the two different markers or predictors. Then, following the logic from Section 6.3, you can write the following equation:

```
h(V | U, D = 1) / h(V | U, D = 0) = exp{β₁ + β₃U}
```

This model is similar to those in Chapters 4 and 5 in the context of the binormal model. It is also identical in form to the one considered in Section 6.2, where U represents age, a continuous covariate. Therefore, the PHREG statements are also identical and are not shown here again.

If β₃ = 0 then the two ROC curves are identical. Therefore, a formal comparison can be done by testing H₀ : β₃ = 0. The Wald test readily available in PROC PHREG output can be used for this purpose. From an operational standpoint, comparing the ROC curves of two markers is no different than comparing the ROC curves for different age groups, so the steps described in Section 6.4 apply here, too.

There is a potentially important aspect in which marker comparisons may differ from covariate comparisons. As we discussed in Chapter 4, paired designs are the dominant form of data collection for most studies involving a comparison of multiple markers. The previous model, when estimated by partial likelihood, implicitly assumes that observations are independent. But Section 6.4 shows that an estimating equations approach to fitting the regression model allows you to obtain the proper standard error estimates when the data are correlated. Paired data are special cases of correlated data, and the marginal model approach outlined in the previous section can be used to fit the model in the presence of paired data.