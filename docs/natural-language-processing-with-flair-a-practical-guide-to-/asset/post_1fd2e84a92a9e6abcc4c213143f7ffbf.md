
The answer is no. With word-based, non-contextual word embeddings, the number of possible embeddings is indeed bound by the number of words in the dictionary. But, because Flair embeddings can model virtually any sequence of characters (not necessarily a known word) in any given context, the number of possible embeddings is unimaginably large. This is because of the following:

- Flair embeddings are contextual, meaning the same word will result in a different embedding when put in a different context.
- Flair embeddings are trained without an explicit notion of words. Words are modeled as sequences of characters.

When we talk about training Flair embeddings, we are not really talking about pre-computing the embeddings themselves. Instead, we are talking about training a language model. Let's learn more about that.

## The idea behind Flair embeddings

Training Flair embeddings is really a process of training language models. Flair embedding models are essentially language models that predict the next character given some context (surrounding text). Flair uses language models based on Long Short-Term Memory (LSTM) recurrent neural network nodes. These nodes are capable of processing sequential data.

> **What Is a Language Model?**
> In simple terms, a language model is a tool that models a probability distribution over strings of text. It assigns a probability to each possible character, word, or sequence of words often given some context (preceding text). A popular type of language model, up until the emergence of deep learning, was the statistical n-gram model; but, the more recent neural-based approaches consistently outperform the n-gram models. There are still some advantages to the classic n-gram models. The n-gram models, unlike most neural models, don't require much data and can be very light in terms of size.

Character-based language models receive input text as a sequence of characters and are trained to predict the next character. The model is then trained so that it learns to predict the probability distribution of the possible next characters given past characters.

*So, what does predicting the next character have to do with word embeddings?*

A trained Flair language model in essence is the model for producing word embeddings. First, we pass the entire context (text that surrounds our word), including the word we want to compute the embedding for, to the model as input. Then, instead of focusing on the predicted character, we focus on a special value of the LSTM cell called the hidden state. We extract the hidden state after the last character in the word, and this is our word embedding.

It is clear that context affects the semantics of certain words. But, you may wonder whether it's the text that *precedes* a word that influences the meaning of the word, or whether it's the text that comes *after* the word that influences its meaning. The answer is both.

## The forward-backward embedding stack

One brilliant design concept in Flair embeddings that helps them achieve their great performance is the way they use a backward-forward embedding stack.

The **forward model** takes into account the entire context before the word and the word itself. It captures the semantic meaning of the word influenced by the text that precedes the word. But, this model doesn't really take into account all the words that come after the word we're trying to compute the embedding for. To accomplish this, we would need to build a model that works in a reverse direction. We would need to build a **backward model**.

A backward model processes words in the opposite direction. For English, this would mean from right to left. It takes into account all the context after the word plus the word itself. This model captures the semantic meaning of the word influenced by the text that follows the word.

Once both models are trained, we can then compute a forward embedding and a backward embedding. We then usually join them together to form a new embedding that we call the **embedding stack**.

## Corpora for Flair embeddings

Training any type of machine learning model requires data. Luckily, training embeddings is an unsupervised learning task. The data used for training is unlabeled. What this means is that potentially any text that contains meaningfully constructed words can be used as part of our corpora. What is important is that we use a large enough corpus.

There is also the question of copyright. There is an ongoing legal question about whether the copyright-related rights of the corpus also apply to the language model itself, and under what conditions. For example, if attribution is required to distribute an original corpus, do the same attribution requirements also apply to a language model that was trained on that same corpus? The answer to this question depends on many factors, but there is currently no clear way of answering in a way that would apply to all types of scenarios. But, some general rules still do apply. If you are learning about NLP at home on your personal machine, it's unlikely you will get yourself into trouble if you train your model on any publicly available text. These practical exemptions, however, do not apply when working in a commercial environment. The enforcement of the copyright laws gets progressively stricter as you move toward the larger tech companies.

## Dictionaries for training Flair embeddings

One of the best things about Flair embedding training is the effort, or lack thereof, needed to prepare a dictionary for the language model. With most traditional word-based language models, you would need to come up with a complete list of words for a given language.

Luckily, Flair embeddings are character-based. This means that the input to the language model isn't words, but rather sequences of characters. We need not worry about coming up with a complete list of words. Instead, we only need a list of all possible characters used in our language. This means that if we are dealing with a language that uses Latin script, our dictionary will contain only a handful of items. Flair also provides classes with predefined commonly used dictionaries, such as Latin.

Flair's pre-trained embeddings yield state-of-the-art results on many downstream NLP tasks in many languages. But, when training new embeddings models, the performance will depend on many factors, and not every embedding model we train will perform well. We need to find out ways of figuring out whether our newly trained embeddings are up to par.

## Evaluating word embeddings

In the previous section, we covered the design of Flair embeddings that use language models. The process of training these language models isn't much different from any other type of deep learning training. But well-performing language models don't necessarily mean good embeddings that yield excellent results on downstream tasks.

Instead, we typically need to rely on the following two approaches of evaluating word embeddings:

- **Intrinsic evaluation** aims to test the quality of embedding word representations independent of any natural language processing tasks. This is done by measuring semantic relationships between words. The simplest type of intrinsic embedding evaluation is word similarity. It simply uses a similarity metric, such as cosine distance, to measure the similarity between word embeddings and compares it to the human-perceived semantic similarity. For example, the words *begin* and *start* are semantically very similar. If our embeddings are well trained, we would then expect those two words to be close together in cosine distance.

Other types of intrinsic evaluation methods exploit the analogy relationships between words, similar to how we learned about word embeddings as part of the *king is to man as queen is to woman* analogy exercise in *Chapter 3, Embeddings in Flair.*

- **Extrinsic evaluation** aims to test the quality of word embeddings by using them in a downstream NLP task such as PoS tagging, NER, or chunking. We feed the embeddings as input to an NLP model. We then evaluate the quality of the NLP model and assume its improvement/regression in performance can be attributed to the performance of our embeddings. Flair has all the tools for training sequence labeling models, as well as offering labeled datasets for commonly spoken languages. If you find yourself training Flair embeddings, chances are you are planning to use them as input to a downstream sequence tagging NLP task. This often makes extrinsic evaluation a straightforward choice.

Let's now jump on to the next part of this chapter, where we train our own embeddings, and later, perform evaluation using one of the techniques described here.

## Training Flair embeddings on the world's smallest language

The Flair package provides a simple, straightforward API for training Flair embeddings. The process involves all the necessary steps required to train any language model. The steps include the following:

- Preparing the dictionary
- Preparing the corpus
- Defining the language model
- Training the language model

Let's cover the process of training Flair embeddings through a practical hands-on exercise.

Training embedding for most languages is not a quick process. A decent GPU-equipped machine would require over a week of training time to produce results comparable to the state-of-the-art published Flair results in English or similar languages. Most pre-trained Flair embeddings, such as the en English embeddings model, produce embeddings of length 2,048.

Part of the reason why we need 2,048 dimensional vectors is that languages such as English have a huge number of words in the dictionary, and their semantics change based on the context in which they are used. But what if we, as part of this exercise, trained embeddings on a very small language? What if we trained embeddings on a language so small, that we could do it even on a machine without a dedicated graphics processing unit (GPU)?
