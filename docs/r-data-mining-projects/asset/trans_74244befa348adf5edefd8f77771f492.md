
```
+ "Butter, Banana",
+ sep="\n")
> cat(data)
Bread, Butter, Jam
Bread, Butter
Bread
Butter, Banana
> write(data, file = "basket_format")
>
> ## read data
> library(arules)
> tr <- read.transactions("basket_format", format = "basket", sep=",")
> inspect(tr)
items
1 {Bread,Butter,Jam}
2 {Bread,Butter}
3 {Bread}
4 {Banana,Butter}
```

现在让我们看一下如何从数据框创建单一格式的事务数据：

```
> ## create single format
> data <- paste(
+ "trans1 Bread",
+ "trans2 Bread",
+ "trans2 Butter",
+ "trans3 Jam",
+ sep="\n")
> cat(data)
trans1 Bread
trans2 Bread
trans2 Butter
trans3 Jam
> write(data, file = "single_format")
>
> ## read data
> tr <- read.transactions("single_format", format = "single", cols = c(1,2))
> inspect(tr)
items transactionID
1 {Bread} trans1
2 {Bread,Butter} trans2
3 {Jam} trans3
```

前面的代码解释了如何将以表格格式接收的典型事务数据（就像 R 数据框对应的电子表格格式）转换为 arules 输入数据格式所需的事务数据格式。

## 假设/前提条件

实施关联规则以进行购物篮分析基于一些假设。以下是这些假设：

- 假设所有数据都是分类数据。
- 不应存在任何稀疏数据；稀疏百分比应最小。稀疏意味着数据集中有很多单元格没有值（空白单元格）。
- 发现不同产品之间有趣关系所需的事务数量是数据库中涵盖的项目或产品数量的函数。换句话说，如果您包含更多产品而事务数量较少，数据集中的稀疏度就会更高，反之亦然。
- 您希望在分析中包含的项目数量越多，验证规则所需的事务数量就越多。

## 建模技术

有两种算法在查找零售事务数据库中存在的频繁项集方面非常流行：

- **Apriori 算法：**由 Agrawal 和 Srikant（1994）提出。它采用广度优先搜索，并提供事务计数。
- **Eclat 算法：**由 Zaki 等人（1997b）提出。它采用深度优先搜索；提供事务的交集，而非事务计数。

在本章中，我们将使用 Groceries.csv 数据集，该数据集将用于这两种算法。

## 局限性

尽管在处理大型事务数据库中的关系时，关联规则是从业者的首选，但它们存在某些固有的局限性：

- 关联规则挖掘是一种概率方法，因为它计算某产品被购买的条件的概率，即客户已经购买/将一组其他商品加入购物车。它只是估计购买该产品的可能性。规则的确切准确性可能为真，也可能不为真。
- 关联规则是一种条件概率估计方法，使用简单的项目计数作为度量。
- 即使具有较高的支持度、置信度和提升度，关联规则在实践中也可能没有用；这是因为在大多数情况下，琐碎的规则可能听起来很合理。没有机制可以从全局规则集中过滤掉琐碎的规则，除非使用机器学习框架（这超出了本书的范围）。
- 该算法会提供大量规则；然而，其中只有少数是重要的，并且没有自动方法来识别有用规则的数量。

## 实践项目

我们将要使用的 Groceries.csv 数据集包含来自一家杂货店 1 个月的真实销售点（POS）交易数据。该数据集包含 9,835 笔交易，共有 169 个类别。项集被定义为顾客在同一次购物中购买的物品或产品 P_i {i =1....n} 的组合。简而言之，项集基本上就是我们在零售店购物时通常得到的小票。小票编号被视为交易编号，小票中提到的物品被视为购物篮。数据集的一个快照如下所示：

