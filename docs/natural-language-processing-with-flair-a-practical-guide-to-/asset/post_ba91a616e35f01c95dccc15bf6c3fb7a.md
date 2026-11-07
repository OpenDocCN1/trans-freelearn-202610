
```
tag_type = 'pos'
# downsample the corpus to 1% of original size
corpus.downsample(0.01)
```

In the preceding code snippet, we load the English corpus into the corpus variable, define our tag type as PoS tagging, and then downsample the corpus to 1% of its original size to speed up the training process for demonstration purposes.

We can then define the search space using the hp.choice parameter expressions:

```
from flair.embeddings import WordEmbeddings
from flair.hyperparameter.param_selection import \
    SearchSpace, Parameter
from hyperopt import hp

search_space = SearchSpace()
search_space.add(Parameter.HIDDEN_SIZE,
                 hp.choice,
                 options=[128, 256])
search_space.add(Parameter.LEARNING_RATE,
                 hp.choice,
                 options=[0.05, 0.1, 0.15, 0.2])
search_space.add(Parameter.EMBEDDINGS,
                 hp.choice,
                 options=([WordEmbeddings('en')]))
```

The preceding code snippet defines our search space, consisting of three hyperparameters. We decided to come up with some likely useful values for the learning rate and the model's hidden size. Although using parameter expressions such hyperopt.hp.quniform and hyperopt.hp.uniform would have likely yielded better results, they would also have vastly increased the search space size. Our search space is now pretty small. There are only eight possible hyperparameter combinations (two times four because there are two hidden size parameter options and four learning rate options).

> **Important Note**
> When doing hyperparameter optimization, some hyperparameters in Flair are mandatory. This includes the `Parameter.EMBEDDINGS` and `Parameter.LEARNING_RATE` parameters. If you do not wish to optimize these parameters, simply pass a single option to the search space for those hyperparameters.

We are now ready to define our SequenceTaggerParamSelector object:

```
from flair.hyperparameter.param_selection import \
    SequenceTaggerParamSelector

param_selector = SequenceTaggerParamSelector(corpus,
                                              tag_type,
                                              'paramopt')
```

This defines our hyperparameter optimization object using the corpus we loaded, the PoS tag type that we defined, and the `paramopt` name of the folder (relative to the directory from which we are running the code) to which the hyperparameter tuning results will be written.

We now simply have to start the optimization process by passing in the search space and the number of evaluations to perform. In our case, the search space is discrete and the number option is clearly defined – 8. However, in most cases, when using non-discrete parameter values, the number of possible hyperparameter combinations will be non-finite, and the number of evaluations will need to be chosen empirically:

```
param_selector.optimize(search_space, max_evals=8)
```

The preceding code will train 8 different sequence taggers using 8 different hyperparameter combinations. It will use loss as the `objective` function and evaluate it on the development dataset. It will train each run for 50 epochs. Flair's implementation of Hyperopt uses the TPE optimizer. The execution should take somewhere between 10 and 30 minutes on an average CPU.

When hyperparameter optimization completes, the results will be written out to `paramopt/param_selection.txt`. The file contains the details of each evaluation run and the following summary:

```
best parameter combination
    embeddings: 0
hidden_size: 1
learning_rate: 3
```

The preceding summary indicates that the best hidden size is the second option (because the summary says 1 and we use zero indexing, meaning 1 refers to the second option): 256. The best learning rate is the fourth option – 0.2. For embeddings, of course, because we only had a single option to choose from, the first and the only option was chosen as the best.

Does this mean that we should always use 256 as the hidden size and 0.2 as the optimal learning rate for English? In short, no. Optimal hyperparameter values will depend on the data used, the problem attempting to be solved, and the techniques used to tackle it. Remember the preceding code snippet where we loaded the corpus and reduced it from 100% to 1% of its original size? By doing this, we greatly increased the speed of model training, which allowed us to perform hyperparameter optimization much quicker, but by doing so, we also focused our model on a very small and potentially biased subset of data. To perform proper hyperparameter tuning on an English dataset, we'd need to do it at 100% corpus size and consider more hyperparameter options. This would vastly increase our search space and execution time, but also improve the performance of our model. These findings lead us to the following takeaway.

