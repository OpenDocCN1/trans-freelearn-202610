The following are some benefits of using ROC curves:

+   - **Not sensitive to changes in class distributions**: Shifts in the numbers of positive and negative examples will not affect the ML model’s ROC curve, assuming the model remains as effective in discriminating between them.
+   - **Useful even if classification error costs are unequal**: When the costs of false positives or benefits of the correct classification of positives change, the ROC curve is unaffected. Only the region of interest along the curve changes. In other words, the optimal threshold setting will shift but the shape of the curve will not.
+   - **Allows the comparison of the performance of different ML models along various threshold settings**: Some models will perform best at conservative settings while others may be the best at more liberal settings. The best model, therefore, depends on the underlying business case.

The following are things to keep in mind with ROC curve analysis:

+   - **Difficult to use intuitively for multi-class models**: It is hard to visualize beyond 2-dimensional space and the dimensions grow quickly with multi-class prediction models. There are methods to do this, but they are still not easy to interpret.
+   - **Hard to explain to non-analytical audiences**: There is a learning curve to ROC curves (it is fun to use the word curve). However, non-specialist people can certainly learn how to understand it with repeated exposure. The author recommends educating them if possible. Organizations that understand these charts can use them when making cost/benefit business decisions. Once they get over the hump of learning to interpret the charts, they will find them very useful.

# Area Under the Curve (AUC)

AUC is simply the portion of the ROC chart "square" that is underneath the ROC curve of the ML. Since it is a proportion, it will always range from 0 to 1. An AUC of 1 represents a curve that covers the entire graph, and therefore it is perfectly aligned to the upper-left corner. This would be the aforementioned rarely observed, yeti-like *perfect model*.

The random model represented by the dashed line in the ROC chart has an AUC of 0.5. So, in real world situations, you will be looking at AUC numbers from 0.5 to 1.0. How to interpret an AUC number depends a lot on the industry and business situation. An AUC of 0.6, indicating better than random, may be a fantastic model for stock picking in the finance industry; while the same AUC measure would be an utter failure for cancer detection in the medical industry.