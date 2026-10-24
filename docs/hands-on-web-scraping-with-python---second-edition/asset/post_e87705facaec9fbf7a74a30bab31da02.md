
In the preceding code, we are going to predict weight based on the data available or X_test. The weight_pred prediction array can now be made using the lr model, the predict() method, and X_test with 20% of the records used to predict Weight:

```
weight_pred = lr.predict(X_test)
weight_pred
array([[461.4418054 ], [180.5058948 ], [801.33957378], [659.13744619 ], [544.68207521], [801.33957378], [360.85981271], [204.78430682], [731.97268228], [284.55623206], [232.53106343], [274.15119833]])
```

Similar to X_test values, we can provide the model predictor with a new value of Length3 and get the predicted result (Weight):

```
lr.predict(np.array([[20.5]])) [0] [0]
# 76.45555753771407
lr.predict(np.array([[29.5]]))
# array([[388.60656932]])
lr.predict([[29.9,]])
# array([[402.47994762]])
```

By having the prediction model ready, we can also use it to calculate the performance and evaluate the model itself using methods from sklearn.metrics. r2_score(0.86) is also interpreted as the accuracy score, which floats between 0 and 1 – the higher the score, the better the accuracy. This shows that the model is acceptable to predict outcomes:

```
mean_squared_error(y_test, weight_pred)
# 9910.93927217145
mean_absolute_error(y_test, weight_pred)
# 87.86230207025248
mean_squared_error(y_test, weight_pred, squared=False)
# 99.5537004443
np.sqrt(mean_squared_error(y_test, weight_pred))
# 99.5537004443
r2_score(y_test, weight_pred)
# 0.8681631783134378
```

> **Important note**
> The **Mean Squared Error (MSE)** value of 9910.9392 is the mean of the sum of the square of residuals. The RMSE value of 99.5537 is the square root of the MSE. The **Mean Absolute Error (MAE)** value of 87.8623 is the mean of the sum of the absolute values of residuals. In general cases, the MSE is always greater than the MAE.

We have explored SLR techniques in this section with code examples, as well as identifying various attributes from sklearn. In the next section, we will be using MLR.

## Multiple linear regression

MLR is used when there is a correlation between multiple independent variables and a single dependent variable. In this case, we want to check the correlation of values from the Width and Height columns with Weight for the Perch species in the dataset used in SLR in the Simple linear regression section.

We will now discuss certain MLR concepts using code, and here is where the code files are available: https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter11/Chapter11_multi_regression.ipynb.

inDepd contains independent variables collected from Perch, whereas depd is the Weight dependent variable:

```
inDepd = perch[['Width', 'Height']] # independent variables
depd = perch['Weight']                 # dependent variable
```

Again, 80% of the values are used for training and 20% for testing. Training and testing sets are created using the train_test_split() method of the sklearn.model_selection model:

```
X_train, X_test, y_train, y_test = train_test_split(inDepd, depd,
train_size=0.8, shuffle=True)
```

The mlr model is created and fitted or trained using the training dataset:

```
mlr = LinearRegression().fit(X_train, Y_train)
mlr.score(X_train, y_train)
0.9560246146854837
```

X_train contains multiple pieces of data to train the model, as seen in Figure 11.5:

|   | Width | Height |
|---|-------|--------|
| 113 | 6.7408 | 10.6091 |
| 86 | 3.4075 | 6.1100 |
| 96 | 3.7230 | 7.2930 |
| 75 | 2.6316 | 4.5924 |
| 106 | 4.2042 | 7.8204 |
| 101 | 4.1440 | 7.1680 |
| 74 | 2.4320 | 3.8240 |
| 76 | 2.9415 | 4.5880 |
| 119 | 7.1064 | 11.9286 |

Figure 11.5: X_train with multiple independent variables

After training, the mlr model is used to predict the data from X_test. It is also to be noted that r2_score is 0.81799, which is acceptable in MLR cases:

```
weight_pred_mlr = mlr.predict(X_test)
weight_pred_mlr
array([643.07827704, 96.06295718, 228.07992289, 76.27638592, 136.430909,
143.8607218, 163.98405844, -300.15848523,
446.23998044,  291.21291802,  944.3976018,  243.67945607])

r2_score(y_test, weight_pred_mlr)
0.817999125036539
```

Inputs for Width and Height can also be provided directly to mlr.predict(). For example, see the following code block:

```
mlr.predict(np.array([[3.45, 6.05]]))
array([154.53150904])

mlr.predict(np.array([[7.10, 11.22]]))
array([807.225824])
```

