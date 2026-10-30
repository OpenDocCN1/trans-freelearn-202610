+   - **FP**: This is the number of **False Positive** cases. Here, we have to count the number of cases that have been predicted as true but are, instead, false. For example, in a fraud detection system, this would be the number of non-fraudulent transactions that were wrongly predicted as fraud.

In a perfect scenario, your confusion matrix will have only true positive and true negative cases, which means that your model has an accuracy of 100%. In practical terms, if that type of scenario occurs, you should be skeptical instead of happy since it is expected that your model will contain errors. If your model does not contain errors, you are likely to be suffering from overfitting issues, so be careful.

Once false negatives and false positives are expected, the most that you can do is to prioritize one of them. For example, you can reduce the number of false negatives by increasing the number of false positives and vice versa. This is known as the precision versus recall trade-off. Let's take a look at these metrics next.

# Extracting metrics from a confusion matrix

The simplest metric that we can extract from a confusion matrix is known as **accuracy**. Accuracy is given by the number of true positives plus true negatives over the total number of cases. In Figure 8.2, we have filled out all the components of the confusion matrix for the sake of this demonstration:

![](bbox=[0.25, 0.58, 0.75, 0.82])

Figure 8.2 – A confusion matrix filled with some examples