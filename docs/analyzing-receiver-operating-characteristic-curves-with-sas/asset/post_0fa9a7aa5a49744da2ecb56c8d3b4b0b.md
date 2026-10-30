
## Figure 7.1 The ROC Curve at 3 Years for the Predictive Power of SUV in Lung Cancer

![](img/ac26020abd4f22abfaafc0607939c6e4_97_0.png)

The AUC for the ROC curve in Figure 7.1 is 0.657 and suggests a moderate level of accuracy. The curve itself is never too far from the diagonal line, supporting the same conclusion. Of course, the ROC curve should be evaluated in the context of competing methods of prediction. In survival analysis, making accurate predictions is much more difficult because the event being predicted, in some cases, is several years away, during which many other things can happen to the patient. From this perspective, an AUC of 0.657 represents a respectable level of accuracy.

It is obvious that the choice of time point can influence the conclusions. In some cases, investigators have a clear target time point. Other studies lack such clarity and may pose a problem to the statistician in choosing the time point. In these cases, try out a few time points and present the results simultaneously, as in Figure 7.2.

## Figure 7.2 The ROC Curves at 2, 3, and 4 Years for the Predictive Power of SUV in Lung Cancer

![](img/ac26020abd4f22abfaafc0607939c6e4_98_0.png)

To generate a display like Figure 7.2, you need to understand how the output data sets are named by the %TDROC macro. The data set name in macro language is &&DSN_&TIMEPT, so for the lung cancer example, the data sets for the three ROC curves in Figure 7.2 are named LUNG_24, LUNG_36, and LUNG_48.

In general, the three curves have similar shapes, although sensitivity seems to increase with time at high levels of specificity and decrease with time at low levels of specificity. The AUCs of the three curves are 0.618, 0.657, and 0.729, suggesting that the SUV is better able to predict the status of patients at later years than earlier years. On the other hand, most of the difference between the curves is in the region where specificity is less than 0.5 and hence the difference in AUCs may be immaterial from a practical perspective.

## 7.4 Concordance Probability with Censored Data

Section 7.3 explained how to construct ROC curves with censored data. The principle idea is to dichotomize the time-to-event outcome at a given time point. As a result, the ROC curve is defined for a specific point in time. To get an idea about the overall predictive value of a marker, you need to perform an analysis like the one presented in Figure 7.2.

This section discusses an alternative method to assess the overall value of a marker in predicting a censored outcome. This alternative approach is based on the idea of concordance. In Chapter 3, we saw that, for a binary outcome, the area under the empirical ROC curve is equivalent to the concordance probability. As a reminder, the concordance probability is defined on a pair of subjects where one of the pair has the outcome and the other does not. The probability that the subject with the outcome has a greater marker value than the other subject is called the concordance probability.

The following way to express concordance probability is consistent with this definition and also makes it amenable to extend this definition to censored outcomes. Define

```
ψ(W_i, W_j) = { 1    W_i > W_j
                  0.5  W_i = W_j
                  0    W_i < W_j }
```

Hence ψ indicates which member of the pair has the higher value, with ties indicated by 0.5. Suppose there are n patients with the outcome and m patients without the outcome. Then, there are a total of mn pairs and the concordance probability can be written as follows:

```
1/(mn) Σ_{i=1}^n Σ_{j=1}^m ψ(W_i, W_j)
```

The summation represents the number of pairs that have W_i > W_j (with an accommodation for ties), so the entire expression is the fraction of patient pairs where the one with the higher marker value had the outcome.

You can use the idea of concordance in time-to-event settings. To see how the definition of concordance can be adopted for censored outcomes, let T_i and T_j be the event times in a given pair of patients with marker values W_1 and W_2. The concordance between a marker W and the censored outcome T is defined as

```
CP(W, T) = P(T_1 > T_2 | W_1 > W_2)
```

Because T is subject to censoring, this estimator cannot be used since the outcome is partially observed.

