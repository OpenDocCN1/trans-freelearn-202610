# Limitations

Among the limitations of the linear SVM classifier, the top three that often pose significant challenges are as follows:

+   - **Poor performance on overly complex data**: When data is heavily overlapped or not linearly separable, a linear SVM might struggle to find an optimal hyperplane, leading to subpar performance.
+   - **Selecting a kernel**: Kernels are a way to represent data samples flexibly so that we can compare them in a complex space. The linear SVM classifier uses a linear kernel, but choosing the right kernel (for non-linear SVMs) and tuning its parameters can be complex and is crucial for the algorithm's performance.
+   - **Scalability**: SVMs, in general, can become computationally intensive and less efficient as the dataset size increases, especially in terms of training time, making them less suitable for very large datasets.

In summary, the linear SVM classifier stands out for its robustness and efficiency in handling high-dimensional data and binary classification tasks, offering precise and reliable predictive modeling across various domains, from text categorization to bioinformatics. However, its effectiveness can be constrained by the linear separability of the data, the challenges in kernel selection for non-linear problems, and scalability issues with large datasets.

# The One-vs-Rest classifier (also known as One-vs-All)

The One-vs-Rest (OvR) classifier, also known as One-vs-All, is a strategy for multi-class classification that involves training a single classifier per class by using the samples of that class as positive samples and all other samples as negatives. This approach is widely used to extend binary classifiers, which are inherently designed to distinguish between two classes, to handle multi-class problems effectively.

In an OvR scheme, if there are N classes, N separate binary classifiers are trained. For example, in a scenario with three classes, A, B, and C, three classifiers would be trained: one to distinguish A from not A (B and C), another to distinguish B from not B (A and C), and a third to distinguish C from not C (A and B). For a given test instance, all N classifiers are applied, and the one that classifies the instance with the highest confidence, typically measured as a probability or distance from the decision boundary, is chosen as the predicted class.

Imagine you're organizing a big talent show with a variety of acts: singers, dancers, magicians, and comedians. To manage the event smoothly, you decide to create separate queues for each type of performer so that they can be judged by a panel specialized in their respective fields. However, you only have enough resources to set up one judging panel at a time.