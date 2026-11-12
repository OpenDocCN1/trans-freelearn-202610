
desired level of accuracy, given a sufficient number of hidden neurons and an appropriate activation function. This property is based on the **Universal Approximation Theorem**, which has been proven for various types of neural networks.

This means that a neural network can learn to represent complex relationships between input and output data, no matter how intricate or non-linear these relationships might be. This capability is one of the reasons why neural networks, especially DNNs, have successfully solved a wide range of problems across different domains. However, this guarantee is theoretical. In practice, finding the correct network architecture, hyperparameters, and training techniques to achieve the desired level of approximation can be challenging. The process often requires experimentation, expertise, and prohibitive computational resources.

## Advantages and disadvantages of deep learning

Given the capabilities of DNNs, we might believe that they should be our first port of call for all machine learning problems. The primary advantage of using DNNs is their high accuracy in very complex domains: the current state-of-the-art performance on a wide range of complex tasks, natural language processing, generative AI, image recognition, and speech recognition are all achieved by DNNs due to their ability to learn complex and hidden patterns in large datasets.

Another advantage is automatic feature extraction. With the correct architecture, a DNN can extract complex or higher-order features automatically, alleviating the need for a data scientist to perform feature engineering.

Finally, DNNs can also transfer learning: pre-trained deep learning models can be fine-tuned on a smaller dataset for a specific task, leveraging the knowledge acquired during the initial training. Transfer learning can significantly reduce training time and data requirements for new tasks.

However, deep learning is not a panacea to all machine learning problems. Some of the disadvantages of using DNNs include the following:

- **Computational resources**: Deep learning models often require significant computing power and memory for training, especially when dealing with large datasets and complex architectures.
- **Large datasets**: DNNs usually perform well when trained on large datasets, but their performance can degrade when trained on smaller datasets. When a dataset is too small, the DNN overfits the training data and cannot generalize to unseen data.
- **Interpretability**: DNNs are often considered “black boxes” due to their complex architectures and the large number of parameters involved. The complexity can make understanding how the model makes decisions difficult, which may be a concern for applications requiring transparency or regulatory compliance.
- **Hyperparameter tuning**: DNNs involve numerous hyperparameters, such as network architecture, learning rate, and activation functions. Coupled with longer training times and resource needs, finding the optimal combination of these hyperparameters can be expensive and time-consuming.

## Introducing TabTransformers

We want to apply deep learning to tabular data, as most practical machine learning problems have tabular data. To this end, we use a new deep learning architecture called **TabTransformer**: a deep neural network model designed to handle tabular data specifically.

Like the GPT family of DNNs, TabTransformer is based on the transformer architecture originally introduced by Vaswani et al. [2]. TabTransformer adapts the transformer architecture to work effectively with tabular data, providing an alternative to other machine learning models such as decision trees and gradient boosting machines for such data [3].

![](img/31b70e4a77c3afbe11032f68ab874b02_91_0.png)

Figure 4.2 – The TabTransformer architecture as implemented in Keras [3]

The model architecture for TabTransformer is shown in Figure 4.2. With TabTransformer, each feature in the tabular data is treated as a token, similar to how words are treated as tokens in natural language processing. The model applies self-attention mechanisms to learn complex interactions and dependencies between features in the input data. The token embedding and attention mechanism allows the model to capture global and local relationships between features.

The TabTransformer model has several key components: token embedding, positional encoding, transformer layers, pooling, and output layers. The token embedding converts each feature value into a continuous vector representation, combined with positional information through positional encoding.

Moving through the layers, shown in Figure 4.2, we can see that the categorical and numerical features are split.

The categorical features first go through an embedding layer, as implemented by layers.Embedding in Keras, and are then passed along to the transformer blocks. A variable number of transformer blocks can be implemented (as set using a hyperparameter), but each consists of a layers.MultiHeadAttention layer and a layers.Dense layer with Dropout. The output values are added and normalized after passing through the attention and dense layers.

Due to their performance, the transformer feedforward layer uses the Gaussian error linear unit (GELU) activation function in our implementation. However, other activation functions may be used [5].

The numerical features are passed through a normalization layer (normalizing the numerical input ranges) and then concatenated with output from the transformers.

The concatenated results are passed through a multilayer perceptron (MLP) block consisting of a variable number of dense layers, each with Dropout. Our implementation uses scaled exponential linear unit (SELU), which causes the activations to self-normalize [6].

Finally, the output from the MLP block is passed to the loss function, the implementation of which depends on the learning problem (classification or regression).

TabTransformers are much more complex to implement and train than gradient-boosted trees. Like other DNNs, TabTransformers require more data preparation and computational power than gradient-boosted trees.

In addition to TabTransformers, this section introduced deep learning alongside its advantages and disadvantages. In the next section, we use a practical example to compare the different approaches, including the complexity of working with TabTransformers.

## Comparing LightGBM, XGBoost, and TabTransformers

In this section, we compare the performance of LightGBM, XGBoost, and TabTransformers on two different datasets. We also look at more data preparation techniques for unbalanced classes, missing values, and categorical data.

### Predicting census income

The first dataset we use is the Census Income dataset, which predicts whether personal income will exceed $50,000 based on attributes such as education, marital status, occupation, and others [4]. The dataset has 48,842 instances, and as we'll see, some missing values and unbalanced classes.

The dataset is available from the following URL: https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data. The data has already been split into a training set and a test set. Once loaded, we can sample the data:

```
train_data.sample(5) [["age", "education", "marital_status", "hours_per_week", "income_bracket"]]
```

The data sample for the selected columns is shown in Table 4.1.

| | age | education | marital_status | hours_per_week | income_bracket |
|---|---|---|---|---|---|
| 12390 | 34 | Some-college | Never-married | 40 | <=50K |
| 20169 | 41 | Assoc-acdm | Married-civ-spouse | 45 | >50K |
| 17134 | 35 | Doctorate | Never-married | 60 | >50K |
| 23452 | 49 | HS-grad | Married-civ-spouse | 40 | >50K |
| 22372 | 31 | HS-grad | Separated | 45 | <=50K |

Table 4.1 – Sample data from the Census Income dataset

Table 4.1 shows that we have mixed data types: some features are numeric, and others are text. Notably, some columns in the dataset are **categorical features**: string-based features with a fixed set of values. Next, we look at encoding these features for use in machine learning.

### Encoding categorical features

Most machine learning algorithms need string-based features to be encoded to numbers; in some cases, this can be done automatically. We discuss automatic encoding for LightGBM in Chapter 6, Solving Real-World Data Science Problems with LightGBM. In this example, we encode the features to understand what this entails.

We need to map each categorical value to a unique number; therefore, we first build a vocabulary of all values for each feature:

```
CATEGORICAL_FEATURES_WITH_VOCABULARY = {
    "workclass": sort_none_last(list(train_data["workclass"].unique())),
    "education": sort_none_last(list(train_data["education"].unique())),
    "marital_status": sort_none_last(list(train_data["marital_status"].unique())),
}
```
