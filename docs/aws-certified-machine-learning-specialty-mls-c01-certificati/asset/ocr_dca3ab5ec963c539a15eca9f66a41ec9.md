Precision-recall curves summarize the precision versus recall trade-off by using different probability thresholds. For example, the default threshold is 0.5, where any prediction above 0.5 will be considered as true; otherwise, it is false. You can change the default threshold according to your need so that you can prioritize recall or precision. Figure 8.3 shows an example of a precision-recall curve:

![](bbox=[0.27, 0.24, 0.73, 0.48])

Figure 8.3 – A precision-recall curve

As you can see in Figure 8.3, increasing the precision will reduce the amount of recall and vice versa. Figure 8.3 shows the precision/recall for each threshold for a gradient boosting model (as shown by the orange line) compared to a no-skill model (as shown by the blue dashed line). A perfect model will approximate the curve to the point (1,1), forming a squared corner in the top right-hand side of the chart.

Another visual analysis we can do on top of confusion matrixes is known as a Receiver Operating Characteristic (ROC) curve. ROC curves summarize the trade-off between the true positive rate and the false positive rate according to different thresholds, as in the precision-recall curve.

You already know about the true positive rate, or sensitivity, which is the same as what we have just learned in the precision-recall curve. The other dimension of an ROC curve is the false positive rate, which is the number of false positives over the number of false positives plus true negatives.

In literature, you might find the false positive rate referred to as inverted specificity, represented by 1 – specificity. Specificity is given as the number of true negatives over the number of true negatives plus false positives. Furthermore, false-positive rates or inverted specificity are the same. Figure 8.4 shows what an ROC curve looks like: