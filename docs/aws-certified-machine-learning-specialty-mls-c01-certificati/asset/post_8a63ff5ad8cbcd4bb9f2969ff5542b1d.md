
## Evaluating classification models

Classification models are one of the most traditional classes of problems that you might face, either during the exam or during your journey as a data scientist. A very important artifact that you might want to generate during the classification model evaluation is known as a **confusion matrix.**

A confusion matrix compares your model predictions against the real values of each class under evaluation. *Figure 8.1* shows what a confusion matrix looks like in a binary classification problem:

![](img/209e007237d34506e7ba16180f2e905c_271_0.png)

Figure 8.1 – A confusion matrix

We find the following components in a confusion matrix:

- **TP**: This is the number of **True Positive** cases. Here, we have to count the number of cases that have been predicted as true and are, indeed, true. For example, in a fraud detection system, this would be the number of fraudulent transactions that were correctly predicted as fraud.
- **TN**: This is the number of **True Negative** cases. Here, we have to count the number of cases that have been predicted as false and are, indeed, false. For example, in a fraud detection system, this would be the number of non-fraudulent transactions that were correctly predicted as not fraud.
- **FN**: This is the number of **False Negative** cases. Here, we have to count the number of cases that have been predicted as false but are, instead, true. For example, in a fraud detection system, this would be the number of fraudulent transactions that were wrongly predicted as not fraud.
- **FP**: This is the number of **False Positive** cases. Here, we have to count the number of cases that have been predicted as true but are, instead, false. For example, in a fraud detection system, this would be the number of non-fraudulent transactions that were wrongly predicted as fraud.

In a perfect scenario, your confusion matrix will have only true positive and true negative cases, which means that your model has an accuracy of 100%. In practical terms, if that type of scenario occurs, you should be skeptical instead of happy since it is expected that your model will contain errors. If your model does not contain errors, you are likely to be suffering from overfitting issues, so be careful.

Once false negatives and false positives are expected, the most that you can do is to prioritize one of them. For example, you can reduce the number of false negatives by increasing the number of false positives and vice versa. This is known as the precision versus recall trade-off. Let's take a look at these metrics next.

## Extracting metrics from a confusion matrix

The simplest metric that we can extract from a confusion matrix is known as **accuracy**. Accuracy is given by the number of true positives plus true negatives over the total number of cases. In Figure 8.2, we have filled out all the components of the confusion matrix for the sake of this demonstration:

![](img/209e007237d34506e7ba16180f2e905c_272_0.png)

Figure 8.2 – A confusion matrix filled with some examples

According to Figure 8.2, the accuracy would be (100 + 90) / 210, which is equal to 0.90.
There is a common issue that occurs when utilizing an accuracy metric, which is related to the balance of each class. Problems with highly imbalanced classes, such as 99% of positive cases and 1% of negative cases, will impact the accuracy score and make it useless.

For example, if your training data has 99% of positive cases (the majority class), your model is likely to correctly classify most of the positive cases but go badly in the classification of negative cases (the minority class). The accuracy will be very high (due to the correctness of the classification of the positive cases), regardless of the bad results in the minority class classification.

The point is that on highly imbalanced problems, we usually have more interest in correctly classifying the minority class, not the majority class. That's the case on most fraud detection systems, for example, where the minority class corresponds to fraudulent cases. For imbalanced problems, you should look for other types of metrics, which we will cover next.

Another important metric that we can extract from a confusion matrix is known as **recall**, which is the number of true positives over the number of true positives plus false negatives. In other words, recall is given by the number of true positive and overall positive cases. Recall is also known as **sensitivity**.

According to Figure 8.2, recall is given by 100 / 112, which is equal to 0.89. **Precision**, on the other hand, is given by the number of true positives over the number of true positives plus false positives. In other words, precision is given by the number of true positive and overall predicted positive cases. Precision is also known as **positive predictive power**.

According to Figure 8.2, precision is given by 100 / 108, which is equal to 0.93. In general, we can increase precision by the cost of decrease recall and vice versa. There is another model evaluation artifact in which we can play around with this precision versus recall trade-off. It is known as a **precision-recall curve**.

Precision-recall curves summarize the precision versus recall trade-off by using different probability thresholds. For example, the default threshold is 0.5, where any prediction above 0.5 will be considered as true; otherwise, it is false. You can change the default threshold according to your need so that you can prioritize recall or precision. Figure 8.3 shows an example of a precision-recall curve:

![](img/209e007237d34506e7ba16180f2e905c_274_0.png)

Figure 8.3 – A precision-recall curve

As you can see in Figure 8.3, increasing the precision will reduce the amount of recall and vice versa. Figure 8.3 shows the precision/recall for each threshold for a gradient boosting model (as shown by the orange line) compared to a no-skill model (as shown by the blue dashed line). A perfect model will approximate the curve to the point (1,1), forming a squared corner in the top right-hand side of the chart.

Another visual analysis we can do on top of confusion matrixes is known as a Receiver Operating Characteristic (ROC) curve. ROC curves summarize the trade-off between the true positive rate and the false positive rate according to different thresholds, as in the precision-recall curve.

You already know about the true positive rate, or sensitivity, which is the same as what we have just learned in the precision-recall curve. The other dimension of an ROC curve is the false positive rate, which is the number of false positives over the number of false positives plus true negatives.

In literature, you might find the false positive rate referred to as inverted specificity, represented by 1 – specificity. Specificity is given as the number of true negatives over the number of true negatives plus false positives. Furthermore, false-positive rates or inverted specificity are the same. Figure 8.4 shows what an ROC curve looks like:

![](img/209e007237d34506e7ba16180f2e905c_275_0.png)

Figure 8.4 – An ROC curve

A perfect model will approximate the curve to the point (0,1), forming a squared corner in the top left-hand side of the chart. The orange line represents the trade-off between the true positive rate and the false positive rate of a gradient boosting classifier. The dashed blue line represents a no-skill model, which cannot predict the classes properly.

To summarize, you can use ROC curves for fairly balanced datasets and precision-recall curves for moderate to imbalanced datasets.

## Summarizing precision and recall

Sometimes, we might want to use a metric that summarizes precision and recall, instead of prioritizing one over the other. Two very popular metrics can be used to summarize precision and recall: F1 score and Area Under Curve (AUC).

The F1 score, also known as F-measure, computes the harmonic mean of precision and recall. AUC summarizes the approximation of the area under the precision-recall curve.

That brings us to the end of this section on classification metrics. Let's now take a look at the evaluation metrics for regression models.

## Evaluating regression models

Regression models are quite different from classification models since the outcome of the model is a continuous number. Therefore, the metrics around regression models aim to monitor the difference between real and predicted values.
