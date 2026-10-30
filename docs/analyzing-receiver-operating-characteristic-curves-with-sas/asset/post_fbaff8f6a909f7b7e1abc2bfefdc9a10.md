
and the form of the adjusted ROC curve follows:

y = x^{θ(u)}

where

θ(U) = β₁ + β₃U

Just as in Chapter 5, the ROC curve for the covariate is unaffected by β₂ but it is critical to include the main effect for age in the model; otherwise, the estimate of β₃, the key component for the ROC curve, might be biased.

Continuing with the magnetic resonance example, age may be an important factor in the accuracy of this type of magnetic resonance imaging, which focuses on the water-fat ratio in the tissue. Because patients naturally accumulate fat as they age, the marker may be less effective in the elderly, with the potential for mistaking fat due to age for fat due to a benign lesion.

The following PHREG call provides the required estimates:

```
proc phreg data=tmp1.ph;
    model signal=grp age grpage/r1;
    if group='normal' then grp=0;
    else if group='benign' then grp=1;
    grpage=grp*age;
run;
```

Again, note the use of programming statements, not just to create the group variable, but also the interaction. Output 6.2 contains the relevant numbers from the PHREG procedure. The three parameters are estimated to be β₁=1.29, β₂=-0.003, and β₃=-0.014. The negative sign of the coefficients for age indicate decreasing accuracy with increasing age. Output 6.2 shows the results.

## Output 6.2

| Analysis of Maximum Likelihood Estimates |
| Variable | DF | Parameter Estimate | Standard Error | Chi-Square | Pr > ChiSq | Hazard Ratio | 95% Hazard Ratio Confidence Limits |
| grp | 1 | 1.28789 | 0.44629 | 8.3276 | 0.0039 | 3.625 | 1.512 | 8.694 |
| Age | 1 | -0.00297 | 0.00320 | 0.8650 | 0.3523 | 0.997 | 0.991 | 1.003 |
| grpage | 1 | -0.01432 | 0.00706 | 4.1123 | 0.0426 | 0.986 | 0.972 | 1.000 |

Substituting β₁ and β₃ in θ(u) = β₁ + β₃U gives the following equation:

θ(AGE) = 1.288 − 0.014*AGE

which, when substituted in $y = x^{\theta(u)}$, gives the age-adjusted ROC curve:

$y = x^{1.288 - 0.014*AGE}$

You can obtain a graphical representation by plotting the ROC curve for a few selected values of age (see Figure 6.4). Each ROC curve in Figure 6.4 is a plot of $y = x^{1.288 - 0.014*AGE}$ for a different age value. The lowest curve represents 70-year-old patients and the highest curve represents 40-year-old patients. The curves in between represent varying levels of age in 5-year increments. As anticipated from the negative sign of $\beta_3$, the ROC curves get closer and closer to the diagonal line as age increases.

## Figure 6.4 ROC Curves for Different Age Values

![](img/ac26020abd4f22abfaafc0607939c6e4_85_0.png)

It is also possible to answer the question of whether a covariate adjustment is needed. Notice that when $\beta_3=0$, the adjusted and the unadjusted ROC curves coincide. Therefore, a test of $\beta_3=0$ is one way of answering whether you should report adjusted or unadjusted ROC curves. The $p$-value for this test is 0.0426 (see Output 6.2); thus, based on this analysis, an age-adjusted ROC curve is appropriate.

## 6.5 Using Estimating Equations to Handle Clustered Data

All the methods described so far require individual data points to be independent of each other. This requirement is usually satisfied when a single observation is made for each individual unit. Here, an individual unit can be a patient awaiting a diagnosis, a debtor applying for a loan, or a calendar day for which a weather forecast will be issued.

As was the case in the magnetic resonance example, sometimes more than one observation may be obtained from an individual unit. In this case, the research question centers on identifying abnormal vertebral marrow processes and classifying them as normal or benign. The radiologist performing the magnetic resonance imaging measures the signal at as many vertebrae as indicated based on the image. Some patients may contribute a single measurement while others will contribute several. When a patient contributes more than one observation to the analysis, the resulting data set is said to be *clustered*. The term is motivated by the observation that data coming from the same units behave similarly and tend to cluster together.

An intuitive way to think about clustered data is that the amount of information in two data points that are independent is generally more than the amount of information in two data points that are clustered. Clustering makes individual data points behave more like each other than they would otherwise. The net result is often an increase in standard errors (when compared with independence across data points), while point estimates are usually not substantially affected.

One can think of clustered data examples in other fields. A weather forecast for a given day is usually multi-dimensional: Temperature, precipitation, and pressure may all be simultaneously forecast. The forecasts within a given day will tend to be clustered because they are all related to various aspects of the same underlying meteorological events.

Clustered data do not always arise from multiple measurements on each observational unit. There could be other reasons to suspect that a subset of the observations behave like one another. For example, debtors in the same ZIP code may exhibit similar behaviors of default. Parts manufactured by the same factory may exhibit similar patterns of failure. All of these will have a bearing on the analysis of ROC curves.

Analysis of clustered data is a statistical topic in itself. An overview from a SAS user’s perspective can be found in *SAS for Mixed Models, Second Edition* by Littell, et al. (2006) for continuous outcomes and *Categorical Data Analysis Using the SAS System, Second Edition* by Stokes, Davis, and Koch (2001).

One method of handling clustered data is based on generalized estimating equations. This is a way of estimating the model parameters using a method that is robust to departures from the assumption of the independence of data points. This method has been very popular in analyzing correlated binary data, and it has been extended to the case of censored data. It can be fit using the COVSANDWICH option in PROC PHREG. The option is named after the sandwich estimator of the covariance, which is one of the features of the estimators obtained by estimating equations.

If we modify the call to PROC PHREG as follows, the model is fit using estimating equations instead of the usual partial likelihood. The ID statement defines the variable that identifies the clustering variable. Records sharing the same ID value are clustered.

```
proc phreg data=tpm1.ph covsandwich(aggregate);
    model signal=group/r1;
    if group='Normal' then grp=0;
    else if group='Benign' then grp=1;
    id name;
run;
```

The format of the PHREG output is not altered with the use of the COVSANDWICH option. In particular we see that θ is estimated to be 1/1.427=0.701 (identical to Section 6.3), supporting the notion that there is very little effect, if any, of clustering on point estimates. Since θ̂ did not change, the estimate of the AUC remains the same at 0.588. The standard error of β̂, on the other hand, is now 0.144, up from 0.088, an increase of 63.5%, as indicated by the StdErrRatio column, the only addition to PROC PHREG output in the presence of the COVS option. See Output 6.3. This results in an increase in the standard error of θ̂ to 0.101 and that of the AUC from 0.030 to 0.050.

## Output 6.3

| Analysis of Maximum Likelihood Estimates |
| Parameter | DF | Parameter Estimate | Standard Error | StdErr Ratio | Chi-Square | Pr > ChiSq |
| group benign | 1 | 0.35541 | 0.14413 | 1.635 | 6.0808 | 0.0137 |
| Analysis of Maximum Likelihood Estimates |
| Parameter | Hazard Ratio | 95% Hazard Ratio Confidence Limits | Variable Label |
| group benign | 1.427 | 1.076 1.892 | group benign |

Figure 6.5 adds to Figure 6.4 the confidence bands based on the estimating equations. The dotted lines both indicate 95% confidence intervals, one based on the standard error assuming independence (narrower bands) and one based on the estimating equations (wider bands). This figure is not intended to help you choose an estimation method. Whether you should use estimating equations depends on whether clustering can reasonably be expected based on the nature of the study.