Harrell et al. (1982) suggest a modification of this estimator that can be used with censored data. This method is based on the realization that even in the presence of censoring, the outcome in some of the pairs can be ordered. For example, if the second subject is dead with a survival time that is shorter than the follow-up time of the first subject who is alive, we can say with certainty that T_1 > T_2. Table 7.1 lists all the possibilities and the corresponding value of ψ(W_1, W_2). If ψ(W_1, W_2)=1, it indicates that the prediction and the outcome are concordant. If ψ(W_1, W_2)=0, it indicates that the prediction and the outcome are discordant. Finally, (W_1, W_2)=? means that, due to censoring, it is not possible to determine whether the prediction and the outcome are concordant or discordant.

## Table 7.1 All Possible Pairings and Concordance Status with Censored Data

| Subject 1 | Subject 2 | T₁ and T₂ | Ψ(W₁,W₂) |
|---|---|---|---|
| Event | Event | T₁>T₂ | 1 |
| Event | No Event | T₁>T₂ | ? |
| No Event | Event | T₁>T₂ | 1 |
| No Event | No Event | T₁>T₂ | ? |
| Event | Event | T₁<T₂ | 0 |
| Event | No Event | T₁<T₂ | 0 |
| No Event | Event | T₁<T₂ | ? |
| No Event | No Event | T₁>T₂ | ? |

Now we can evaluate all the pairs to see whether they are concordant or not using the definitions in Table 1. The denominator is the number of informative pairs (that is the pairs for which we can make a decision), indicated in the following equation by k. The notation (i,j)=1 to (i,j)=k means that the summation is over pairs of (i,j) and there are k such pairs. The resulting estimator of concordance probability is often called the c-index.

```
c = 1/k Σ_{(i,j)=1}^k ψ(W_i, W_j)
```

The c-index has several advantages. It mimics the computation of the area under the ROC curve with binary data, it is intuitive, and it is conceptually simple to compute. Despite the conceptual simplicity, the computation requires forming all pairs and can take a long time. Computation of the c-index is not built into existing SAS procedures, but the %CINDEX macro, available from this book’s Web site, can be used to compute it, as follows:

```
%CINDEX (DSN=LUNG, PRED=SUV, SURV=SURVIVAL, CENS=CENS);
```

Here, DSN is the name of the data set, Pred is the variable in the data set to be used as the predictor, Surv is the survival time variable, and Cens is the censoring indicator (1 for censored, 0 for event). Output 7.4 simply reports the c-index.

## Output 7.4

```
cindex
--------
0.692708
```

In the case of the lung cancer example, the concordance probability is estimated to be 0.69, using the c-index as the estimator.

If the question of interest centers around multiple markers or a multivariable predictive model, then the input macro variable PRED of the %CINDEX macro should point to the variable containing the predictions of the multivariate model.

Despite all its advantages, the c-index has one big drawback. It is not a consistent estimator of the true concordance probability. A consistent estimate gets closer to the true value as the sample size increases. However, the c-index can remain far away from the true value even as the sample size increases. Therefore, the c-index can give “wrong” answers even in large samples. In my experience, if the censoring rate is low (which implies that the proportion of undecided pairs is low), the c-index works reasonably well. As the censoring rate increases, its performance becomes unpredictable.

The reason for the inconsistency of the c-index, at least intuitively, is that it uses the observed survival or follow-up times. Unless a parametric model is adopted, procedures using the observed values do not provide consistent estimates. For this reason, most of the non-parametric or semi-parametric methods of analyzing censored data, such as the Kaplan-Meier estimate or the proportional hazards regression, use the ranks of the data, not their actual values.

Before we finish this section, note an interesting property of the concordance probability:

CP(W,T)=1−CP(−W,T)

This equation tells us that if a marker is known to have high values when the outcome is unlikely, then CP can be less than 0.5. The more informative measure in this case is 1−CP, which can be taken as a measure of the marker to correctly order event times.

## 7.5 Concordance Probability and the Cox Model

There is an alternative way of computing the concordance probability in a very important special case. Most cases involving regression with censored data, such as modeling the relationship between a continuous covariate and a censored outcome or between multiple covariates and a censored outcome, are handled within the framework of the Cox model (proportional hazards regression). It turns out that concordance probability can be computed analytically in the context of the Cox model. The following development is from Gönen and Heller (2005).
