
The following figure illustrates NER using colored backgrounds and tags associated with the words:

![](img/c56c48b8fd18a11ab705352d618ed976_25_0.png)

Figure 1.1 – Visualization of NER tagging

In the previous example, we can see that three entities were identified and tagged. The first and third tags are particularly interesting because they both represent the same word, Berkeley, yet the first one clearly refers to an organization whereas the third one refers to a geographic location. The human brain is excellent at distinguishing between different entity types based on context and is able to do so almost seamlessly, whereas machines have struggled with it for decades. Recent advancements in contextual string embeddings, an essential part of Flair, made a huge leap forward in solving that.

## Word-sense disambiguation

Word-Sense Disambiguation (WSD) is an NLP technique concerned with identifying the intended sense of a given word with multiple meanings.

For example, take the given sentence:

*George tried to return to Berlin to return his hat.*

WSD would aim to identify the sense of the first use of the word return, referring to the act of giving something back, and the sense of the second return, referring to the act of going back to the same place.

## Part-of-speech tagging

Part-of-Speech (POS) tagging is a technique closely related to both WSD and NER that aims to tag the words as corresponding to a particular part of speech such as nouns, verbs, adjectives, adverbs, and so on.

![](img/c56c48b8fd18a11ab705352d618ed976_25_1.png)

Figure 1.2 – Visualization of POS tagging

Actual POS taggers provide a lot more information with the tags than simply associating the words with noun/verb/adjective categories. For example, the Penn Treebank Project corpus, one of the most widely used NER corpora, distinguishes between 36 different types of POS tags.

## Chunking

Another NLP technique closely related to POS tagging is **chunking**. Unlike **parts of speech (POS)**, where we identify individual POS, in chunking we identify complete short phrases such as noun phrases. In *Figure 1.2*, the phrase *A lovely day* can be considered a chunk as it is a noun phrase, and in its relationship to other words works the same way as a noun.

## Stemming and lemmatization

**Stemming** and **lemmatization** are two closely related text normalization techniques used in NLP to reduce the words to their common base forms. For example, the word *play* is the base word of the words *playing*, *played* and *plays*.

The simpler of the two techniques, **stemming**, simply accomplishes this by cutting off the ends or beginnings of words. This simple solution often works, but is not foolproof. For example, the word *ladies* can never be transformed into the word *lady* by stemming only. We therefore need a technique that understands the POS category of a word and takes into account its context. This technique is called *lemmatization*. The process of lemmatization can be demonstrated using the following example.

Take the following sentence:

```
this meeting was exhausting
```

Lemmatization reduces the previous sentence to the following:

```
this meeting be exhaust
```

It reduces the word *was* to *be* and the word *exhausting* to *exhaust*. Also note that the word *meeting* is used as a noun and it is therefore mapped to the same word *meeting*, whereas if the word *meeting* was used as a verb, it would be reduced to *meet*.

A popular and easy-to-use library for performing lemmatization with Python is **spaCy**. Its models are trained on large corpora and are able to distinguish between different POS, yielding impressive results.

## Text classification

Text classification is an NLP technique used to assign a text or a document to one or more classes or document types. Practical uses for text classification include spam filtering, language identification, sentiment analysis, and programming language identification from syntax.

Having covered the basic NLP concepts and terminology, we can now move on to understanding what Flair is and how it manages to solve NLP tasks with state-of-the-art results.

## Introducing Flair

Flair is a powerful NLP framework published as a Python package. It provides a simple interface that is friendly, easy to use, and caters to people from various backgrounds including those with little prior knowledge in programming. It is published under the MIT License, which is one of the most permissive free software licenses.

Flair as an NLP framework comes with a variety of tools and uses. It can be defined in the following ways:

- *It is an NLP framework used in NLP research for producing models that achieve state-of-the-art results across many NLP tasks such as POS tagging, NER, and chunking across several languages and datasets.* In Flair's GitHub repository, you will find step-by-step instructions on how to reproduce these results.
- *It is a tool for training, validating, and distributing NER, POS tagging, chunking, word sense disambiguation, and text classification models.* It features tools that help ease the training and validation processes such as the automatic corpora downloading tool, and tools that facilitate model tuning such as the hyperparameter optimization tool. It supports a growing number of languages.
- *It is a tool for downloading and using state-of-the-art pre-trained models.* The models are downloaded seamlessly, meaning that they will be automatically downloaded the first time you use them and will remain stored for future use.
- *It is a platform for the proposed state-of-the-art Flair embeddings.* The state-of-the-art results Flair achieves in many NLP tasks can by and large be attributed to its proposed Flair contextual string embeddings described in more detail in the paper *Contextual String Embeddings for Sequence Labeling*. The author refers to them as "the secret sauce" of Flair.
- *It is an NLP framework for working with biomedical data.* A special section of Flair is dedicated solely to working with biomedical data and features a set of pretrained models that achieve state-of-the-art results, as well as a number of corpora and comprehensive documentation on how to train custom biomedical tagging models.
- *It is a great practical introduction to NLP.* Flair's extensive online documentation, simple interface, inclusive support for a large number of languages, and its ability to perform a lot of the tasks on non-GPU-equipped machines all make it an excellent entry point for someone aiming to learn about NLP through practical hands-on experimentation.

## Setting up the development environment

Now that you have a basic understanding of features offered by the framework, as well as an understanding of the basic NLP concepts, you are now ready to move to the next step of setting up your development environment for Flair.

To be able to follow the instructions in this section, first make sure you have Python 3.6+ installed on your device as described in the Technical requirements section.

## Creating the virtual environment

In Python, it's generally good practice to install packages in virtual environments so that the project dependencies you are currently working on will not affect your global Python dependencies or other projects you may work on in the future.

We will use the venv tool that is part of the Python Standard Library and requires no installation. To create a virtual environment, simply create a new directory, move into it, then run the following command:

```
$ python3 -m venv learning-flair
```

Then, to activate the virtual environment on Linux or macOS, run the following:

```
$ source learning-flair/bin/activate
```

If you are running Windows, run the following:

```
$ learning-flair\Scripts\activate.bat
```

Your command line should become prefixed with (learning-flair) $ and your virtual environment is now active.

## Installing a published version of Flair in a virtual environment

You should now be ready to install Flair version 0.11 with this single command:

```
(learning-flair) $ pip install flair==0.11
```

The installation should now commence and finish within a minute or so depending on the speed of your internet connection.
