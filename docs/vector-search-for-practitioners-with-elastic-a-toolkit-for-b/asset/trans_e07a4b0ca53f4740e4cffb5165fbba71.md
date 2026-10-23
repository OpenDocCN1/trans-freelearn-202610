

这意味着向量能够捕捉语义含义和上下文，最终服务于终端用户体验。

如果你感兴趣的话，在电商客户搜索“wireless headphones”的情况下，该组织会将用户查询表示为高维空间中的向量，并使用预训练模型如 *word2vec* 将查询中的每个词转换为向量表示。合并后的结果代表整个查询。这使我们能够将该向量与由产品表示构建的向量空间进行比较。以下是一个 10 维向量，给出了表示的示例：

```
[0.213, -0.178, 0.425, -0.067, 0.321, 0.195, -0.267, 0.132, -0.455, -0.033]
```

这里有趣的是，查询突然被转换到可能性空间，这使我们能够比较向量空间，而不是关键词，以确定相似性。产品是否存在？正在搜索的是什么类型的产品？从查询中提取的语义令人着迷。

## Applying ML

AI 搜索体验的另一个层级是使用诸如 **convolutional neural networks (CNNs)**、**recurrent neural networks (RNNs)** 或 **transformers** 等模型对数据进行训练。这是在之前向量化的数据上进行的，以学习和理解数据之间的模式和关系。此阶段的目标是为搜索应用程序做好准备，使其能够支持 NLP。

继续以无线耳机为例，使用诸如 CNNs 或 RNNs 等模型使我们能够浏览该特定文章可能拥有的连接图。在这种情况下，考虑诸如降噪、电池寿命、是否防水、或在做波比跳时是否仍留在佩戴者耳中的特征。

## Integrating personalization

没有个性化，搜索无论是否比关键词搜索更智能，都将保持冰冷。个性化允许你根据过去的搜索和类似搜索将上下文整合到搜索中。这些分别被称为网络画像和用户画像。

CNNs/RNNs/transformers 的美妙之处在于，其优势并不止于我们之前提到的内容——它们还能根据用户的过去搜索为用户提供推荐。

## Getting into production

我们很想告诉你，AI 搜索正在解决搜索应用程序在相关性和延迟性能之间所需的来回往复，从持续优化的角度来看，但事实并非如此。

拥有正确数据的关键是做出正确的决策并限制优化工作，这意味着你不仅需要能够完全监控用户体验，还需要“有趣的事件”来让你了解搜索质量。

为此你需要一定程度的 A/B 测试，并比较给定状态与搜索引擎的新状态的性能，例如在调整模型之后。然而，你也需要某种反馈循环来了解结果的相关性。

一旦你使用我之前提到的高级步骤构建了 AI 搜索系统，你就可以通过整合诸如 NER、情感分析、文本分类、Q/A 和文本摘要等技术来进一步增强系统。我们将在下一部分了解更多关于这些技术的信息。

## Named Entity Recognition (NER)

NER 是 NLP 的一个组件，用于在非结构化文本中检测和分类命名实体。实体可以是具有唯一名称或标识符的名称、品牌、位置或组织。

有了这种技术，流入系统中的非结构化数据将被分类，这通常是上游在标注数据上训练的 ML 模型的成果。在此过程中，数据会被标记为相应的类型。训练完成后，ML 模型使用标签来学习文本中的模式和关系，从而能够准确地识别和分类新文本中的命名实体。

我们可以想到的一种用例是 **personally identifiable information (PII)**，它指的是流入系统而没有任何控制的敏感信息。这些信息可以是姓名、地址、社会安全号码和财务信息。

对于银行来说，无法检测流入其数据中的 PII 可能会对银行及其客户造成严重后果。例如，如果黑客设法访问银行的数据并窃取 PII，他们可以利用它进行欺诈和身份盗窃，这可能导致客户的财务损失以及银行的处罚，因为他们受监管。

