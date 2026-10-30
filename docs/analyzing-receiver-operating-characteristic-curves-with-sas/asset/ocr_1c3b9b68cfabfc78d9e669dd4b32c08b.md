# Ordinal Predictors

![](bbox=[0.635, 0.085, 0.885, 0.315])

+   5.1 Introduction ......................................................... 53
+   5.2 Credit Rating Example................................................... 54
+   5.3 Empirical ROC Curve for Ordinal Predictors............................... 54
+   5.4 Area under the Empirical ROC Curve ..................................... 56
+   5.5 Latent Variable Model .................................................. 57
+   5.6 Comparing ROC Curves for Ordinal Markers ............................... 63

# 5.1 Introduction

Chapter 3 addressed the single continuous predictor, probably the most natural and common setting for ROC curves. Nevertheless, the idea of an ROC curve applies equally well to an ordinal predictor. This chapter investigates how techniques from the previous chapter can be extended to accommodate an ordinal predictor.

Ordinal predictors can arise in a variety of ways. Sometimes, they are simply the result of categorization of an underlying continuous predictor. While this may result in the loss of information, and hence predictive accuracy, gains in having a simple and easy-to-communicate predictor may offset these losses. Rain forecasts might offer an example. Sometimes they are reported as low-medium-high, and other times they are reported in increments of 10%. Regardless of the terminology used to report the categorized predictors, data analysis can proceed using the predictors as ordinal.

Ordinal predictions also can arise through expert opinions. Most subjective opinion is hard to quantify, but it is relatively easier to offer a few scenarios ordered according to their probability—in effect, reporting ordinal predictions. Examples can be found in practically any field, such as medicine (diagnostic radiologists reporting their assessment of the chance or severity of disease), business (consultants opining on the likelihood of consumer preferences), or finance (stock picks, reported as buy, stay, or sell).