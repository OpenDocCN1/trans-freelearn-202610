
> 最大术语长度：9
> 加权：术语频率 (tf)
>
> 术语
> 文档 包装 支架 悬挂牌 午餐 金属 蛋糕 圣诞节 设计
> 1 0 0 0 0 0 1 0 0 0 0
> 2 0 1 0 0 0 0 0 0 0 0
> 3 0 0 0 0 0 0 0 0 0 0
> 4 0 0 0 0 0 0 0 0 0 0
> 5 0 0 0 0 0 0 0 0 0 0
> 6 0 0 0 0 0 0 0 0 0 0
> 7 0 0 0 0 0 0 0 0 0 0
> 8 0 0 0 0 0 0 0 0 0 0
> 9 0 0 0 0 0 0 0 0 0 0
> 10 0 0 0 0 0 0 0 0 0 0
> 术语
> 文档 回顾 心形
> 1 0 0
> 2 0 0
> 3 0 0
> 4 0 1
> 5 0 0
> 6 0 0
> 7 0 0
> 8 0 0
> 9 0 0
> 10 0 0

## k-means 术语聚类

现在我们可以使用 k-means 对术语-文档矩阵进行聚类。为说明目的，我们将指定生成五个簇：

```
kmeans5 <- kmeans(dtms, 5)
```

k-means 完成后，我们将簇编号附加到原始数据，然后根据簇创建五个子集：

```
kw_with_cluster <- as.data.frame(cbind(OnlineRetail, Cluster = km
# subset the five clusters
cluster1 <- subset(kw_with_cluster, subset = Cluster == 1)
cluster2 <- subset(kw_with_cluster, subset = Cluster == 2)
cluster3 <- subset(kw_with_cluster, subset = Cluster == 3)
cluster4 <- subset(kw_with_cluster, subset = Cluster == 4)
cluster5 <- subset(kw_with_cluster, subset = Cluster == 5)
```

## 检查簇 1

打印数据样本：

```
> head(cluster1[10:13])
Desc2 lastword firstword Cluster
50 VintageBillboardLove/hateMug MUG VINTAGE 1
86 BagVintagePaisley PAISLEY BAG 1
113 ShopperVintagePaisley PAISLEY SHOPPER 1
145 ShopperVintagePaisley PAISLEY SHOPPER 1
200 VintageHeadsAndTailsCardGame GAME VINTAGE 1
210 PaperChainKitVintageChristmas CHRISTMAS PAPER 1
```

列出频率并打印簇中最流行的术语。观察到许多这些项目与圣诞节和花边物品有关，似乎经常一起出现：

```
tail(sort(table(cluster1$lastword)), 10) MUG GAME BUNTING CARDS D
427 431 456 482 535 717 911
```

## 检查簇 2

对簇 2 进行汇总后，查看文件的一部分。综合来看首词和末词，这个簇似乎与悬挂式支架有关：

```
> head(cluster2[10:13]) Desc2 lastword firstword Cluster 6 GlassS
```

与其只看记录，不如看最流行单词的频率。心形、悬挂和文件夹是簇 2 中出现频率最高的三个词：

```
tail(sort(table(cluster2$lastword)), 10)
```

| 古董 | 灯笼 | 支架 | 装饰品 | 玻璃 | T- | 灯光 | 悬挂 |
|---|---|---|---|---|---|---|---|
| 167 | 181 | 226 | 260 | 319 | 361 | 531 | 639 |

## 检查簇 3

簇 3 可能与客户购买不同标志的套装有关。套装、标志、盒子和设计是四个最常见的单词：

```
head(cluster3[10:13]) Desc2 lastword firstword Cluster 5 MetalLan
> tail(sort(table(cluster3$lastword)),10)
```

| 蜡烛 | 波点 | 三角旗 | 袋子 | 装饰品 | 套装 | 标志 | 盒子 | 设计 | |
|---|---|---|---|---|---|---|---|---|---|
| 2307 | 2539 | 2610 | 3084 | 3156 | 3844 | 6184 | 6277 | 10975 | 16627 |

## 检查簇 4

簇 4 可以看作购买复古斑点产品的客户：

```
> head(cluster4[10:13]) Desc2 lastword firstword Cluster 100 Lunc
tail(sort(table(cluster4$lastword)), 10) APRON NAPKINS BANK PC TI
486 511 512 514 531 664 1203
BAG CASES RETROSPOT
1395 4318 6485
```

## 检查簇 5

最后，簇 5 似乎与瓶子的购买有关，可能与香水、精华液和滋补品有关：

```
head(cluster5)
```

```
> head(cluster5[10:13])
 Desc2 lastword firstword Cluster
59 KnittedUnionFlagHotWaterBottle BOTTLE KNITTED 5
68 KnittedUnionFlagHotWaterBottle BOTTLE KNITTED 5
181 AssortedBottleTopMagnets MAGNETS ASSORTED 5
206 EnglishRoseHotWaterBottle BOTTLE ENGLISH 5
229 RetrospotHeartHotWaterBottle BOTTLE RETROSPOT 5
261 HotWaterBottleTeaAndSympathy SYMPATHY HOT 5
```

```
tail(sort(table(cluster5$lastword)), 10)
```

