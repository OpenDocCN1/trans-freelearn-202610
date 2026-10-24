
pandas的`copy()`方法会创建一个重复的DataFrame。这里之所以需要复制，是因为我们要清理某些列的值。`inplace=True`参数相当敏感，因为它会直接修改bookDF这个DataFrame：

```
bookDF = books.copy()  # Duplicating a DataFrame
bookDF["Rating"].replace(["One","Two","Three","Four",
    "Five"],[1,2,3,4,5],inplace=True)

bookDF["Price"] = bookDF["Price"].str.replace("£","")
bookDF["Price"] = bookDF.Price.astype(float)
```

bookDF现在已经干净了，其中`Price`是浮点类型，`Rating`是整数类型。`groupby()`方法按每个`Rating`对`Price`值进行分组或合并，并使用`np.sum`对`Price`值求和：

```
price_groupby=bookDF.groupby("Rating")["Price"].agg(np.sum)
price_groupby.plot.bar()
```

上述代码的结果已绘制成柱状图，如*图10.10*所示：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_259_0.png)

图10.10：各评分的汇总价格

如*图10.10*所示，我们可以根据`Rating`数值（来自客户反馈）得出以下结论：

- `Rating`值为3和1的书籍销量位居前两名
- `Rating`值为5的书籍销量最低

最后，使用to_csv()将清理后的DataFrame（即bookDF）写入一个新的CSV文件。此外，使用book_details_clean.csv文件进行了EDA分析：

```
bookDF.to_csv('book_details_clean.csv', index=False)
```

> **重要说明**
> 同时包含未清理和已清理数据的EDA HTML报告文件已作为报告提供。建议查看这两份报告，了解它们在详细信息方面的差异。

book_details_clean.csv文件的内容经过清理和处理后用于后续代码中。在下一个示例中，我们将进一步探索图表和pandas操作。

## 示例2——语录分析

本示例的代码可在data_analysis_quote.ipynb中找到。

在示例1（书籍分析）中，我们通过读取JSON文件创建了一个DataFrame。此处，我们的数据在CSV文件中，使用read_csv()读取。可以给usecols参数赋值，以选择要放入新DataFrame中的列：

```
quotes = pd.read_csv("quote_details.csv",
 usecols=["author", "quote", "tags", "tag_count"])
```

标签之间用逗号（,）分隔。tag_count统计标签内的单词数。

以下代码在tag_count大于1的条件下生成了一个新的DataFrame（newQuoteDF）。这里只选择了tags和author两列：

```
newQuoteDF = quotes[quotes["tag_count"]>1][['tags', 'author']]
newQuoteDF
```

新的DataFrame（newQuoteDF）以及所选的两列（tags和author）如图10.11所示：

|   | tags | author |
|---|------|--------|
| 0 | change.deep-thoughts.thinking.world | Albert Einstein |
| 1 | abilities.choices | J.K. Rowling |
| 2 | inspirational,life,live,miracle,miracles | Albert Einstein |
| 3 | alliteration,books,classic,humor | Jane Austen |

图10.11：tag_count > 1的newQuoteDF示例

为了进行正确的分析，我们需要从tags中取出每个单词，并将其与作者关联。例如，change、deep-thoughts、thinking、world —— Albert Einstein这一行需要改为['change', 'Albert Einstein']、['deep-thoughts', 'Albert Einstein']、['thinking', 'Albert Einstein']、['world', 'Albert Einstein']。为此，需要按行迭代，以拆分标签并同时创建一个新的DataFrame（tagsDF）。使用value_counts()方法获得的值绘制在tagsDF的Tag列上的图表如图10.12所示：

```python
tmpData = []
for index, row in newQuoteDF.iterrows(): # tags, author
    tagList = row.tags.split(",")
    for tag in tagList:
        tmpData.append([tag, row.author])

tagsDF = pd.DataFrame(tmpData, columns=['Tag', 'Author'])
tagsDF.Tag.value_counts().plot()
```

如图10.12所示，包含life和love文本的标签在计数上似乎较高：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_261_0.png)

如下查询所示，共有11个以字母l开头的标签：

```python
tagsDF.query("Tag.str.startswith('l')")[['Tag']].nunique()    # 11
```

