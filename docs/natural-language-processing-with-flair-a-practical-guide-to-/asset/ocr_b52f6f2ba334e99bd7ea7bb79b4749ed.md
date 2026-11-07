+   - **Extrinsic evaluation** aims to test the quality of word embeddings by using them in a downstream NLP task such as PoS tagging, NER, or chunking. We feed the embeddings as input to an NLP model. We then evaluate the quality of the NLP model and assume its improvement/regression in performance can be attributed to the performance of our embeddings. Flair has all the tools for training sequence labeling models, as well as offering labeled datasets for commonly spoken languages. If you find yourself training Flair embeddings, chances are you are planning to use them as input to a downstream sequence tagging NLP task. This often makes extrinsic evaluation a straightforward choice.

Let's now jump on to the next part of this chapter, where we train our own embeddings, and later, perform evaluation using one of the techniques described here.

# Training Flair embeddings on the world's smallest language

The Flair package provides a simple, straightforward API for training Flair embeddings. The process involves all the necessary steps required to train any language model. The steps include the following:

+   - Preparing the dictionary
+   - Preparing the corpus
+   - Defining the language model
+   - Training the language model

Let's cover the process of training Flair embeddings through a practical hands-on exercise.

Training embedding for most languages is not a quick process. A decent GPU-equipped machine would require over a week of training time to produce results comparable to the state-of-the-art published Flair results in English or similar languages. Most pre-trained Flair embeddings, such as the en English embeddings model, produce embeddings of length 2,048.

Part of the reason why we need 2,048 dimensional vectors is that languages such as English have a huge number of words in the dictionary, and their semantics change based on the context in which they are used. But what if we, as part of this exercise, trained embeddings on a very small language? What if we trained embeddings on a language so small, that we could do it even on a machine without a dedicated graphics processing unit (GPU)?