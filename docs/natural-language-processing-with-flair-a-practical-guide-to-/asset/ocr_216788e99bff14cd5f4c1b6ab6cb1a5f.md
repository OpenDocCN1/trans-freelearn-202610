Where:

+   - **True Positives (TP)**: The number of examples predicted positive that are actually positive
+   - **False Positives (FP)**: The number of examples predicted positive that are actually negative
+   - **False Negatives (FN)**: The number of examples predicted negative that are actually positive

The F1 score is then defined as the harmonic mean of precision and recall:

$F1 = 2 \cdot \frac{precision \cdot recall}{precision + recall}$

The process of working out the F1 score is the same as the one used for sequence labeling and involves the following steps:

+   1. Counting true positives, false positives, and false negatives
+   2. Getting precision and recall using the preceding formulae
+   3. Computing the F1 score as the harmonic mean of precision and recall

If we compute the F1 score for each class label separately and then average all the F1 scores, we obtain the **macro F1 score**. This metric takes into account imbalanced class distributions in our dataset.

If we compute the F1 score for all the classes counting the total true positives, false negatives, and false positives together, we obtain the **micro F1 score**. This metric will have the same value as accuracy in single-label classification.

Let's see how this theory holds up in practice with one of the simplest yet most powerful NLP libraries out there.

# Text classification in Flair

Flair offers a simple interface for using pre-trained models as well as training custom text classifiers. While Flair's secret sauce – the forward and backward Flair embeddings – aren't the best tool for text classification tasks, Flair can leverage other third-party methods to yield excellent performance in text classification.

Let's first learn how to train and use Flair's pre-trained models.