Still going in that direction, AWS has other built-in algorithms that work for regression and classification problems; that is, **Factorization Machines, K-Nearest Neighbor (KNN)** and the **XGBoost** algorithm. Since these algorithms can also be used for classification purposes, we'll cover them in the section about classification algorithms.

> **Important note**
> You just got a very important tip to remember during the exam: linear learner, Factorization Machines, K-Nearest Neighbor, and XGBoost are suitable for both regression and classification problems. These algorithms are often known as algorithms for general purposes.

With that, we have reached the end of this section on regression models. I hope you have enjoyed it; remember to check out our support material before you take your exam. By the way, you can use that reference material when you're working on your daily activities! Now, let's move on to another classical example of a machine learning problem: classification models.

# Working with classification models

You have been learning what classification models are throughout this book. However, now, we are going to discuss some algorithms that are suitable for classification problems. Keep in mind that there are hundreds of classification algorithms out there, but since we are preparing for the AWS machine learning specialty exam, we will cover the ones that have been pre-built by AWS.

We already know what the linear learner does (and we know that it is suitable for both regression and classification tasks), so let's take a look at the other built-in algorithms for general purposes (which includes classification tasks).

We will start with **Factorization Machines**. Factorization machines are considered an extension of the linear learner, optimized to find the relationship between features within high-dimensional sparse datasets.

> **Important note**
> A very traditional use case for Factorization machines is *recommendation systems*, where we usually have a high level of sparsity in the data. During the exam, if you are faced with a general-purpose problem (either a regression or binary classification task) where the underlying datasets are sparse, then Factorization Machines are probably the best answer from an algorithm perspective.