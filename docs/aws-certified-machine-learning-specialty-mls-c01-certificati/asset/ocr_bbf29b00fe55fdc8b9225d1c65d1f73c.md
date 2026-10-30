**Answer**
b, Although you might take some benefits from multi-thread processing and large amounts of RAM, using a GPU to train a neural network will give you the best performance. You will learn much more about neural networks in later chapters of this book, but you already know that they perform a lot of matrix calculations during training, which is better supported by the GPU rather than the CPU.

# 10. Which of the following statements is not true about data resampling?

+   a) Cross-validation is a data resampling technique that helps to avoid overfitting during model training.
+   b) Bootstrapping is a data resampling technique often embedded in ML models that needs resampling capabilities to estimate the target function.
+   c) The parameter k in k-fold cross-validation specifies how many samples will be created.
+   d) Bootstrapping works without replacement.

**Answer**
d, All the statements about cross-validation and bootstrapping are correct except option d, since bootstrapping works with replacement (the same observations might appear on different splits).