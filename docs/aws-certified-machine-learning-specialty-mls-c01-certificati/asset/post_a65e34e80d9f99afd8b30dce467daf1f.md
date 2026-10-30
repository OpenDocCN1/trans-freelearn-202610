
On the other hand, you can decide to go for oversampling, where you will create new observations/samples of the minority class. The simplest approach is the naïve one, where you randomly select observations from the training set (with replacement) for duplication. The downside of this method is the potential issue of overfitting, since you will be duplicating/highlighting the observed pattern of the minority class.

To either underfit or overfit of your model, you should always test the fitted model on your testing set.

> **Important note**
> The testing set cannot be under/over sampled: only the training set should pass through these resampling techniques.

You can also oversample the training set by applying synthetic sampling techniques. Random oversample does not add any new information to the training set: it just duplicates the existing ones. By creating synthetic samples, you are deriving those new observations from the existing ones (instead of simply copying them). This is a type of data augmentation technique known as the **Synthetic Minority Oversampling Technique (SMOTE)**.

Technically, what SMOTE does is plot a line in the feature space of the minority class and extract points that are close to that line.

> **Important note**
> You may find questions in your exam where the term SMOTE has been used. If that happens, keep the context where this term is applied in mind: oversampling.

Now, let's move on to the next topic regarding data preparation, where we will learn how to prepare text data for machine learning models.

## Dealing with text data

We have already learned how to transform categorical features into numerical representations, either using label encoders, ordinal encoders, or one-hot encoding. However, what if we have fields containing long piece of text in our dataset? How are we supposed to provide a mathematical representation for them in order to properly feed ML algorithms? This is a common issue in **natural language processing (NLP)**, a subfield of AI.

NLP models aim to extract knowledge from texts; for example, translating text between languages, identifying entities in a corpus of text (also known as **Name Entity Recognition (NER)**), classifying sentiments from a user review, and many other applications.

**Important note**
In *Chapter 2, AWS Application Services for AI/ML*, you learned about some AWS application services that apply NLP to their solutions, such as Amazon Translate and Amazon Comprehend. During the exam, you might be asked to think about the fastest or easiest way (with the least development effort) to build certain types of NLP applications. The fastest or easiest way is usually to use those out of the box AWS services, since they offer pre-trained models for some use cases (especially machine translation, sentiment analysis, topic modeling, document classification, and entity recognition).

In a few chapters’ time, you will also learn about some built-in AWS algorithms for NLP applications, such as BlazingText, **Latent Dirichlet Allocation (LDA)**, **Neural Topic Modeling**, and the Sequence-to-Sequence algorithm. Those algorithms also let you create the same NLP solutions that are created by those out of box services; however, you have to use them on SageMaker and write your own solution. In other words, they offer more flexibility, but demand more development effort.

Keep that in mind for your exam!

Although AWS offers many out of the box services and built-in algorithms that allow us to create NLP applications, we will not look at those AWS product features now (as we did in *Chapter 2, AWS Application Services for AI/ML*, and will do so again in *Chapter 7, Applying Machine Learning Algorithms*). We will finish this chapter by looking at some data preparation techniques that are extremely important for preparing your data for NLP.

## Bag of words

The first one we will cover is known as **bag of words (BoW)**. This is a very common and simple technique, applied to text data, that creates matrix representations to describe the number of words within the text. BoW consists of two main steps: creating a vocabulary and creating a representation of the presence of those known words from the vocabulary in the text. These steps can be seen in the following diagram:

This movie is really good, although really old production

- Data cleansing
- Lower case
- Tokenization
- Remove punctuation
- Remove non-alphabetical tokens
- Remove stop words

[this, movie, really, good, although, really, old, production]

Vocabulary
{
(this, 1),
(movie, 1),
(really, 2),
(good, 1),
(although, 1),
(old, 1),
(production, 1),
}

| this | movie | really | good | although | old | production |
|---|---|---|---|---|---|---|
| Binary | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| Frequency | 0.14 | 0.14 | 0.28 | 0.14 | 0.14 | 0.14 | 0.14 |
| Count | 1 | 1 | 2 | 1 | 1 | 1 | 1 |

Figure 3.23 – BoW in action

First things first, we usually can't use raw texts to prepare a bag of words representation. There is a data cleansing step where we will lowercase the text; split each word into tokens; remove punctuation, non-alphabetical, and stop words; and, whenever necessary, apply any other custom cleansing technique you may want.

Once you have cleansed your raw text, you can add each word to a global vocabulary. Technically, this is usually a dictionary of tuples, in the form of (word, number of occurrences); for example, {(apple, 10), (watermelon, 20)}. As I mentioned previously, this is a global dictionary, and you should consider all the texts you are analyzing.

Now, with the cleansed text and updated vocabulary, we can build our text representation in the form of a matrix, where each column represents one word from the global vocabulary and each row represents a text you have analyzed. The way you represent those texts on each row may vary according to different strategies, such as binary, frequency, and count. Let's dive into these strategies a little more.

In the preceding diagram, we are processing a single text but trying the three different strategies for bag of words. That's why you can see three rows on that table, instead of just one (in a real scenario, you have to choose one of them for implementation).

In the first row, we have used a binary strategy, which will assign 1 if the word exists in the global vocabulary and 0 if not. Because our vocabulary was built on a single text, all the words from that text belong to the vocabulary (the reason you can only see 1s in the binary strategy).

In the second row, we have used a frequency strategy, which will check the number of occurrences of each word within the text and divide it by the total number of words within the text. For example, the word "this" appears just once (1) and there are seven other words in the text (7), so 1/7 is equal to 0.14.

Finally, in the third row, we have used a count strategy, which is a simple count of occurrences of each word within the text.

**Important note**
This note is really important – you are likely to find it in your exam. You may have noticed that our BoW matrix contains **unique words** in the *columns* and **each text** representation is in the *rows*. If you have 100 long texts with only 50 unique words across them, your BoW matrix will have 50 columns and 100 rows. During your exam, you are likely to receive a list of texts and be asked to prepare the BoW matrix.

There is one more extremely important concept you should know about BoW, which is the **n-gram** configuration. The term n-gram is used to describe the way you would like to look at your vocabulary, either via single words (uni-gram), groups of two words (bi-gram), groups of three words (tri-gram), or even groups of n words (n-gram). So far, we have seen BoW representations using a uni-gram approach, but more sophisticated representations of BoW may use bi-grams, tri-grams, or n-grams.

The main logic itself does not change, but you need to know how to represent n-grams in BoW. Still using our example from the preceding diagram, a bi-gram approach would combine those words in the following way: [this movie, movie really, really good, good although, although old, old production]. Make sure you understand this before taking the exam.

**Important note**
The power and simplicity of BoW comes from the fact that you can easily come up with a training set to test your algorithms, or even create a baseline model. If you look at *Figure 3.23*, can you see that having more data and just adding a classification column to that table, such as good or bad review, would allow us to train a binary classification model to predict sentiments?
