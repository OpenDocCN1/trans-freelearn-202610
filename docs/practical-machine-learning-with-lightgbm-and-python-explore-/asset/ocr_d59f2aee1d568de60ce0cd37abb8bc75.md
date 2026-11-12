XGBoost and LightGBM performed very similarly on the dataset, obtaining F1 scores of 0.82 and 0.80, respectively. The DNN struggles significantly with the problem, obtaining an F1 score of only 0.05 even using class weights to compensate for the imbalanced dataset.

Debugging performance issues in DNNs are notoriously tricky. Due to the complexity and opaque­ness of building and training DNN models, small changes can have a significant effect.

Possible reasons for the poor performance include the following:

+   * **Inadequate model architecture:** This is the most likely cause. The architecture is not well suited to the problem. Further experimentation with the architecture, the size of layers, or even the type of neural network is needed.
+   * **Insufficient training:** The model might not be trained long enough. Increasing the training epochs can improve performance. However, in our experiments, the loss stagnated after 10 epochs (while training continued to 15).
+   * **Inappropriate loss function:** We applied a `BinaryCrossentropy` loss function with class weights. However, a more advanced loss function, such as a focal loss, may be tried [9].

In terms of the training and validation time, it is a similar story as with the Census Income dataset. The LightGBM model was trained and validated significantly faster than the other approaches: 3.1 times faster than the XGBoost and 4.62 times faster than the DNN.

# Summary

In this chapter, we discussed two additional algorithms that may be used to solve tabular learning problems: XGBoost, another gradient-boosting framework, and TabTransformer, a deep learning approach.

We showed how to set up and train both XGBoost models and TabTransformer on two datasets. We also showed how to encode categorical features for tree-based and neural network models. Both datasets also had imbalanced classes, which we had to compensate for during training.

We found that LightGBM and XGBoost produced similarly accurate models but that LightGBM trained models much faster and more efficiently. We also saw the complexity of training DNNs and the lackluster performance on these problems. Deep learning is an extremely powerful technique, but tree-based approaches are often more applicable when working with tabular datasets.

In the next chapter, we focus on more effective parameter optimization with LightGBM using a framework called Optuna.