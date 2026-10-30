\(\frac{h_F(w)}{h_G(w)} = \theta\)

Statisticians who routinely analyze censored data will recognize this as the family of proportional hazards. The Lehmann family and the proportional hazards family are identical. Cox (1972, 1975), in two key papers, developed the idea of partial likelihood and showed how it can be used to estimate θ. His work led the way to Cox (or proportional hazards) regression models, which have become the primary engine for modeling censored data. The PHREG procedure in SAS implements this methodology and provides an easily accessible venue for parameter estimation for the Lehmann family.

Although familiarity with the proportional hazards models and PROC PHREG might help you understand this chapter, it also might lead to confusion. Because proportional hazards models are almost exclusively used with censored data, it is easy to mistakenly assume that this chapter describes how to obtain ROC curves for censored data. In fact, neither the proportional hazards family nor the method of partial likelihood requires that the data be censored. The data sets in this chapter are actually identical in structure to the ones in Chapters 3 and 4: continuous markers and a binary gold standard. The goal is to provide an alternative to the binormal model, which has a full complement of regression models available.

The principle behind the regression versions of the binormal model developed in Chapter 4 was to use the marker value as the dependent variable and the gold standard as the independent variable in a regression model. We will use the same idea here. In the context of proportional hazards regression, this means

\(h_F(w; D) = h_G(w) \exp\{\beta D\}\)

PROC PHREG produces an estimate for β and its standard error. We can use these to obtain an estimate of θ and its standard error. Remember that W is the marker and D is the binary outcome. Therefore, if D=0, then h_F=h_G and if D=1, then h_F=h_G e^β. Rearranging this gives the following equation:

\(\frac{h_F(w)}{h_G(w)} = e^{\beta}\)

Comparing this with \(\frac{h_F(w)}{h_G(w)} = \theta\) gives the following:

\(\hat{\theta} = e^{\beta}\)

You can use Taylor’s theorem to approximate the variance of \(\hat{\theta}\) as follows:

\(V(\hat{\theta}) = \hat{\theta}^2 V(\hat{\beta})\)

Both \(\hat{\beta}\) and \(V(\hat{\beta})\) are available from the PROC PHREG output.