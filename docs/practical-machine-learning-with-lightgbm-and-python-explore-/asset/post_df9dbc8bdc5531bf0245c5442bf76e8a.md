
## An overview of XGBoost

XGBoost, short for eXtreme Gradient Boosting, is a widely popular open source gradient boosting library with similar goals and functionality to LightGBM. XGBoost is older than LightGBM and was developed by Tianqi Chen and initially released in 2014 [1].

At its core, XGBoost implements GBDTs and supports building them highly efficiently. Some of the main features of XGBoost are as follows:

- **Regularization**: XGBoost incorporates both L1 and L2 regularization to avoid overfitting
- **Sparsity awareness**: XGBoost efficiently handles sparse data and missing values, automatically learning the best imputation strategy during training
- **Parallelization**: The library employs parallel and distributed computing techniques to train multiple trees simultaneously, significantly reducing training time
- **Early stopping**: XGBoost provides an option to halt the training process if there is no significant improvement in the model's performance, improving performance and preventing overfitting
- **Cross-platform compatibility**: XGBoost is available for many programming languages, including Python, R, Java, and Scala, making it accessible to a diverse user base

Over the years, XGBoost has gained popularity in the machine learning community due to its support for various applications and the library's ease of use and efficiency.

## Comparing XGBoost and LightGBM

There is considerable overlap in functionality between XGBoost and LightGBM. Both libraries implement GBDTs and DART and support building random forests. Both have similar techniques to avoid overfitting and handle missing values and sparse data automatically.

However, some of the differences between XGBoost and LightGBM are as follows:

- **Tree-growing strategy**: XGBoost employs a level-wise tree growth approach, where trees are built level by level, while LightGBM uses a leaf-wise tree growth strategy that focuses on growing the tree by choosing the leaf with the highest delta loss. This difference in growth strategy generally makes LightGBM faster, especially for large datasets.
- **Speed and scalability**: LightGBM is designed to be more efficient regarding memory usage and computation time, making it a better choice for large-scale datasets or when training time is critical. However, this speed advantage can sometimes come at the cost of higher variance in model predictions.
- **Handling categorical features**: LightGBM has built-in support for categorical features, meaning it can handle them directly without needing one-hot encoding or other preprocessing techniques. XGBoost, on the other hand, requires the user to preprocess categorical features before feeding them into the model.
- **Early stopping**: XGBoost provides an option to halt the training process if there is no significant improvement in the model's performance. LightGBM does not have this feature built in, although it can be implemented manually using callbacks, as seen in earlier chapters.

In summary, LightGBM and XGBoost provide similar functionality. LightGBM performs better on large datasets with many features, whereas XGBoost may provide more stable and accurate results on smaller or medium-sized datasets.

## Python XGBoost example

XGBoost provides a scikit-learn-based interface for building models. The following example shows how to use XGBoost on the Forest Cover dataset:

```
from xgboost import XGBClassifier
...
dataset = datasets.fetch_covtype()

X_train, X_test, y_train, y_test = train_test_split(
    dataset.data, dataset.target, random_state=179
)
y_train = y_train - 1
y_test = y_test - 1
xgb = XGBClassifier(
    n_estimators=150, max_leaves=120, learning_rate=0.09
)
xgb = xgb.fit(X_train, y_train)
f1_score(y_test, xgb.predict(X_test), average="macro")
```

The scikit-learn interface should be familiar to you at this stage. The preceding code shows that XGBoost supports similar hyperparameters as we used to train LightGBM-based models. A full list of parameters is available at https://xgboost.readthedocs.io/en/stable/parameter.html.

XGBoost represents a direct alternative to LightGBM as another gradient-boosting library. In the next section, we look at deep learning, a wholly different but extremely popular learning technique, and how it compares to gradient boosting on tabular learning problems.

## Deep learning and TabTransformers

We now look at an approach to solving tabular-based data problems using deep learning. Deep learning has gained immense popularity in recent years due to the performance of deep-learning-based models. Deep-learning-based techniques such as AlphaZero, Stable Diffusion, and the GPT series of language models have achieved human or superhuman performance in gameplay, art generation, and language-based reasoning.

### What is deep learning?

Deep learning is a subfield of the broader machine learning field of artificial neural networks. Artificial neural networks are mathematical mimics of the human brain and consist of interconnected layers of nodes (or “neurons” in biological parlance) that process and transmit information.

Simple artificial neural networks consist of only a few layers. The term “deep” in deep learning refers to using neural networks of many more layers, each with potentially thousands of neurons. These layers are organized hierarchically, with input layers at the bottom, output at the top, and hidden between. Each layer extracts and refines features as data passes through the network, allowing the model to learn complex patterns and representations.

The following diagram depicts a simple neural network called a multilayer perceptron with a single hidden layer.

![](img/31b70e4a77c3afbe11032f68ab874b02_88_0.png)

Figure 4.1 – A multilayer perceptron with a single hidden layer and an output layer. The layers are fully connected

Each neuron receives input from other neurons, performs a mathematical operation, and then passes the result to the next layer of neurons.

The mathematical operation involves two main steps – weighted sum and activation function:

1. **Weighted sum**: The neuron takes the inputs (input data or outputs from previous neurons), multiplies each input by its corresponding weight, and then adds them together. A bias term is often added to the weighted sum for better control over the neuron’s output. Mathematically, this can be represented as follows:

   \( z_j = \sum_i(w_{ij}x_i) + b_j \)

   Here, \( x_i \) represents all inputs to the neuron, \( w_{ij} \) is the weight associated with the \( i^{th} \) input, and \( b_j \) is the bias for the neuron.

2. **Activation function**: The weighted sum is then passed through an activation function, determining the neuron’s output. The purpose of the activation function is to introduce non-linearity into the mathematical operation. The non-linearity allows the neural network to model non-linear and, therefore, complex relationships between inputs and outputs. There are various activation functions, such as **sigmoid** (logistic function), **hyperbolic tangent (tanh)**, and **Rectified Linear Unit (ReLU)**, each with its own properties and use cases. This can be represented as:

   \( a_j = \sigma(z_j) \)

   where \( a_j \) is the neuron output and \( \sigma \) is the activation function.

Combining these two steps, a neuron in a neural network processes the input data, allowing the network to learn and model complex patterns.

Neural networks are trained by adjusting the weights associated with the neurons. The algorithm can be summarized as follows:

1. Weights are initialized to small, random values.
2. A **forward pass** is performed: for each example in a batch, the input features are passed through the entire network (calculating the sum and activation at each neuron) to produce a prediction at the output layer.
3. The loss is then calculated by comparing the output against the actual output for each example in a batch. Like GBDTs, the loss function has to be differentiable, and standard loss functions include the MSE and cross-entropy loss.
4. **Backpropagation** is performed: the gradient of the loss function with respect to the weights is calculated using the calculus chain rule. The process is started at the output layer and works backward through the network.
5. The weights are then updated using gradient descent or one of the modern variants, such as Adam, based on the backpropagated gradients.
6. The process is repeated for a set number of epochs (each epoch running through the entire dataset) to minimize the loss function.

A unique property of neural networks is that neural networks have been proven to be **universal function approximators**. DNNs have the theoretical capability to approximate any continuous function to a
