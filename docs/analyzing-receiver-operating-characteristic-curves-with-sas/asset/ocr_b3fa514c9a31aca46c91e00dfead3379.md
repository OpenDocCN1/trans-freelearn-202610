# Output 3.3

```
The ROC Macro

        ROC Curve Areas and 95%
            Confidence Intervals
        ROC Area Std Error Confidence Limits
SUV     0.8713    0.0263    0.8198    0.9228
```

Frequent ROC curve users should be familiar with this macro. If, on the other hand, you only occasionally need an AUC estimate and its standard error, then it may be easier to work with PROC FREQ.

# 3.5 Selecting an Optimal Threshold

In certain cases, although the original predictions are continuous, it is of interest to report a binary prediction. Recall the rain prediction example used at the beginning of this chapter. Suppose you have a model that provides a predicted probability of precipitation for a given day. Along with this predicted probability, you might want to display an icon that depicts rain to communicate the prediction in simpler terms. Most of us have seen such icons in television or newspaper weather forecasts. By including this icon we are essentially reporting a binary prediction (rain/no rain) based on a continuous predictor (predicted probability of rain). This is equivalent to choosing a threshold, which itself is equivalent to choosing an operating point on the ROC curve.

Sometimes external criteria may guide the choice of an operating point. In the absence of such criteria, you might choose a threshold that is optimal in some sense. There are two widely used ways of doing so:

+   1. Choose the threshold that will make the resulting binary prediction as close to a perfect predictor as possible.

+   2. Choose the threshold that will make the resulting binary prediction as far way from a noninformative predictor as possible.

To understand these methods better, remember that a perfect predictor has a single point on its ROC curve (namely, the upper left corner of the unit square) with 100% sensitivity and 100% specificity. In a similar vein, a noninformative marker’s ROC curve lies along the diagonal of the unit square. The distance between a point on the ROC curve and these reference points is measured using the so-called Euclidean method. The Euclidean distance between points A and B with coordinates (x₁, y₁) and (x₂, y₂) is given as follows:

```
d_AB = √(x₁ − x₂)² + (y₁ − y₂)²
```

To make this clear, the empirical ROC curve is plotted again in Figure 3.5 with two candidate optimal operating points, A and B. R denotes the reference point for the first method of choosing an optimal threshold. Similarly, P and Q denote the reference points for the second method. According to the first definition, you need to compute the length of line segments RA and RB and prefer A over B if RA is shorter than RB. Similarly, if the second method of choosing a threshold is adapted, then the lengths of AP and BQ need to be compared, with A preferred over B if AP is longer than BQ.