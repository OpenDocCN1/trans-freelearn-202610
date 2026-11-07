
## Assessing and comparing performance metrics

The final metric that will determine how good our tagger is, is the performance metric that evaluates our model on training data. In the case of PoS tagging, this will likely be the F1-score. But the value itself doesn't tell us much. For example, in the preceding exercise, we achieved an F1-score of 0.88. If we used a representative dataset, this number on its own does give us some general understanding of how well the model will perform in the real world, but it does little in helping assess whether our tagger is something we should be happy with or whether it is better than any other tagger out there.

One of the most objective ways of assessing model performance is comparing it to the state-of-the-art results published as part of related studies or experiments. That way, if we achieved a larger F1-score, we can claim that (given the testing data that was used in evaluation) our model indeed achieves better performance. However, when comparing results across different studies or experiments, we need to be absolutely sure that the same train and test data were used in all experiments. Otherwise, we will essentially be comparing apples to oranges. For example, if one study used a dataset with very repetitive and simple language with a small vocabulary whereas the other one focused on some highly specialized and obscure uses of language with a rich vocabulary, it's safe to assume that a tagger trained on the former type of dataset will achieve better results. It's therefore vital to only compare the performance of taggers trained on same data.

## Summary

The code examples, explanations, and exercises that we covered in this chapter are a quick introduction to training sequence labeling models in Flair, and it should give you the confidence to prepare, train, validate, and use Flair models that solve real-world problems.

Also, you should now be able to have the ability to tell whether model training was a success or whether it should be restarted with different parameters. We learned that there is a large number of parameters that govern sequence labeling model training as well as the training process. There's the learning rate, the number of epochs, the optimizer type, the number of hidden RNN layers, and a long list of other parameters that will likely affect the performance of our model. In the preceding examples, we usually chose the default parameters that Flair happens to use in most of its code examples, but there's no guarantee that the default parameters are the optimal parameters for our problem. In fact, it's highly unlikely. This will leave you wondering which parameters you should really use.

Luckily, Flair features a solution to this problem – a special technique that attempts model training with different combinations of parameters. It then measures performance on a special dataset called the development or validation dataset. This technique is called hyperparameter tuning. Let's learn how to do it in Flair in the upcoming chapter.

## 6 Hyperparameter Optimization in Flair

Grasping the concept of sequence tagging and getting a basic understanding of how it works generally isn't a huge problem. What isn't as straightforward is understanding all the parameters that govern model training and choosing the values that yield desired results. A special technique called **hyperparameter optimization** (also called **hyperparameter tuning**) helps us achieve that.

We will start with providing a general overview of what hyperparameter tuning is, why it's useful, and what different optimization methods are out there. We'll then dive into how to do tuning in Python with the **Hyperopt** library. We will conclude the chapter with a hands-on exercise where we will find the optimal hyperparameters for a **Part-of-Speech (POS)** tagger in Flair.

In this chapter, we will cover hyperparameter optimization in Flair as part of the following sections:

- Understanding hyperparameter tuning
- Hyperparameter tuning in Python
- Hyperparameter optimization in Flair

## Technical requirements

All of the Python libraries used in this chapter are direct dependencies of Flair version 0.11 and require no special setup, assuming that Flair is already installed on your machine. Code examples covered in this chapter are found in this book's official GitHub repository in the following Jupyter notebook: https://github.com/PacktPublishing/Natural-Language-Processing-with-Flair/tree/main/Chapter06.

## Understanding hyperparameter tuning

When first faced with a long list of model training parameters and their possible values, you might think that in order to successfully train a model, you need a special superpower that helps you pick the right parameter for the right scenario. This isn't necessarily true. While experience may help you narrow down the set of possible hyperparameters, there usually isn't a reliable way of knowing with certainty what the best hyperparameter is in advance.

Let's imagine the simplest possible scenario – a sequence tagging model trainer that receives a single parameter – say, a learning rate. This is generally a value between 0 (exclusive) and 1. To create a set of possible hyperparameter values, we simply discretize the range into a set of 10 possible hyperparameter values: `[0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0]`. We can then perform the most trivial type of hyperparameter optimization by training 10 different models – each one trained with a different learning rate value. We evaluate the quality of each of our models and declare the highest-performing model as the one with the optimal learning rate value.

This type of hyperparameter tuning is called **grid search**, described in more detail at https://en.wikipedia.org/wiki/Hyperparameter_optimization#Grid_search. It is an exhaustive search algorithm, meaning it tries out the entire search space. Our preceding example illustrates the process of hyperparameter tuning if given a very dumbed down and simplified problem. Unfortunately, real-world hyperparameter selection usually isn't as forgiving for the following reasons:

- A fixed number of (in our example, 10) different learning rate values may not always be enough to find the optimal learning rate.
- The discrete uniform distribution (a distribution where the possible values are discrete and equally spaced) isn't always optimal.
- We can quickly experience the curse of dimensionality. Most real-world model trainers will govern training on more than just a single hyperparameter. This is where things get complicated. In our simplified example where we had a single hyperparameter, we had a search space of 10 possible options. Let's imagine a problem with two such hyperparameters. The number of possible parameter option combinations would be 10². Given three such hyperparameters, the number of possible combinations would be 10³. This clearly suggests exponential growth that can very quickly grow out of proportion. This problem is referred to as the curse of dimensionality because with each extra parameter, we are adding another dimension that needs exploration.

Luckily, there are more sophisticated optimization methods that help us mitigate these problems.

> **Important Note**
> When doing hyperparameter tuning evaluation, it's best to use a dataset that is completely distinct from the data that will be used in our final model. This dataset is called a development (also known as the validation) dataset. Keeping data used for hyperparameter tuning separate from data used for training and evaluation helps us in avoiding overfitting, which would result in choosing the hyperparameters that would overfit our test dataset.

To sum up, hyperparameter tuning in NLP is simply a process that takes in a search space, an optimization method, and an objective function. It then uses the optimization (search) method to train models using a number of different hyperparameter option combinations and returns a set of hyperparameters that result in the best-performing model.

## Optimization methods

Hyperparameter tuning utilizes some of the following commonly used optimization methods:

- Grid search
- Random search
- Bayesian optimization
- Evolutionary algorithms

Let's look at each of these methods in detail.
