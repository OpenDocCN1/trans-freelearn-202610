
This chapter covers the following topics:

- Machine learning (ML):
  - Core concepts
  - Feature engineering
  - Validation methods
  - The Bias-variance tradeoff
  - Comparing models to find the best fit
  - Random forests
  - Gradient boosting machines
  - Anomaly detection
- Forecasting using ARIMA
- Deep learning:
  - Use cases with IoT data
  - Setting up and running a simple deep learning model using TensorFlow

# Machine learning (ML)

As a wise uncle of a human-arachnid hybrid once said, With great power, comes great responsibility. This is very true of ML. There are many ways to go wrong. When in the hands of a skilled practitioner, it truly is a form of art. It can be used to do some incredible things on a grand scale, but it should come with a big caution sign. Use it carefully. Be paranoid and validate, validate, validate.

Although we will be going over some core concepts and providing code that you can take and run yourself, this is a big field with lots to learn. It takes years to skillfully and competently apply it. Each section in this chapter is really a book in itself. No, many books. If you plan to use it yourself on IoT data, read, read, read, and then read some more. This chapter is meant to able to provide you with a good foundation to have meaningful conversations with data scientists on the subject.

# What is machine learning?

Ask a hundred experts for the definition of ML, and you are likely to get a hundred slightly different answers. Some will take a broad view and fully include deep learning, artificial intelligence, and some traditional statistical techniques, such as the sum of least squares linear regression. Others will be narrow and restrict their definition to a few modeling techniques, considering it separate from a related field called statistical learning.

Some will say it does not even exist in the real world, only in over-hyped media stories. They feel it is all the same traditional statistical analysis that has been done for decades. Some will consider the term ML as completely interchangeable with the term artificial intelligence, while others will consider them very separate things.

ML is an application of statistical techniques in an ordered set of steps (otherwise known as an algorithm). The statistical techniques are rarely new, many have been around for decades and some over a century. Many of the ML methods have also been around for several decades as well. What has changed is the dramatic decline in the cost of compute power along with a dramatic increase of computing capability. What would have taken months to calculate in 1980 now takes seconds or less.

With the availability of some strong open source statistical software libraries, such as R and Python, combined with the low cost and available speed of modern computing hardware, ML has become practical to implement in a large variety of applications. With an increased use of ML came refinements to the existing methods and developments of new algorithms. This has led to a significant increase in predictive capability and a golden age for ML.

We will use a (hopefully not overblown) analogy to help you think about this. With traditional statistical techniques, you are like a mechanical engineer, applying your expertise and knowledge of how things work to define a set of components that fit together. You use these methods to explicitly build your statistical model. You define the detail of each component based on testing and analysis of the data.

With ML, you become more like an agricultural engineer. A farmer of data models. In this chapter, we will define ML as a method that has three general components that, when combined, *grow* a program from the soil of the provided data. This set of statistical techniques *learn* a representation of the underlying function that determines the target values or categories. Learning, in this case, is adaptation and not cognition, as it would be with us humans. At no point does your computer have even an inkling of what it all means—it is all zeros and ones to it.

![](img/9f2a55db1236f995dcd9392acbc82910_277_0.png)

You as a ML practitioner.

Illustration: Jim Campbell; Inlander; https://www.inlander.com/spokane/farming-data/Content?oid=2136658

The true underlying function is never really known. So, the accuracy of the method can only be inferred from the error rates on new data examples. The goal of many of the ML algorithms (really a set of algorithms) is to iteratively find the right combination of levers to minimize these error rates. If the resulting ML model does a good job of this, then it is said to generalize well. More on this will be explained later.

Any ML model can be viewed as having three interrelated components:

- Representation
- Evaluation
- Optimization

We will cover each in the next sections.

# Representation

Representation is how a model is formally constructed in a way which a computer can interpret. Examples are decision trees, support vector machines, and neural nets. ML models are commonly referred to by the name of the representation. A classifier is an instance in the set of possible models generated by the representation. When you make the choice of which representation to use, you are determining the possibilities of classifiers that your model is able to learn. The range of possibilities is known as the **hypothesis space**.

If the true (and remember, *unknown*) classifier model is not in the hypothesis space, it cannot be learned. Most representation models you will use have a large hypothesis space, so this is probably not going to be a problem. But you should be aware of it, as you may need to expand your choices of representation models in order to expand the collective hypothesis space. This may be necessary if you are getting poor predictive performance from your normal *go-to* set of ML models.

# Evaluation

As the ML model *tunes itself*, there needs to be a way to evaluate how well it is doing. There needs to be a function that measures the performance to know which classifiers are good and which are bad. This is where the evaluation function comes into play. Some examples are accuracy, error rate, precision, recall, F-score, squared error, and information gain. These functions are also referred to as the **objective function** or **scoring function**.

# Optimization

Most of the ML model representations have large hypothesis spaces. A sequential search of all possibilities would take longer than you would ever want to wait for an answer; months, years, or lifetimes, depending on the complexity of the model representation. The choice of the optimization method determines the efficiency of the learning process. There are several general methods to search for the optimal classifier. Some examples are gradient descent, greedy search, and linear programming.

| Representation | Evaluation | Optimization |
|---|---|---|
| Instances<br>　*K*-nearest neighbor<br>　Support vector machines<br>Hyperplanes<br>　Naive Bayes<br>　Logistic regression<br>Decision trees<br>Sets of rules<br>　Propositional rules<br>　Logic programs<br>Neural networks<br>Graphical models<br>　Bayesian networks<br>　Conditional random fields | Accuracy/Error rate<br>Precision and recall<br>Squared error<br>Likelihood<br>Posterior probability<br>Information gain<br>K-L divergence<br>Cost/Utility<br>Margin | Combinatorial optimization<br>　Greedy search<br>　Beam search<br>　Branch-and-bound<br>Continuous optimization<br>　Unconstrained<br>　　Gradient descent<br>　　Conjugate gradient<br>　　Quasi-Newton methods<br>　Constrained<br>　　Linear programming<br>　　Quadratic programming |

The three components of ML algorithms. Source: A Few Useful Things to Know about Machine Learning by Pedro Domingos, University of Washington

# Generalization

The fundamental goal of any ML project is to produce a model that operates well with datasets beyond what is available to you currently. You want it to predict, classify, or estimate with minimal errors. Not so much on the data you already have, but on data that you will have when the resulting model is implemented.

The data that you already have to develop your ML model is called the **training set**. The training set should be representative of what you expect to find in the datasets that you will be applying the model to in the future. Even if you have a very large training set, it is unlikely that future datasets will be precisely the same. This great big, complex world produces a lot of variation.
