
unhealthy ecosystems through our spectral clustering algorithm. A human reviewing results would not have to switch many labels (20%) to generate correct training labels for this data to be used to train a classification model. The second column of our dataset includes correct tone classification labels. Let’s convert our BERT embeddings to a DataFrame by adding to Script 5.2:

```
#create training dataset for supervised learning
#turn BERT embedding into array
BERT_array1=np.array([x for x in encoded_text1])

#convert to dataframes
BERT_df1=pd.DataFrame(BERT_array1)
```

Now, let’s split our data into training and test sets (80% and 20% splits, respectively), create a k-nearest neighbors classifier (which classifies points according to the labels of their two nearest neighbors with k = 2), and measure the test set error by adding to Script 5.2:

```
#create KNN classifier and test accuracy
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import train_test_split

#get predictors and outcome
BERT_df1['Type']=mydata['Type']
df_train, df_test = train_test_split(BERT_df1,test_size=0.2, random_state=0)
X = df_train.iloc[:,0:767]
y = df_train.iloc[:,768]
X_test = df_test.iloc[:,0:767]
y_test = df_test.iloc[:,768]

#create KNN classifier and print accuracy
eu=KNeighborsClassifier(n_neighbors=2,metric='euclidean')
eu.fit(X,y)
print(eu.score(X_test,y_test))
```

Our split yields an accuracy of 100%, suggesting no misclassification of ecosystem types. Yours may vary given seeds and random number generators on your machine. However, this approach seems to work well for this dataset. Our pipeline of processing text data via embeddings, deriving a good start to label creation via spectral clustering, and then creating a classifier model yielded a high-accuracy classification system for our dataset.

## Summary

In this chapter, we learned how to collect ecological data for a variety of data science problems. After a brief introduction to the theory of spectral clustering, we showed how spectral clustering could parse out different animal populations through our Gaboon viper distribution example. Finally, we explored spectral clustering of nearest neighbor networks that can be used in semi-supervised learning pipelines through an ecosystem note data example. In Chapter 6, we’ll introduce centrality measurements and use them to find tipping points in stock pricing.

## References

Angelici, F. M., Effah, C., Inyang, M. A., & Luiselli, L. (2000). A preliminary radiotracking study of movements, activity patterns and habitat use of free-ranging Gaboon vipers, Bitis gabonica. Revue d’Ecologie, Terre et Vie, 55(1), 45-55.

Corrales, D. C., Figueroa, A., Ledezma, A., & Corrales, J. C. (2015). An empirical multi-classifier for coffee rust detection in Colombian crops. In Computational Science and Its Applications--ICCSA 2015: 15th International Conference, Banff, AB, Canada, June 22-25, 2015, Proceedings, Part I 15 (pp. 60-74). Springer International Publishing.

Froese, G. Z., Ebang Mbélé, A., Beirne, C., Atsame, L., Bayossa, C., Bazza, B., ... & Poulsen, J. R. (2022). Coupling paraecology and hunter GPS self-follows to quantify village bushmeat hunting dynamics across the landscape scale. African Journal of Ecology, 60 (2), 229-249.

Mutombo, F. K. (2012). Long-range interactions in complex networks.

Ng, A., Jordan, M., & Weiss, Y. (2001). On spectral clustering: Analysis and an algorithm. Advances in neural information processing systems, 14.

Nunez-Mir, G. C., Iannone III, B. V., Pijanowski, B. C., Kong, N., & Fei, S. (2016). Automated content analysis: addressing the big literature challenge in ecology and evolution. Methods in Ecology and Evolution, 7 (11), 1262-1272.

Qi, X., Fuller, E., Wu, Q., Wu, Y., & Zhang, C. Q. (2012). Laplacian centrality: A new centrality measure for weighted networks. Information Sciences, 194, 240-253.

Reimers, N., & Gurevych, I. (2019). Sentence-bert: Sentence embeddings using siamese bert-networks. arXiv preprint arXiv:1908.10084.

White, S., & Smyth, P. (2005, April). A spectral clustering approach to finding communities in graphs. In Proceedings of the 2005 SIAM international conference on data mining (pp. 274-285). Society for industrial and applied Mathematics.

## Part 3:
Temporal Data Applications

Part 3 introduces temporal data applications, including problems related to stock market volatility prediction, change-point analysis of goods prices and sales volumes, and epidemic spread through dynamic animal interaction networks. In this part, we will get an overview of vertex- and edge-based centrality metrics, extensions of network data structures to simplicial complexes, triadic closure, and dynamic epidemic models.

Part 3 has the following chapters:

- Chapter 6, Stock Market Data
- Chapter 7, Goods Prices/Sales Data
- Chapter 8, Dynamic Social Networks

## Introduction to temporal data

In *Chapter 2*, we briefly introduced temporal data or data in the form of a time series or a group of time series. **Time series data** tracks important metrics in many different industries: daily store sales volumes, weekly software product marketing lead volumes, daily incidence of an emerging disease, yearly behavior rates (such as smoking or vegetable consumption) in a population, or hourly stock prices tracking market trends. Many related factors can influence trends over time, and some models consider these factors directly if they are known in advance.

However, consider the case of sales trends for a new gem store in a city where gem stores are a new phenomenon, perhaps somewhere rural between Haifa and Tel Aviv (*Figure 6.1*). Thus, there is very little known about what might influence sales. Understanding what trends exist in the time series data is critical when mining for factors that might influence sales. However, time series datasets pose significant challenges to many supervised learning methods, such as **random forest models or linear regression**. At one point in time, sales are not independent; they rely on factors that influence sales in the previous days, limiting the use of supervised learning and many types of **unsupervised learning**. The lack of predictors also poses a challenge.

![](img/71ff737206f7d80bb625b3c9aca9dc7f_122_0.png)

Figure 6.1 – An illustration of a gem store located partway between Tel Aviv and Haifa, Israel

Fortunately, for our gem store, many algorithms are designed to handle time series data, such as **autoregressive integrated moving average models (ARIMA models)**, **singular spectrum analysis (SSA)**, and **Holt–Winters models**. However, changes in time series behavior (spikes, crashes, and changes in variance) pose a challenge to these models. Capturing and predicting these changes is critical if you want to create a predictive model or mine for factors influencing the time series values. In our gem store example, seasonality in tourism, conflicts in the region, and holiday travel promotions may influence traffic along the route in which our store is positioned.

One industry with abundant and very complex time series data is finance. Many social and economic factors influence stock prices, and untangling the relationships and randomness in stock price fluctuations underlies much of the finance industry. Let’s have a look at the stock market pricing data and common tasks in analyzing stock market pricing data.

## Stock market applications

In recent years, the financial sector has shifted from an expert-driven model of stock market insight to a more machine learning-based approach. Machine learning models sift out emerging trends and catch subtle trends that may escape a human pouring over the data. This tactic also allows analysts to process a much larger data collection than a human could process, including many different sectors, international stock exchanges, and even individual frontier markets. By collecting more insight, it is possible for investors and investment management firms to invest in a wider variety of markets without as much expertise in those markets.

Stock market analytics covers a vast field of applications. Investments can be made within certain market sectors (such as technology or agriculture) or across markets. They can focus on short-term gains (including those made in minutes or hours) and long-term gains (which may span decades). They can also focus on foreign markets, where stock prices may be influenced by factors very different from those influencing a local market’s prices. All these scenarios guide analytics efforts and the time scale of data collected for analysis.

In general, analyzing stock market data involves assessing many types of trends over time. Stocks can have constant prices over time, experience gradual price growth and reduction, crash suddenly, or grow exponentially. Each of these suggests a different purchase/sale strategy for investors in the short term and the long term. Figure 6.2 shows a hypothetical stock that exhibits many of these patterns:
