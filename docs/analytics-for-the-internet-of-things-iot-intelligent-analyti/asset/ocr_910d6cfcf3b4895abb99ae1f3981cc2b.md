There are several ways to compare ML models against each other. ROC charts and AUC measures are two of the more popular methods. We will introduce each next.

# ROC curves

Originating from World War II radar engineering, the **receiver operating characteristics** (**ROC**) curve is a common way to compare the effectiveness of ML models against each other. It measures the Recall rate against the **fall-out rate** (calculated as [*1-specificity*]) along a threshold measure. The fall-out rate is also known as the **False Positive Rate (FPR)** or the **probability of false alarm.**

You may have noticed a trend in that a lot of the measures have several different names. It can get confusing but it is important to know the pseudonyms of the measures since different articles, blogs, and research papers will use different names for them.

In most cases, the ROC curve will be used with binary classification problems. Some examples of binary classification questions include **failure/no failure**, **operating/not operating**, **raining/not raining**, and **purchase/no purchase**. An ROC curve shows the change in trade-off between the benefits of correct classifications (true positives) and the costs of incorrect classifications (false positives), along intervals of a threshold parameter.

It can help to think of the true positive rate as follows:

*True positive rate = Correctly classified positives / All positives in the training set*

And think of the false positive rate as follows:

*False positive rate = Incorrectly classified negatives / All negatives in the training set*