**Important note**
As seen in the code implementation in this section, scikit-learn possesses a large set of assets for various ML-related concepts. If users are seeking a large number of statistical and quantitative values from their ML models, then statsmodel and scipy are more applicable, because of the short code.

In the next section, we will use an NLP-related technique.

## Sentiment analysis

NLP is a subset of ML, and sentiment analysis is a subset of NLP. Using sentiment analysis, we can detect whether the sentiment of a text is positive, neutral, or negative. Sentiment analysis is done on text, reviews, feedback, and more.

This analysis helps to evaluate client or customer reviews and feedback and whether they are intended as positive or negative. The detected sentiment allows better marketing, and companies, e-commerce sites, banks, travel offices, and many more organizations can monitor and manage their strategies, marketing policy, product branding, customer needs, and more.

We will be using code and examples in the following sections that are available at https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter11/Chapter11_textblob_vader.ipynb.

The nltk Python library is one of the pioneers of NLP. There are plenty of ways that NLP can be used, such as classification. In the current case, we will be using the textblob and vaderSentiment libraries:

- textblob: This is a text-processing library, built on top of nltk. It can be used for various NLP aspects, such as POS tagging, sentiment analysis, lemmatizing, tokenizing, and text classification. To analyze the sentiment of text, textblob calculates and returns polarity and subjectivity as sentiment results:
  - polarity: Indicates the positive or negative sentiment of the text. The values range from -1 (very negative) to 1 (very positive), with 0 being neutral.
  - subjectivity: Indicates personal feeling. The values range from 0 (objective) to 1 (subjective).

- vaderSentiment: This is a social media sentiment analyzer. Similar to TextBlob, Valence Aware Dictionary and Sentiment Reasoner (VADER) is a rule-based sentiment analyzer. VADER has been trained with a large collection of social media texts. vaderSentiment returns these four elements:
  - compound: This is the valence score of words in the lexicon. Values range from -1 (extremely negative) to 1 (very positive).
  - pos: A positive value with a compound score of >=0.05.
  - neg: A negative value with a compound score of <=-0.05.
  - neu: A neutral sentiment with a compound score of >=-0.05 and a compound score of <0.05.

Sentiment analysis is done upon the quote_details.csv data, which was scraped from the http://quotes.toscrape.com/ URL in Chapter 8. The column named quote is only required in the current case. It's the only string or text element that will be used with textblob and vaderSentiment to analyze sentiment. Individual columns or groups of selected columns can only be read from a file by using the usecols parameter in pd.read_csv():

```
import pandas as pd
quotes = pd.read_csv("quote_details.csv",usecols=["quote"])
quotes['quote'][0]
'The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.'
```

With the content read to be explored as the quotes DataFrame, in the following examples, we will analyze the sentiment of the contents using two quite popular Python libraries.

quotes, as seen in Figure 11.6, now has enough information about sentiment, which can be analyzed:

```
quotes.head()
```

|  | quote | polarity | subjectivity |
|---|---|---|---|
| 0 | The world as we have created it is a process o... | 0.000000 | 0.000000 |
| 1 | It is our choices, Harry, that show what we tr... | 0.300000 | 0.750000 |
| 2 | There are only two ways to live your life. One... | 0.003788 | 0.625000 |
| 3 | The person, be it gentleman or lady, who has n... | -0.050000 | 0.800000 |
| 4 | Imperfection is beauty, madness is genius and ... | -0.277778 | 0.833333 |

Figure 11.6: DataFrame quotes with polarity and subjectivity

The details related to sentiment are obtained from the chosen dataset using textblob. While we use ready-to-use libraries for procedures like in this example, the result might not be more insightful compared to the result we can obtain using the dataset.

So, vaderSentiment will be used on the same dataset (quotes) in the next section, which will help generate more information and help in comparing the results obtained with textblob.

### Example 2 – using vaderSentiment

The SentimentIntensityAnalyzer class of vaderSentiment has the polarity_score() method, which accepts a string and returns four keys – pos, neg, neu, and compound. Each returned key’s value is collected and added to the original DataFrame, quotes:

```
positive, negative, neutral, compound=[[], [], [], []]
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()
for quote in quotes["quote"]:
    text = analyzer.polarity_scores(quote)
    positive.append(text['pos'])
    negative.append(text['neg'])
    neutral.append(text['neu'])
    compound.append(text['compound'])
quotes['positive']=positive
quotes['negative']=negative
quotes['neutral']=neutral
quotes['compound']=compound
```
