
is that the vector captures the semantic meaning and the context as well, which ultimately serves the end user experience.

If you’re curious, in the situation where an e-commerce customer would search for “wireless headphones,” the organization would represent user queries as vectors in a high-dimensional space and use a pre-trained model such as *word2vec* to convert each word in the query into a vector representation. The combined result would represent the entire query. This enables us to compare the vector to the vector space built out of the product representation. The following is a 10-dimensional vector that gives us an idea of the representation:

```
[0.213, -0.178, 0.425, -0.067, 0.321, 0.195, -0.267, 0.132, -0.455, -0.033]
```

What’s interesting here is the fact that the query is suddenly converted into a space of possibility, which allows us to compare vector space, instead of keywords, to determine similarities. Does the product exist? What type of product is being searched for? The semantics that are extracted from a query are fascinating.

## Applying ML

Another level of AI-based search experience is training the data with models such as **convolutional neural networks (CNNs)**, **recurrent neural networks (RNNs)**, or **transformers**. This is done on the previously vectorized data to learn and understand patterns and relationships between data. The goal of this phase is to prepare the search application so that it can support NLP.

To continue our example of wireless headphones, using models such as CNNs or RNNs allows us to browse the graph of connections this specific article could have. In this case, think about features such as noise cancellation, battery life, whether they’re waterproof, or whether they stay in the wearer’s ears during their burpee session.

## Integrating personalization

Without personalization, the search, whether smarter than a keyword search or not, would stay cold. Personalization allows you to integrate context into the search based on past searches and similar searches. These are known as network profiling and user profiling, respectively.

The beauty of CNNs/RNNs/transformers is that the benefits don’t stop at what we mentioned previously – they help provide recommendations to users based on their past searches.

## Getting into production

We would love to tell you that AI-based search is solving the back and forth that a search application requires from a continuous optimization perspective between relevancy and latency performance, but that’s not the case.

The key to having the right data is to make the right decision and limit the optimization effort, which means that not only do you need to be able to fully monitor the user experience, but you also need “interesting events” that let you understand the search quality.

You will need some level of A/B testing for that and compare the performance of a given state to a new state of the search engine, such as after tuning the model. However, you will also need some sort of feedback loop to understand the pertinence of the results.

Once you have built an AI-based search system using the high-level steps I mentioned earlier, you can further enhance the system by incorporating techniques such as NER, sentiment analysis, text classification, Q/A, and text summarization. We’ll learn more about these techniques next.

## Named Entity Recognition (NER)

NER is a component of NLP that detects and classifies named entities within unstructured text. Entities can be names, brands, locations, or organizations that have a unique name or identifier.

With this technique in place, the unstructured data flowing into the system is classified, which is typically the result of an ML model that’s been upstream-trained on annotated data. In this process, the data is labeled with the corresponding type. Once trained, the ML model uses the labels to learn patterns and relationships in the text, allowing it to accurately identify and categorize named entities in new, unseen text.

One of the use cases we can think about is **personally identifiable information (PII)**, which refers to sensitive information flowing into the system without any control. This information can be names, addresses, social security numbers, and financial information.

For a bank, being unable to detect PII flowing into its data could have serious consequences for both the bank and its customers. For example, if a hacker manages to gain access to the bank’s data and steals PII, they can use it to commit fraud and identity theft, which could result in financial losses for customers and penalties for the bank since they are subject to regulations.

That is why financial services companies use NER to mitigate the risk of PII exposure, employing it either on stale data or inflight data during processing. The result is redacting, encrypting, masking, or setting the right privilege to access the data, thus avoiding the legal and reputational risks associated with PII exposure.

## Sentiment analysis

Another technique of NLP is sentiment analysis, which aims to identify and extract the sentiment in an unstructured text in terms of emotion. It’s commonly used in social networks, such as for extracting emotions from tweets. Often, organizations use it to understand what sentiment relates to their brand.

However, while the usage of sentiment analysis is broad, it is also useful for much more targeted use cases such as analyzing product reviews in real time or monitoring the market sentiment relative to new investments. This helps financial analysts make more informed investments.

The worst that can happen to a company today is failing to detect negative sentiment toward their products or services, which could lead them to suffer reputational damage and customer churn.

The natural language Python toolkit (https://www.nltk.org/) comes in handy for this very popular task. The following code example shows how to implement a very simple sentiment analysis and what the results could be:

```
import nltk
nltk.download('vader_lexicon')

from nltk.sentiment import SentimentIntensityAnalyzer
sia = SentimentIntensityAnalyzer()
text = "I really enjoyed the new movie. The acting was great and the plot was engaging."
scores = sia.polarity_scores(text)
print(scores)
```

You can find the preceding code and run it in this notebook: https://github.com/PacktPublishing/Vector-Search-for-Practitioners-with-Elastic/blob/main/chapter1/sentiment-analysis.ipynb. The Python code uses SentimentIntensityAnalyzer to perform the actual sentiment analysis. The polarity score is then returned with a score for each polarity (negative, neutral, and positive), as well as a compound score that goes from -1 (most negative) to +1 (most positive). Running the notebook should give you the following result:

```
{'neg': 0.0, 'neu': 0.522, 'pos': 0.478, 'compound': 0.8777}
```

The score states that the emotion that was captured in the text is very positive!

## Text classification

Text classification is another NLP technique that assigns predefined categories to unstructured data. A real-life example that affects us all is spam management or the action of filtering and tagging emails as spam or non-spam.

Spam emails can have many signals that would make them suspicious, such as the use of certain keywords, typos, links, and more. Once a model has been trained on a large dataset of labels, emails are classified as spam or non-spam automatically. Multiple values result from this, such as an overall better email experience or increased productivity.

For the sake of simplification, here is an example that uses an online data source and shows classification results, alongside its accuracy:

```
from sklearn.datasets import fetch_20newsgroups
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import accuracy_score
# Load the dataset
newsgroups_train = fetch_20newsgroups(subset='train')
newsgroups_test = fetch_20newsgroups(subset='test')
# Vectorize the text data
vectorizer = CountVectorizer()
X_train = vectorizer.fit_transform(newsgroups_train.data)
X_test = vectorizer.transform(newsgroups_test.data)
# Train a Naive Bayes classifier
clf = MultinomialNB()
clf.fit(X_train, newsgroups_train.target)
# Predict on the test set
y_pred = clf.predict(X_test)
# Print the accuracy and predicted classes
print(f"Accuracy: {accuracy_score(newsgroups_test.target, y_pred)}")
print(f"Predicted classes: {y_pred}")
```

Executing the code in a Google Colab notebook should give you the following output:

```
Accuracy: 0.7728359001593202
Predicted classes: [ 7 11 0 ... 9 3 15]
```

Let's understand what we are achieving in the preceding code:

- As we mentioned previously, the code uses an online dataset called the 20 Newsgroup dataset (http://qwone.com/~jason/20Newsgroups/). It's a great playground to test this type of classification as it regroups 20,000 newsgroup documents, partitioned into about 20 different newsgroups. This dataset was assembled by Ken Lang at Carnegie Mellon University.
- We use scikit-learn, better known as sklearn, a very popular and adopted open source ML library for Python. Because it provides a wide range of supervised and unsupervised learning algorithms such as classification, regression, clustering, and dimensionality reduction, we will use it as a tool in this book.
- We use a vector of word count techniques in this example via the CountVectorizer function, which essentially converts text data into a numeric format that can be used for ML.
- We train a naïve Bayes classifier on the vectorized training data. In this example, the classifier assumes that the frequency of each word in a document is independent of the frequency of all other words in the document. The assumption is to make predictions about the class of new documents.