这就是为什么金融服务公司使用 NER 来降低 PII 暴露的风险，无论是在陈旧数据上还是在处理过程中对飞行中数据使用。结果是涂黑、加密、屏蔽或设置正确的数据访问权限，从而避免与 PII 暴露相关的法律和声誉风险。

## Sentiment analysis

NLP 的另一种技术是情感分析，其目的是识别和提取非结构化文本中的情感情绪。它通常用于社交网络，例如从推文中提取情绪。组织经常使用它来了解与他们的品牌相关的情感。

然而，虽然情感分析的使用很广泛，但它对于更具体的用例也很有用，例如实时分析产品评论或监控相对于新投资的市场情绪。这有助于金融分析师做出更明智的投资。

今天一家公司可能发生的最糟糕的事情是未能检测到对其产品或服务的负面情绪，这可能导致声誉受损和客户流失。

自然语言 Python 工具包 (https://www.nltk.org/) 对于这个非常流行的任务非常有用。以下代码示例展示了如何实现一个非常简单的情感分析以及结果可能是什么：

```
import nltk
nltk.download('vader_lexicon')

from nltk.sentiment import SentimentIntensityAnalyzer
sia = SentimentIntensityAnalyzer()
text = "I really enjoyed the new movie. The acting was great and the plot was engaging."
scores = sia.polarity_scores(text)
print(scores)
```

你可以在此 notebook 中找到上述代码并运行它：https://github.com/PacktPublishing/Vector-Search-for-Practitioners-with-Elastic/blob/main/chapter1/sentiment-analysis.ipynb。Python 代码使用 SentimentIntensityAnalyzer 执行实际的情感分析。然后返回极性分数，为每个极性（负面、中性和正面）提供分数，以及一个复合分数，从 -1（最负面）到 +1（最正面）。运行 notebook 应该会给你以下结果：

```
{'neg': 0.0, 'neu': 0.522, 'pos': 0.478, 'compound': 0.8777}
```

该分数表明文本中捕捉到的情绪非常积极！

## Text classification

文本分类是另一种 NLP 技术，它为非结构化数据分配预定义类别。影响我们所有人的一个现实生活例子是垃圾邮件管理或将电子邮件过滤和标记为垃圾邮件或非垃圾邮件的操作。

垃圾邮件可能具有许多会使其可疑的信号，例如使用某些关键词、拼写错误、链接等。一旦模型在大量标注数据集上训练完成，电子邮件就会自动被分类为垃圾邮件或非垃圾邮件。这会产生多个结果，例如整体更好的电子邮件体验或提高生产力。

为了简化起见，这里有一个使用在线数据源并显示分类结果及其准确性的示例：

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

在 Google Colab notebook 中执行代码应会给出以下输出：

```
Accuracy: 0.7728359001593202
Predicted classes: [ 7 11 0 ... 9 3 15]
```

让我们理解上述代码中我们实现了什么：

- 正如我们之前提到的，代码使用一个名为 20 Newsgroup 数据集 (http://qwone.com/~jason/20Newsgroups/) 的在线数据集。它是测试此类分类的绝佳游乐场，因为它汇集了 20,000 份新闻组文档，分为约 20 个不同的新闻组。该数据集由卡内基梅隆大学的 Ken Lang 汇编。
- 我们使用 scikit-learn，更为人所知的是 sklearn，一个非常流行且被广泛采用的 Python 开源 ML 库。因为它提供广泛的监督和无监督学习算法，如分类、回归、聚类和降维，我们将在本书中使用它作为工具。
- 我们在此示例中使用词频向量技术，通过 CountVectorizer 函数，本质上将文本数据转换为可用于 ML 的数字格式。
- 我们在向量化训练数据上训练朴素贝叶斯分类器。在此示例中，分类器假设文档中每个词的频率与文档中所有其他词的频率相互独立。该假设是为了对新文档的类别做出预测。
