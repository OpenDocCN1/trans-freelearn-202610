

```python
import pickle
import os

# Connect to TabPy
client = Client('http://localhost:9004/')

def sentiment_analyzer(review_texts):
    """
    Analyzes sentiment of customer reviews
    Returns: polarity score (-1 to 1) and subjectivity score (0 to 1)
    """
    import pandas as pd
    from textblob import TextBlob

    results = []
    for text in review_texts:
        if pd.isna(text) or text == "":
            results.append([0.0, 0.0]) # Neutral for empty reviews
        else:
            try:
                blob = TextBlob(str(text))
                polarity = blob.sentiment.polarity
                subjectivity = blob.sentiment.subjectivity
                results.append([polarity, subjectivity])
            except:
                # Fallback if TextBlob fails
                results.append([0.0, 0.0])

    return results

# Deploy function to TabPy
client.deploy('sentiment_analyzer',
              sentiment_analyzer,
              'Returns sentiment polarity and subjectivity for customer reviews',
              override=True)

print("Models deployed successfully!")
print("Check endpoints at: http://localhost:9004/endpoints")
```

```
[marleenmeier@Mac Chapter 14 % Python3 Exerc1.py
Models deployed successfully!
Check endpoints at: http://localhost:9004/endpoints
marleenmeier@Mac Chapter 14 % 
```

图 14.54: 部署 TabPy 模型

- 2. 打开浏览器并导航到 http://localhost:9004/endpoints。您应该能看到已列出 sentiment_analyzer 和 keyword_extractor。

```
{"sentiment analyzer": {"description": "Returns sentiment polarity and subjectivity for customer reviews", "type": "model", "version": 1, "dependencies": [ ], "target": null, "creation_time": 1772977538, "last_modified_time": 1772977538, "schema": null, "is_public": false, "docstring": "\nAnalyzes sentiment of customer reviews \nReturns polarity score (-1 to 1) and subjectivity score (0 to 1) \n", "keyword_extractor": {"description": "Extracts top keywords from review text", "type": "model", "version": 1, "dependencies": [ ], "target": null, "creation_time": 1772977539, "last_modified_time": 1772977539, "schema": null, "is_public": false, "docstring": "\nExtracts most frequent keywords from reviews \n"}}}
```

图 14.55: 端点

- 3. 现在让我们逐步构建 Tableau 仪表板。打开 Tableau Desktop

- 4. 连接到您的客户评论数据集（Excel、CSV 或文本文件）

- 5. 我们将创建两个调用已部署情感分析器的计算字段

- 1. 第一个是情感极性：

```
SCRIPT_REAL("
return tabpy.query('sentiment_analyzer', _arg1)['response'][0]
",
ATTR([Review Text])
)
```

此函数调用我们部署的 sentiment_analyzer 函数，传递评论文本，并返回响应数组的第一个元素（极性分数）。

- 2. 第二个是情感主观性：

```
SCRIPT_REAL("
return tabpy.query('sentiment_analyzer', _arg1)['response'][1]
",
ATTR([Review Text])
)
```

这将返回第二个元素（主观性分数）。

- 6. 现在创建一个名为情感类别的计算字段，以对情感分数进行分类：

```
IF [Sentiment Polarity] > 0.1 THEN "Positive"
ELSEIF [Sentiment Polarity] < -0.1 THEN "Negative"
ELSE "Neutral"
END
```

- 7. 现在让我们创建一个内联 Python 计算（如您章节中的情感分析示例），以计算评论长度和复杂度。我们将此计算字段命名为评论文本统计：

```
SCRIPT_INT("
import pandas as pd
import numpy as np

reviews = _arg1
ratings = _arg2

results = []
for i in range(len(reviews)):
    if pd.isna(reviews[i]):
        results.append([0, 0])
    else:
        text = str(reviews[i])
        word_count = len(text.split())
        char_count = len(text)

        # Calculate complexity score (words per sentence approx)
        sentences = text.count('.') + text.count('!') + text.count('?')
        if sentences == 0:
            sentences = 1
        words_per_sentence = word_count / sentences

        # Combine metrics into a single complexity score (0-100)
        complexity = min(100, (words_per_sentence * 5) + (char_count / 20))

        results.append([word_count, int(complexity)])

# Return as list of lists - Tableau will interpret each element separately
return [item[0] for item in results] # Word count
",
ATTR([Review Text]),
ATTR([Rating])
)
```

此脚本演示了内联 Python；整个脚本都位于 Tableau 计算字段内。

- 8. 现在让我们通过创建单个工作表，将所有这些计算字段组装成专业仪表板

## 工作表 1: 随时间变化的情感

- 1. 将 [Date] 拖到 列（作为连续的 DAY(Date)）

