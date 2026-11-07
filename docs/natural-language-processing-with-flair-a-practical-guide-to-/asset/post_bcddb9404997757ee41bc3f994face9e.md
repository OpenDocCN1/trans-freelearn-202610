
## Measuring the F1 score for sequence labeling tasks

F1 score is the harmonic mean of two other metrics called *precision* and *recall*. It ranges from 0 (worst possible score) to 1 (best possible score).

In sequence tagging, *precision* is defined as the percentage of named entities found by the learning system that are correct. It is defined as the following:

$$precision = \frac{tp}{tp + fp}$$

*Recall* is the percentage of named entities present in the corpus that are identified by the system. It is defined as:

$$recall = \frac{tp}{tp + fn}$$

The following apply:

- **True positives** (*tp*): The number of tokens that are correctly labeled with a specific tag. For example, if a token is labeled as noun in the training data and our tagger labels it with the noun tag, this is considered a true positive.
- **False positives** (*fp*): The number of tokens that were incorrectly labeled with a specific tag. For example, if a token is labeled as verb in the training data, but our tagger labels it with the noun tag.
- **False negatives** (*fn*): The number of tokens that should have been labeled with a specific tag, but weren't. For example, if a token is labeled as noun in the training data, but our tagger labels it with the verb tag.

F1 score is then defined as the harmonic mean of precision and recall.

$$F1 = 2 \cdot \frac{precision \cdot recall}{precision + recall}$$

The process of working out the F1 score therefore involves the following:

- Counting true positives, false positives, and false negatives
- Getting *precision* and *recall* using the above formulae
- Computing the F1 score as the harmonic mean of *precision* and *recall*

If we compute the F1 score for each tag (for example, for each named entity) separately, and then average all the F1 scores, we obtain the macro F1 score. This metric takes into account unequal representations of tags in the corpus.

If we compute the F1 score for all the tags counting the total true positives, false negatives, and false positives together, we obtain the *micro F1 score*. This metric will have the same value for accuracy as single-label problems (for taggers where one token can have at most one tag assigned).

Next time you assess reported sequence tagger results, be careful about whether the F1 score refers to the micro or the macro variant. The variant most often used in Flair's results (when not explicitly specified) is the micro F1 score.

## Summary

With the knowledge gained in this chapter, we are not only able to use many of the pretrained sequence taggers available in Flair, but we are also capable of interpreting and understanding their output.

In this chapter, we covered sequence tagging in Flair from different angles. We touched on the design and architecture of Flair's proposed sequence taggers. We then covered NER motivation and theory, and what pretrained NER taggers can be found in Flair. We did the same with PoS tagging, where we covered the most notable tag sets, such as the Penn tag set and the universal PoS tag set, before covering the important pretrained PoS taggers found in Flair. We then finally studied the two metrics most often used to evaluate sequence taggers, and we emphasized the importance of distinguishing between micro and macro F1 scores.

In the next chapter, we will be looking into using Flair to train, store, and reuse your own sequence labeling models.

## Part 2: Deep Dive into Flair – Training Custom Models

In this part, you will learn how to train custom sequence labeling models, embeddings, and classifiers with Flair, evaluate their performance, and achieve optimal results with hyperparameter optimization.

This part comprises the following chapters:

- Chapter 5, Training Sequence Labeling Models
- Chapter 6, Hyperparameter Optimization in Flair
- Chapter 7, Train Your Own Embeddings
- Chapter 8, Text Classification in Flair

## 5 Training Sequence Labeling Models

In this book, we have so far covered a number of features and models Flair offers right out of the box. The set of pre-trained sequence taggers, embeddings, and other models at first seems large enough for us to never need anything more than what's already available in Flair. But chances are, if you work with **natural language processing (NLP)** for long enough, you will encounter a problem that isn't generic enough to be solved by pre-trained models. When faced with such a problem, we usually need to train our own. This process involves acquiring training data, preprocessing it, possibly hand-labeling it, and finally, working with an NLP framework to train the model. But because Flair ships a wide selection of labeled corpora, chances are that you will only ever need to perform the last step, model training – the main focus of this chapter.

We will start this chapter by explaining when and why training custom models is necessary. Next, we will go through the hardware requirements for training sequence labeling models, showing both the free and paid approaches to training your models in the cloud.

We will then briefly cover the process of neural model training. After that, we will look at the metrics and parameters that govern training. We will go through the actual model training syntax as part of a hands-on exercise where we train an English part-of-speech tagger. Finally, we will learn a few basic approaches to assessing model quality and running visualizations that will help us decide whether our model is good enough or whether we should continue training or start from scratch.

In this chapter, we will cover the preceding topics as part of the following sections:

- The motivation behind training custom models
- Understanding the hardware requirements for training models
- Working with parameters for training and evaluation
- Training custom sequence labeling models
- Knowing when to stop and try again

## Technical requirements

All of the Python libraries used in this chapter are direct dependencies of Flair 0.11 and require no special setup, assuming Flair is already installed on your machine. Code examples covered in this chapter are found in this book's official GitHub repository in the following Jupyter notebook: https://github.com/PacktPublishing/Natural-Language-Processing-with-Flair/tree/main/Chapter05.

Running the code covered in this chapter on a GPU-enabled machine is not mandatory, though it is advised, as it will speed up the training process and allow you to train higher-quality models.

## The motivation behind training custom models

In this book we so far discussed the features and models that are available in Flair straight out of the box. However, if you work with NLP long enough, you will likely encounter a sequence labeling problem that is complex or specific enough that there will be no pre-trained models available out there. This can happen in either of the following situations:

- *The problem you are solving is domain-specific*: Sequence taggers such as **Named Entity Recognition (NER)** or **Part-of-Speech (PoS)** taggers are usually trained on large, generic corpora that are supposed to represent the general use of a language. But if our problem is domain-specific, it's likely that we will require a custom tagger with domain-specific labels trained on domain-specific corpora.
- *A pre-trained model exists but doesn't perform well enough*: Every model made available in Flair and the approaches used for training are usually reviewed by other contributors, but it's entirely possible you may be able to build a better model yourself. Chances for this increase as you move toward the less popular languages, since their performance will not be as optimized. It may also happen that the pre-trained model achieves better overall performance in terms of the F1-score or accuracy but may fall short in tagging that one specific label that you really care about.