| 巴布什卡 | 磁铁 | 通感剂 | 灵丹妙药 | 香水 | 开瓶器 | 同情 |
|---|---|---|---|---|---|---|
| 87 | 95 | 108 | 112 | 132 | 149 | 318 |

## 预测簇分配

本练习的目标是对测试数据集打分，通过基于训练数据集的 predict 方法分配簇。

## 使用 flexclust 预测簇分配

标准 kmeans 函数没有预测方法。然而，我们可以使用 flexclust 包，它具有此方法。由于预测方法运行可能耗时较长，我们将仅对样本行和列进行说明。为了比较测试和训练结果，它们还需要具有相同数量的列。为说明目的，我们将数量设置为 10。

首先，从 OnlineRetail 训练数据中取样：

```
set.seed(1)
sample.size <- 10000
max.cols <- 10
library("flexclust") OnlineRetail <- OnlineRetail[1:sample.size,
```

接下来，从采样数据集的 description 列创建文档-术语矩阵。我们将使用 RTextTools 包的 create_matrix 函数，它可以在不单独创建语料库的步骤下先创建 TDM：

```
require(tm)
library(RTextTools) #Create the DTM for the training data
```

```
dtMatrix <- create_matrix(OnlineRetail$Description, minDocFreq =
minWordLength = 4, removeStopwords = TRUE, removePunctuation = T
weighting = weightTf)
```

接下来，检查数据的维度。可以看到有 1300 个术语。几乎所有这些都是稀疏术语，因此我们将从矩阵中删除它们：

```
dim(dtMatrix)
> [1] 10000 1300 dtMatrix <- removeSparseTerms(dtMatrix, 0.99)
```

删除稀疏术语将列数从 1300 减少到 62！

```
dim(dtMatrix) > [1] 10000 62
```

我们将只在测试和训练中保留相同数量的术语（max.cols）：

```
dtMatrix <- dtMatrix[, 1:max.cols]
```

为了查看 dtMatrix 中前 10 个术语至少出现两次的行，我们可以使用 rowSums 函数将每个术语的 1 和 0 指示符相加：

```
tmp <- rowSums(as.matrix(dtMatrix)) tmp3 <- data.frame(tmp[tmp>1]
```

然后我们将提取 >1 条件的行索引：

```
selected <- as.numeric(rownames(tmp3)) head(selected) > head(sele
```

打印满足此条件的前 10 个术语的 DTM 的前 10 行：

```
kable(tm::inspect(dtMatrix[selected[1:10], ]))
```

| | 报警 | 古董 | 各式各样 | 巴布什卡 | 烘焙类 | 鸟 | 瓶子 | 盒子 | 蛋糕 | 蜡烛 |
|---|---|---|---|---|---|---|---|---|---|---|
| 3 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 0 | 0 |
| 38 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 0 | 0 |
| 68 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 83 | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |
| 92 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 111 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 113 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 115 | 0 | 0 | 1 | 0 | 0 | 1 | 0 | 0 | 0 | 0 |
| 121 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 0 | 0 |
| 155 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 | 0 |

## 运行 k-means 生成簇

## 使用 kcca 包运行 kmeans 函数生成五个簇分配：

```
# repeat kmeans using the kcca function. Clusters=5

clust1 = kcca(dtMatrix, k = 5, kccaFamily("kmeans"))
clust1 > kcca object of family 'kmeans'
>
> call:
> kcca(x = dtMatrix, k = 5, family = kccaFamily("kmeans"))
> cluster sizes:
>
> 1 2 3 4 5
> 360 120 152 387 8981
```

## 打印每个簇的分类数量：

```
table(clust1@cluster) >
> 1 2 3 4 5
> 360 120 152 387 8981
```

| | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| | 360 | 120 | 152 | 387 | 8981 |

## 将簇与训练数据合并，并显示一些分配给每个记录的簇的示例记录：

```
kw_with_cluster2 <- as.data.frame(cbind(OnlineRetail, Cluster =
head(kw_with_cluster2) > InvoiceNo StockCode Description Quantit
> 5 536365 71053 METAL LANTERN 6
> 6 536365 21730 GLASS STAR FROSTED T-LIGHT HOLDER 6
> 2 536365 22752 SET 7 BABUSHKA NESTING BOXES 2
> 4 536365 84029E WOOLLY HOTTIE HEART. 6
> 1 536365 84406B CREAM CUPID HEARTS COAT HANGER 8
> 8 536366 22632 HAND WARMER POLKA DOT 6
> InvoiceDate UnitPrice CustomerID Country itemcount
> 5 12/1/2010 8:26 3.39 17850 United Kingdom 7
> 6 12/1/2010 8:26 4.25 17850 United Kingdom 7
> 2 12/1/2010 8:26 7.65 17850 United Kingdom 7
> 4 12/1/2010 8:26 3.39 17850 United Kingdom 7
> 1 12/1/2010 8:26 2.75 17850 United Kingdom 7
> 8 12/1/2010 8:28 1.85 17850 United Kingdom 2
> Desc2 lastword firstword Cluster
> 5 MetalLantern LANTERN METAL 5
> 6 GlassStarFrostedT-lightHolder HOLDER GLASS 5
> 2 Set7BabushkaNestingBoxes BOXES SET 5
> 4 WoollyHottieHeart. HEART. WOOLLY 5
```
