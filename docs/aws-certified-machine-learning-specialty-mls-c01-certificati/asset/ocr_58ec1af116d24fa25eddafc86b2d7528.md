Last but not least, note the following configurations of Blazing Text, since they are likely to be present in your exam:

+   - In word2vec mode, only the train channel is available.
+   - Blazing Text expects a single text file with space-separated tokens. Each line of the file must contain a single sentence. This means you usually have to pre-process your corpus of data before using Blazing Text.

# Sequence-to-sequence algorithm

This is a supervised algorithm that transforms an input sequence into an output sequence. This sequence can be a text sentence or even an audio recording.

The most common use cases for sequence-to-sequence are machine translation, text summarization, and speech-to-text. Anything that you think is a sequence-to-sequence problem can be approached by this algorithm.

Technically, AWS SageMaker's Seq2Seq uses two types of neural networks to create models: a Recurrent Neural Network (RNN) and a Convolutional Neural Network (CNN) with an attention mechanism.

**Latent Dirichlet Allocation**, or **LDA** for short, is used for topic modeling. Topic modeling is a textual analysis technique where you can extract a set of topics from a corpus of text data. LDA learns these topics based on the probability distribution of the words in the corpus of text.

Since this is an unsupervised algorithm, there is no need to set a target variable. Also, the number of topics must be specified up-front and you will have to analyze each topic to find their domain meaning.

# Neural Topic Model (NTM) algorithm

Just like the LDA algorithm, the Neural Topic Model (NTM) also aims to extract topics from a corpus of data. However, the difference between LDA and NTM is their learning logic. While LDA learns from probability distributions of the words in the documents, NTM is built on top of neural networks.

The NTM network architecture has a bottleneck layer, which creates an embedding representation of the documents. This bottleneck layer contains all the necessary information to predict document composition, and its coefficients can be considered topics.

With that, we have completed this section on textual analysis. In the next section, we will learn about image processing algorithms.