如*图10.13*所示，还有很多标签的长度小于或等于10——几乎占标签总数的80%：

```
tagsDF.query('Tag.str.len() > 10').count()

Tag       32
Author    32
dtype: int64

tagsDF.query('Tag.str.len() <= 10').count()

Tag       164
Author    164
dtype: int64
```

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_262_0.png)

在*图10.14*中，使用`describe()`方法，我们可以看到在总共196行中，有120个唯一标签，并且有38位作者的语录。出现频率最高的标签是`life`，出现了12次。类似地，阿尔伯特·爱因斯坦是出现频率最高的作者，出现了22次：

```text
tagsDF.describe()

          Tag             Author
count     196             196
unique    120             38
top       life            Albert Einstein
freq      12              22
```

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_262_1.png)

*图10.14*中显示的信息也可以通过`value_counts()`获得：

```
authorCount = tagsDF.Author.value_counts(ascending=True)
authorCount.plot.area()
```

tagsDF中的`Author.value_counts()`绘制在*图10.15*中。结果按升序排列，因此图中出现了陡峭的梯度：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_263_0.png)

图10.15：面积图

下面的代码对`authorDF1.Count`（出现频率）执行筛选操作，并以饼图显示：

```python
fig1 = px.pie(authorDF1[authorDF1.Count>5], values='Count',
              names='Author', title='Author with count >5')
fig1.show()
```

*图10.16*传达的信息与*图10.15*类似，但使用的是饼图。在38位作者中，提到了出现次数超过5次的作者，其中**阿尔伯特·爱因斯坦占该图表的16.8%**。

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_263_1.png)

图10.16：作者饼图

在前面的示例中，我们能够找出出现次数最多的文本的数学统计信息。在pandas中处理原始数据并将其转移到新的DataFrame中进行分析也是目标之一。

此外，当你想对字符串数据进行可视化时，wordcloud是一个有用的库。在下面的代码中，我们从Python的wordcloud库导入了WordCloud类：

```
from wordcloud import WordCloud  # accepts string
```

WordCloud类在初始化时设置了少量属性，例如max_words和background_color，并传入tagsDF中Tag列的内容，结果如图10.17所示：

```
wordcloud2 = WordCloud(background_color="white",max_words=500).generate(' '.join(tagsDF.Tag))
px.imshow(wordcloud2)
```

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_264_0.png)

图10.17：使用标签生成的词云

进行EDA、清理数据、将数据存储在文件和DataFrames中、分析数据集、创建可视化以及生成有意义或隐藏的模式和信息，是本部分的主要收获。使用pandas还有更多内容值得探索；请访问https://pandas.pydata.org/了解更多详细信息。

## 总结

使用不同的分析技术生成和收集信息，并将其用于决策，是一个不断发展的领域。商业智能（BI）、AI和ML等领域需要并使用各种数据分析技术。Python编程为数据收集、数据处理、信息抽象和知识发现等过程提供了强大的基础设施。pandas、NumPy、csv、json和plotly等库是整个系统化过程的核心Python库。

对数据挖掘、数据分析、EDA和数据可视化相关概念的实践性介绍是本章的主要目标。

在下一章中，我们将学习机器学习和网络爬虫。

## 延伸阅读

- *数据挖掘：*
  - https://www.ibm.com/topics/data-mining
  - https://www.springboard.com/blog/data-science/data-mining-python-tutorial/
  - https://www.investopedia.com/terms/d/datamining.asp
- *数据分析：*
  - https://monkeylearn.com/data-analysis/
  - https://www.datapine.com/blog/data-analysis-methods-and-techniques/
  - https://www.investopedia.com/terms/d/data-analytics.asp
- *探索性数据分析：*
  - https://ydata-profiling.ydata.ai/docs/master/index.html
  - https://www.digitalocean.com/community/tutorials/exploratory-data-analysis-python
  - https://www.kaggle.com/code/imoore/intro-to-exploratory-data-analysis-eda-in-python
- *可视化：*
  - https://plotly.com/python/
  - https://www.ibm.com/topics/data-visualization
  - https://matplotlib.org/stable/tutorials/introductory/pyplot.html
  - https://gilberttanner.com/blog/introduction-to-data-visualization-inpython/

