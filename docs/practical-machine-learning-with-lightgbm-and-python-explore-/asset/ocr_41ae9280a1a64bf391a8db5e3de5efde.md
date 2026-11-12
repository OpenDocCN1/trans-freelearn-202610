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

# # Comparing LightGBM, XGBoost, and TabTransformers

In this section, we compare the performance of LightGBM, XGBoost, and TabTransformers on two different datasets. We also look at more data preparation techniques for unbalanced classes, missing values, and categorical data.