|    |    |    |    |    |    |
|----|----|----|----|----|----|
| 全麦面包 | 汽水 | 果蔬汁 | 罐装啤酒 | 报纸 | 购物袋 |
| 酸奶 | 饮料 | 瓶装水 | 特色棒 |  |  |
| 汉堡肉 | 其他蔬菜 | 面包卷/小圆面包 | 香料 | 瓶装水 | 卫生用品 |
| 根茎类蔬菜 | 其他蔬菜 | 全脂牛奶 | 饮料 | 糖 |  |
| 猪肉 | 浆果 | 其他蔬菜 | 全脂牛奶 | 掼奶油/酸奶油 | 人工甜味剂 |
| 牛肉 | 葡萄 | 清洁剂 |  |  |  |

在上方显示的快照中，列代表物品，行代表交易。让我们探索该数据集以了解其特征：

```
> # Load the libraries
> library(arules)
> library(arulesViz)
> # Load the data set
> data(Groceries) #directly reading from library
> Groceries<-read.transactions("groceries.csv",sep=",") #reading from local computer
```

事务数据框包含三列。第一列代表产品/物品的名称，第二列代表产品的二级分类（共 55 个级别），第三个变量是产品的三级细分（共 10 个类别）。下面的截图可以更清楚地说明这一点：

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_159_0.png)

事务数据的摘要可以让人了解数据库中存在的总交易数、构成数据库的项目数、数据的稀疏度，以及事务数据库中最突出的一些项目的频率：

```
summary(Groceries)
transactions as itemMatrix in sparse format with
9835 rows (elements/itemsets/transactions) and
169 columns (items) and a density of 0.02609146
most frequent items:
whole milk other vegetables rolls/buns soda yogurt
2513 1903 1809 1715 1372
(Other)
34055
```

数据集中有 9,835 笔交易；在 169 个物品中，最频繁的物品及其对应频率已在上面显示。在 9,835 × 169 的矩阵中，只有 0.0261（即 2.61%）的单元格有值；其余为空。这 2.61% 就是数据集的稀疏度。

### 元素（项集/事务）长度分布：

```
sizes
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17
2159 1643 1299 1005 855 645 545 438 350 246 182 117 78 77 55 46 29
18 19 20 21 22 23 24 26 27 28 29 32
14 14 9 11 4 6 1 1 1 1 3 1
Min. 1st Qu. Median Mean 3rd Qu. Max.
1.000 2.000 3.000 4.409 6.000 32.000
includes extended item information - examples:
labels
1 abrasive cleaner
2 artif. sweetener
3 baby cosmetics
```

从前面的项集长度分布可以看出，有一笔交易同时购买了 32 种产品。有 3 笔交易同时购买了 29 个物品。同样，我们可以解释数据集中其他项集的频率。通过项目频率图可以更好地理解这一点。要了解数据集中的前三笔交易，可以使用以下命令：

```
> inspect(Groceries[1:3])
items
1 {citrus fruit,semi-finished bread,margarine,ready soups}
2 {tropical fruit,yogurt,coffee}
3 {whole milk}
```

要了解杂货数据库中包含每个物品的交易比例，可以使用以下命令。第一个物品 frankfurter（法兰克福香肠）出现在 5.89% 的交易中；sausage（香肠）出现在 9.39% 的交易中；以此类推：

```
> cbind(itemFrequency(Groceries[,1:10])*100)
[,1]
frankfurter 5.8973055
sausage 9.3950178
liver loaf 0.5083884
ham 2.6029487
meat 2.5826131
finished products 0.6507372
organic sausage 0.2236909
chicken 4.2907982
turkey 0.8134215
pork 5.7651246
```

在事务数据集中，有两个重要参数决定频繁项集：支持度和置信度。支持度越高，数据集中的规则数量就越少，并且您可能会错过各个变量之间有趣的关系，反之亦然。有时即使具有较高的支持度、置信度和提升度值，也不能保证获得有用的规则。因此，在实践中，用户可以尝试不同水平的支持度和置信度，以获得有意义的规则。即使最小支持度和最小置信度水平足够高，规则也似乎相当琐碎。

