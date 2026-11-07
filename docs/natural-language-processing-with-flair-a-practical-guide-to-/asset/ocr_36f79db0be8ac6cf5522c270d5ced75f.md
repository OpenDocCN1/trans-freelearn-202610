# Chunking

Another NLP technique closely related to POS tagging is **chunking**. Unlike **parts of speech (POS)**, where we identify individual POS, in chunking we identify complete short phrases such as noun phrases. In *Figure 1.2*, the phrase *A lovely day* can be considered a chunk as it is a noun phrase, and in its relationship to other words works the same way as a noun.

# Stemming and lemmatization

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

# Text classification

Text classification is an NLP technique used to assign a text or a document to one or more classes or document types. Practical uses for text classification include spam filtering, language identification, sentiment analysis, and programming language identification from syntax.