## Summary

Hyperparameter tuning is an utterly useful technique for finding optimal hyperparameters, not just in sequence tagging, but across many areas of machine learning. However, it does so at a cost; it is incredibly expensive. In essence, it performs the training and evaluation process tens, hundreds, or potentially thousands of times. But in the real world, exploring each and every potentially useful hyperparameter value can take months to execute, and the search space is usually narrowed down to a finite set of potentially useful options. Hyperparameter tuning is, therefore, a process that does require a certain level of expert knowledge and experience that will help you achieve the correct trade-off between exploration (trying out different not-yet-explored options) and exploitation (narrowing down the parameter values to the ones that generally seem to work well).

As part of this chapter, we learned why hyperparameter tuning is useful, when to use it, and how. We learned how to use a hyperparameter tuning library in its purest form in Python as well as how to use it through Flair to tune sequence-labeling models.

In the next chapter, we will be covering yet another NLP problem where hyperparameter tuning proves to be useful – training embeddings.

## 7 Train Your Own Embeddings

The reason Flair sequence taggers yield such outstanding performance can mainly be attributed to its secret sauce – Flair embeddings. Their contextual design, the fact that they are character-based, and the way they can be used in the backward-forward configuration make them a perfect fit for use in sequence labeling tasks. But, up until this point in this book, we haven’t focused much on how these embeddings are actually trained.

In the previous chapter, where we covered model training and learned about word embeddings, we simply used the pre-trained embeddings that were available as part of the Flair Python package. But there are many Natural Language Processing (NLP) problems we may stumble upon where the pre-trained embeddings will not be sufficient.

When working with flair, you may find yourself dealing with a language that isn’t covered by Flair’s pre-trained embeddings yet, or you may simply need embeddings that are trained on some specific corpus. When this happens, you can make use of Flair’s built-in classes that allow you to train your own Flair embeddings, which can later be used to train sequence taggers or solve other downstream NLP tasks.

We’ll begin with explaining why and in what circumstances training custom embeddings may be useful. We’ll cover how Flair embeddings work and how they achieve such great performance on downstream sequence labeling tasks. We will then focus on methods for evaluating word embeddings. Finally, we’ll train our own embeddings as part of a hands-on exercise on the world’s smallest language.

In this chapter, we will cover training Flair word embeddings as part of the following sections:

- Understanding the how and why behind custom Flair embeddings
- Evaluating word embeddings
- Training Flair embeddings on the world's smallest language

## Technical requirements

The exercises in this chapter require Flair version 0.11, as well as the `requests` Python package, to be installed on your development machine.

The code examples covered in this chapter are found in this book's official GitHub repository in the following Jupyter notebook: `https://github.com/PacktPublishing/Natural-Language-Processing-with-Flair/tree/main/Chapter07.`

## Understanding the how and why behind custom Flair embeddings

Word embeddings play an important, if not essential, role in sequence tagging models' performance. The details of what embeddings are, is covered in *Chapter 3, Embeddings in Flair*. But, for the purposes of this chapter, it's important to understand that embeddings are essentially word representations most often found in the form of real-valued vectors. These vectors can then be used as input on a number of downstream tasks, such as **part-of-speech** (**PoS**) tagging and **named entity recognition** (**NER**). Let's first quickly cover how Flair generates embeddings and how they are trained.

## Why training embeddings rarely ever means training embeddings

The term **training embeddings** is very often a confusing term that dates back to the older methods, where the result of training embeddings was a set of word embeddings. This term makes less sense with Flair's way of training embeddings, and even less so given that the embeddings are contextual and character-based. For the more traditional types of embeddings, such as **GloVe**, once the embeddings are trained, we can simply memorize an embedding for each word in the dictionary. At this point, someone who doesn't fully understand the design of Flair embeddings might wonder the following:

*Given that Flair's embeddings are contextual, doesn't this mean that the process of training embeddings results in precomputing one embedding for each word in the dictionary for each possible context we may find the word in?*
