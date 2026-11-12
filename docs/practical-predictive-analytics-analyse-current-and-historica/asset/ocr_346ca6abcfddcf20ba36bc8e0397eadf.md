breaks down by gender. In this case, females make up the majority class at 58% of the population. The goal in decision of tree algorithms is to always grow a tree and improve predictions. So, at this point, we want to do better than 58% prediction for gender.

As we grow the tree, two questions will always need to be taken into consideration:

+   - Which variable will best improve the prediction for each of the classes of the target variable at that point?
+   - For that variable, what is the best split point that will segment a categorical variable into two parts, or divide a continuous variable into two parts, and will improve the prediction for that class?

The outcome of this decision will be a new tree node, or leaf. In a classical decision tree, there are typically two paths (yes or no, high or low) that are followed, but there can be more than two paths depending upon the algorithm.

In our preceding example, the algorithm will first run through all of the variables and determine which one will improve the prediction. That variable happens to be the presence of osteoporosis. If osteoporosis is present, one could predict that the patient is female with 74% accuracy. However, when osteoporosis is not present, the probability of being female actually decreases to 51%, which is slightly worse than the original 58% representation of females in the study.

But wait. The partitioning algorithm will continue to try to split the data again. This time it looks at the average prescriptions variable as a way to improve the prediction. The average number of prescriptions per year per beneficiary for this analysis runs from about 15 per year to about 200. The algorithm will try all values in this range to split the data into two parts. For example, it might try to first compare patients with 10 or more prescriptions, with those with 9 or less prescriptions. It will then look at the prediction for that cutoff. Then it will continue to change to cutoff value until it finds the cutoff value which optimizes the separation between the two classes.

In this instance, it has found that 51 prescriptions is the cutoff that optimizes