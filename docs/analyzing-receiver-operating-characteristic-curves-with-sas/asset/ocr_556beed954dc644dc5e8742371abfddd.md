From the perspective of ROC curves, there is a duality between continuous and ordinal predictors. Both the empirical curve and the binormal model can accommodate ordinal predictors, either directly or with some modification. Sections 5.3 and 5.4 provide details.

# 5.2 Credit Rating Example

Consider the following data set collected and published by Güttler (2005) on the performance of the scoring systems of two of the most prominent credit evaluation agencies in the world: Moody’s and Standard and Poor’s. Table 5.1 reports the data on Moody’s ratings, which use 17 categories on an ordinal scale ranging from Aaa (most favorable rating) to C (least favorable rating). The Rating row lists these 17 ratings, the Default row contains the number of companies that defaulted on their loans during the follow-up period of the study, and the Total row presents the total number of companies receiving the particular credit rating during the same period. The goal of this example is to evaluate the accuracy of Moody’s ratings as predictors of the likelihood of default. For each of the 17 ratings, from Aaa to C, the total number in the sample and the number defaulting out of that total are provided.

# Table 5.1 Moody's Ratings

| Rating | Aaa | Aa1 | Aa2 | A1 | A2 | A3 | A3- | Baa1 | Baa2 |
| Default | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 1 | 0 |
| Total | 49 | 47 | 120 | 164 | 163 | 254 | 226 | 238 | 242 |
| Rating | Baa3 | Ba1 | Ba2 | Ba3 | B1 | B2 | B3 | C |  |
| Default | 1 | 1 | 1 | 2 | 8 | 15 | 13 | 55 |  |
| Total | 212 | 105 | 108 | 175 | 238 | 215 | 139 | 164 |  |

Note that Table 5.1 contains all the data. With ordinal predictors, you can display an entire data set with nearly 3,000 observations because the space requirement has more to do with the number of categories than with the actual sample size. This is in contrast with continuous data, where the original data are usually not reported due to space constraints.

# 5.3 Empirical ROC Curve for Ordinal Predictors

The empirical ROC curve for an ordinal predictor is built on the same principle as the empirical ROC curve for a continuous predictor. In fact, because the empirical ROC curve is based on the ranks of the data only, whether the predictor is ordinal or continuous has no bearing on the way the ROC curves are constructed. Thus, the methods from Chapter 3 can be used to construct and compare empirical ROC curves with ordinal predictors.

To briefly review, using each distinct observed value of the ordinal predictor as a possible threshold, you can compute the sensitivity and specificity of the resulting binary predictor. A scatter plot of sensitivity versus one minus specificity for each threshold constitutes the ROC points. Note that the following code is identical to the program used in Chapter 3 for continuous predictors: