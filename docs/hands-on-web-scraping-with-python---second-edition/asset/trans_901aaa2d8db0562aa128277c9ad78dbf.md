
在前面的代码中，我们将基于可用数据或 X_test 来预测体重。现在可以使用 lr 模型、predict() 方法和 X_test 生成 weight_pred 预测数组，其中使用 20% 的记录来预测 Weight：

```
weight_pred = lr.predict(X_test)
weight_pred
array([[461.4418054 ], [180.5058948 ], [801.33957378], [659.13744619 ], [544.68207521], [801.33957378], [360.85981271], [204.78430682], [731.97268228], [284.55623206], [232.53106343], [274.15119833]])
```

与 X_test 值类似，我们可以向模型预测器提供一个 Length3 的新值，并获得预测结果（Weight）：

```
lr.predict(np.array([[20.5]])) [0] [0]
# 76.45555753771407
lr.predict(np.array([[29.5]]))
# array([[388.60656932]])
lr.predict([[29.9,]])
# array([[402.47994762]])
```

准备好预测模型后，我们还可以使用 sklearn.metrics 中的方法来计算性能并评估模型本身。r2_score(0.86) 也被解释为准确率得分，其值介于 0 和 1 之间——得分越高，准确度越好。这表明该模型在预测结果方面是可以接受的：

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

> **重要提示**
> **均方误差（MSE）** 值 9910.9392 是残差平方和的均值。RMSE 值 99.5537 是 MSE 的平方根。**平均绝对误差（MAE）** 值 87.8623 是残差绝对值之和的均值。在一般情况下，MSE 总是大于 MAE。

在本节中，我们通过代码示例探讨了 SLR 技术，并识别了 sklearn 中的各种属性。在下一节中，我们将使用 MLR。

## 多元线性回归

当多个自变量与一个因变量之间存在相关性时，使用 MLR。在本例中，我们希望检查在“简单线性回归”一节中用于 SLR 的数据集中，Perch 物种的 Width 和 Height 列的值与 Weight 的相关性。

现在，我们将通过代码讨论一些 MLR 概念，以下是代码文件的地址：https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter11/Chapter11_multi_regression.ipynb。

inDepd 包含从 Perch 收集的自变量，而 depd 是因变量 Weight：

```
inDepd = perch[['Width', 'Height']] # independent variables
depd = perch['Weight']                 # dependent variable
```

同样，80% 的数据用于训练，20% 用于测试。使用 sklearn.model_selection 模型的 train_test_split() 方法创建训练集和测试集：

```
X_train, X_test, y_train, y_test = train_test_split(inDepd, depd,
train_size=0.8, shuffle=True)
```

使用训练数据集创建并拟合（或训练）mlr 模型：

```
mlr = LinearRegression().fit(X_train, Y_train)
mlr.score(X_train, y_train)
0.9560246146854837
```

X_train 包含多个用于训练模型的数据，如图 11.5 所示：

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

图 11.5：包含多个自变量的 X_train

训练完成后，使用 mlr 模型来预测来自 X_test 的数据。还需注意，r2_score 为 0.81799，这在 MLR 情况下是可以接受的：

```
weight_pred_mlr = mlr.predict(X_test)
weight_pred_mlr
array([643.07827704, 96.06295718, 228.07992289, 76.27638592, 136.430909,
143.8607218, 163.98405844, -300.15848523,
446.23998044,  291.21291802,  944.3976018,  243.67945607])

r2_score(y_test, weight_pred_mlr)
0.817999125036539
```

也可以直接将 Width 和 Height 的输入提供给 mlr.predict()。例如，请参见以下代码块：

```
mlr.predict(np.array([[3.45, 6.05]]))
array([154.53150904])

mlr.predict(np.array([[7.10, 11.22]]))
array([807.225824])
```

**重要提示**
如本节代码实现所示，scikit-learn 拥有大量用于各种 ML 相关概念的工具。如果用户希望从其 ML 模型中获得大量统计和定量值，那么 statsmodel 和 scipy 更适用，因为代码更简短。

在下一节中，我们将使用一种与 NLP 相关的技术。

## 情感分析

NLP 是 ML 的子集，而情感分析是 NLP 的子集。通过情感分析，我们可以检测文本的情感是正面、中性还是负面。情感分析可用于文本、评论、反馈等内容。

这种分析有助于评估客户或顾客的评论和反馈，以及它们是正面还是负面。检测到的情感有助于更好地开展营销，公司、电子商务网站、银行、旅行社以及更多组织可以监控和管理其战略、营销政策、产品品牌、客户需求等。

在接下来的部分中，我们将使用代码和示例，这些内容可在 https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter11/Chapter11_textblob_vader.ipynb 获取。

nltk Python 库是 NLP 的先驱之一。NLP 有很多用途，例如分类。在目前的情况下，我们将使用 textblob 和 vaderSentiment 库：

- textblob：这是一个基于 nltk 构建的文本处理库。它可用于各种 NLP 方面，例如词性标注、情感分析、词形还原、分词和文本分类。为了分析文本的情感，textblob 计算并返回极性和主观性作为情感结果：
  - polarity：表示文本的正面或负面情感。取值范围从 -1（非常负面）到 1（非常正面），0 表示中性。
  - subjectivity：表示个人感受。取值范围从 0（客观）到 1（主观）。

- vaderSentiment：这是一个社交媒体情感分析器。与 TextBlob 类似，Valence Aware Dictionary and Sentiment Reasoner（VADER）是一个基于规则的情感分析器。VADER 已使用大量社交媒体文本进行训练。vaderSentiment 返回以下四个元素：
  - compound：这是词典中词语的效价得分。取值范围从 -1（极度负面）到 1（非常正面）。
  - pos：compound 得分 >=0.05 的正面值。
  - neg：compound 得分 <=-0.05 的负面值。
  - neu：compound 得分 >=-0.05 且 <0.05 的中性情感。

对 quote_details.csv 数据执行情感分析，这些数据是在第 8 章中从 http://quotes.toscrape.com/ URL 抓取的。在当前案例中，只需要名为 quote 的列。它是唯一将与 textblob 和 vaderSentiment 一起用于分析情感的字符串或文本元素。只能通过使用 pd.read_csv() 中的 usecols 参数，才能从文件中读取单个列或选定的列组：

```
import pandas as pd
quotes = pd.read_csv("quote_details.csv",usecols=["quote"])
quotes['quote'][0]
'The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.'
```

将内容作为 quotes DataFrame 读取后，在下面的示例中，我们将使用两个非常流行的 Python 库来分析内容的情绪。

如图 11.6 所示，quotes 现在拥有足够的情感信息，可以对其进行分析：

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

图 11.6：包含极性和主观性的 DataFrame quotes

使用 textblob 从所选数据集中获得情感相关细节。虽然我们在这样的示例中使用了现成的库，但结果可能不如使用该数据集所能获得的结果更具洞察力。

因此，在下一节中，将对同一数据集（quotes）使用 vaderSentiment，这将有助于生成更多信息，并有助于与 textblob 获得的结果进行比较。

### 示例 2 – 使用 vaderSentiment

vaderSentiment 的 SentimentIntensityAnalyzer 类包含 polarity_score() 方法，该方法接受一个字符串并返回四个键：pos、neg、neu 和 compound。每个返回键的值都会被收集并添加到原始 DataFrame quotes 中：

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

