which, when substituted in $y = x^{\theta(u)}$, gives the age-adjusted ROC curve:

$y = x^{1.288 - 0.014*AGE}$

You can obtain a graphical representation by plotting the ROC curve for a few selected values of age (see Figure 6.4). Each ROC curve in Figure 6.4 is a plot of $y = x^{1.288 - 0.014*AGE}$ for a different age value. The lowest curve represents 70-year-old patients and the highest curve represents 40-year-old patients. The curves in between represent varying levels of age in 5-year increments. As anticipated from the negative sign of $\beta_3$, the ROC curves get closer and closer to the diagonal line as age increases.

# Figure 6.4 ROC Curves for Different Age Values

![](bbox=[0.17, 0.32, 0.77, 0.66])

It is also possible to answer the question of whether a covariate adjustment is needed. Notice that when $\beta_3=0$, the adjusted and the unadjusted ROC curves coincide. Therefore, a test of $\beta_3=0$ is one way of answering whether you should report adjusted or unadjusted ROC curves. The $p$-value for this test is 0.0426 (see Output 6.2); thus, based on this analysis, an age-adjusted ROC curve is appropriate.

# 6.5 Using Estimating Equations to Handle Clustered Data

All the methods described so far require individual data points to be independent of each other. This requirement is usually satisfied when a single observation is made for each individual unit. Here, an individual unit can be a patient awaiting a diagnosis, a debtor applying for a loan, or a calendar day for which a weather forecast will be issued.