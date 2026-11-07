
## 10

## Hands-On Exercise – Building a Trading Bot with Flair

| Left | Page | Right | Page |
|---|---|---|---|
| Technical requirements | 160 | Implementing the Flair trading bot | 162 |
| Understanding the trading strategy | 160 | A Flair coding cheat sheet | 166 |
|  |  | Summary | 167 |

## Index

## Other Books You May Enjoy

## Preface

Natural Language Processing (NLP) is currently one of the fastest-growing subfields of AI where new advances or changes to NLP frameworks happen daily. Anyone new to the field will find it difficult to know where to start, which ideas are outdated, which ones are still relevant, and which are soon to become state of the art. Similarly, people with a solid theoretical NLP background aiming to gain practical experience in NLP will find it difficult to choose the right NLP library or framework given the oversupply of different options currently available in the open source community. As someone who has used Flair both professionally as well as for personal projects, I would find it hard to recommend any other framework that is as intuitive, as relevant, and as packed with ready-to-use, state-of-the-art models as Flair.

The most interesting thing about Flair may not be its simplicity, performance, or ease of use. Instead, it has to do with how Flair was originally designed. The first release was never intended to serve as a full-fledged NLP framework. The first release of Flair (v0.1), at heart, was merely a tool that served as a real-world implementation of Flair embeddings – the underlying concept that gives Flair sequence labeling models their amazing performance. It wasn’t until later Flair versions that the library introduced Transformer models, text classification models, and other tools that you would normally expect from an established NLP framework.

Anyone aiming to build real-world NLP solutions will need to juggle between spending time learning more about NLP theory and investing effort in choosing the right engineering approach. This book will help you find the right balance between both. It will arm you with just the right amount of theoretical foundation to help understand the underlying NLP concepts as well as help you gain enough engineering knowledge and experience so that you can use Flair proficiently.

## Who this book is for

This book is for anyone eager to learn about NLP through one of the most beginner-friendly, yet powerful, Python NLP libraries out there. The audience includes software engineering students, developers, data scientists, and anyone who is transitioning into NLP and is interested in learning about practical approaches to solving problems with Flair. This book, however, is not recommended for readers aiming to get an in-depth theoretical understanding of the mathematics behind NLP. Beginner-level knowledge of Python is recommended to get the most out of this book.

The book is also an opportunity for anyone hoping to start contributing to the open source community. You could fix bugs, create new tutorials, expand documentation, add support for new languages by building pre-trained models, or simply open new GitHub issues with suggestions about improving Flair. The knowledge gained by reading and doing the exercises in this book will help you considerably on your way to becoming an active Flair contributor.

## What this book covers

- *Chapter 1, Introduction to Flair,* provides a quick overview of NLP and its basic problems and techniques. The chapter then introduces Flair and shows how to set up your local environment.
- *Chapter 2, Flair Base Types,* introduces Flair’s basic syntax, its typical classes, and methods.
- *Chapter 3, Embeddings in Flair,* explains the concept behind word and document embeddings and their role in NLP. It describes all the different types of embeddings available in Flair.
- *Chapter 4, Sequence Tagging,* describes sequence tagging and its subtypes, such as named entity recognition and part-of-speech tagging. This chapter also demonstrates how to use pre-trained sequence taggers in Flair.
- *Chapter 5, Training Sequence Labeling Models,* explains how to train, save, and use custom sequence tagging models in Flair.
- *Chapter 6, Hyperparameter Optimization in Flair,* shows the importance of using the right hyperparameters for model training. It introduces hyperparameter optimization tools in Python and explains how to perform hyperparameter optimization in Flair.
- *Chapter 7, Train Your Own Embeddings,* explains how to train custom embeddings in Flair and how to leverage different evaluation techniques for measuring success.
- Chapter 8, Text Classification in Flair, introduces the problem of text classification. This chapter demonstrates how to use pre-trained models as well as how to train custom classifiers. It also introduces a novel approach to text classification called TARS.
- Chapter 9, Deploying and Using Models in Production, talks about the challenges of deploying and using NLP models in production. This chapter demonstrates how to set up custom minimum viable product NLP services and how to host Flair models on the Hugging Face models hub.
- Chapter 10, Hands-On Exercise – Building a Trading Bot with Flair, solves a real-world problem as part of a hands-on exercise by building a trading bot with Flair.

## To get the most out of this book

To run the code in this book, you will need a Python environment version 3.6 or later. It is highly recommended you run the code as part of a Jupyter notebook session, which will allow you to experiment and try out different variations of the original code more easily. To bypass the local setup, you can also run the code in a hosted Jupyter notebook environment such as Google Colab. You will also need the following Python packages:

| Python packages needed to run the code in this book | Operating system requirements |
|---|---|
| Flair 0.11 | Windows, macOS, or Linux |
| Flask 2.0.3 | Windows, macOS, or Linux |

Detailed instructions about how to set up your local environment and install the Python packages can be found in Chapter 1, Introduction to Flair.

If you are using the digital version of this book, we advise you to type the code yourself or access the code from the book's GitHub repository (a link is available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.

## Download the example code files

You can download the example code files for this book from GitHub at https://github.com/PacktPublishing/Natural-Language-Processing-with-Flair/. If there's an update to the code, it will be updated in the GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Download the color images

We also provide a PDF file that has color images of the screenshots and diagrams used in this book. You can download it here: https://static.packt-cdn.com/downloads/9781801072311_ColorImages.pdf.

## Conventions used

There are a number of text conventions used throughout this book.

Code in text: Indicates code words in the text, variable names, class names, object methods, and properties.

A block of code is set as follows:

```
from flair.data import Sentence
from flair.tokenization import SpaceTokenizer

tokenizer = SpaceTokenizer()
s = Sentence('Some nice text.', use_tokenizer=tokenizer)

print(s)
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items are set in bold:

```
from flair.data import Sentence

sentence = Sentence('A short sentence')
sentence.get_token(1).add_label('manual-pos', 'DT')

print(sentence)
```

Any command-line input or output is written as follows:

```
$ python3 --version
$ pip3 install flair==0.11
```

**Keyword**: Indicates a new term, an important name, or a concept of great significance; for example, **Hugging Face** transformer models.