- 2. 将 [Sentiment Polarity] 拖到 行

- 3. 将 [Product Category] 拖到 颜色

- 4. 添加趋势线：右键单击 | 趋势线 | 显示趋势线

- 5. 另存为 "Sentiment Trends"

![](img/66cf6b89349fda3581a4f196cb3c98a4_680_0.png)

图 14.56: 情感趋势

## 工作表 2: 情感分布

- 1. 将 [Sentiment Category] 拖到 列

- 2. 创建一个计算字段 Number of Records =1 并将其拖到 行

- 3. 将 [Product Category] 拖到 颜色

- 4. 将 Review ID 拖到 详细信息

- 5. 将标记类型更改为 条形图

- 6. 另存为 "Sentiment Breakdown"

![](img/66cf6b89349fda3581a4f196cb3c98a4_681_0.png)

图 14.57: 情感分解

## 工作表 3: 评分与情感散点图

- 1. 将 [Rating] 拖到 列

- 2. 将 [Sentiment Polarity] 拖到 行

- 3. 将 [Product Category] 拖到 颜色

- 4. 禁用 分析 | 聚合值

- 5. 另存为 "Rating vs. Sentiment"

![](img/66cf6b89349fda3581a4f196cb3c98a4_682_0.png)

图 14.58: 评分与情感

## 工作表 4: 评论文本长度分析

- 1. 将 [Product Name] 拖到 行

- 2. 将 [Review Text Stats] 拖到 列（在 X 轴上重命名为 "Word Count"）

- 3. 将 [Sentiment Category] 拖到 颜色

- 4. 禁用 分析 | 聚合度量

- 5. 按字数降序排序

- 6. 另存为 "Review Length Analysis"

![](img/66cf6b89349fda3581a4f196cb3c98a4_683_0.png)

图 14.59: 评论长度分析

是时候组装仪表板了。

- 9. 打开新的 仪表板 选项卡

- 10. 按网格模式添加工作表：

  - 顶部行：情感趋势（全宽）
  - 中间行：情感分解和评分与情感（并排）
  - 底部行：评论长度分析

- 11. 将产品类别颜色标签添加到顶部

- 12. 更改字体、颜色和间距，以符合您的喜好：

## 客户评论 - 情感

![](img/66cf6b89349fda3581a4f196cb3c98a4_684_0.png)

## 情感分解

![](img/66cf6b89349fda3581a4f196cb3c98a4_684_1.png)

## 评分与情感

![](img/66cf6b89349fda3581a4f196cb3c98a4_684_2.png)

图 14.60: 客户评论 – 情感

## 常见问题及修复：

- 假设您的脚本返回错误。可能是 TabPy 服务器未运行。检查终端并重新启动 TabPy。

- 如果性能缓慢，可能是因为数据量太大。尝试添加数据源筛选器，并进一步聚合。

- 如果情感分数全部为 0，文本字段可能为空或 null。添加数据质量筛选器。

## 如果仪表板感觉缓慢，尝试以下优化：

- 1. **添加数据源筛选器**：仅限制最近 12 个月的评论
- 2. **使用提取而非实时连接**：在 Python 处理后创建 Tableau 提取
- 3. **简化脚本**：将复杂逻辑移至部署的函数中（比内联更快）
- 4. **缓存结果**：对于静态分析，仅运行一次 Python 并保存结果

这正是高级分析应用程序的典型示例，它让 Tableau 专家脱颖而出，将 Python 的分析深度与 Tableau 的可视化能力相结合，创造出单靠任一平台都无法实现的工具。

## 摘要

本章仅涉及使用 R 和 Python 的选项的表面内容。完成本章后，您现在应该能够从 Tableau 连接到 Python 和 R，并在 Tableau 中识别和编写两种编程语言的基本语法。最重要的是，您现在足够熟练，可以在 Tableau 中利用 R 和 Python 函数的强大功能，从简单的平均值计算到回归，再到实现自己的机器学习模型。虽然我们介绍了安装、集成和工作流，以及一些更流行的函数和用例，但还有更多可以探索。事实上，Tableau 编程集成的可能性在 BI 社区中仍在很大程度上是未被探索的领域。

数据可视化领域的先锋正在不断突破界限，但仍有许多工作要做。对于希望提升职业选择的读者来说，精通这两个包可能会带来巨大优势。更多内容即将推出！

在下一章中，我们将从编写代码转向探索同样令人兴奋的内容——Tableau 如何重新定义人工智能所能实现的目标。您刚刚看到 Python 和 R 如何扩展 Tableau 的分析能力；现在，请想象一种体验，其中洞察会主动来找您，AI 代理可以对话式地回答问题，而预测模型只需点击几下即可。这就是 Tableau 智能时代的承诺。

