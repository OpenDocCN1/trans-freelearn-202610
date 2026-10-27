
Horsepower RPM Rev.per.mile Man.trans.avail
Min. : 55.0 Min. :3800 Min. :1320 No :32
1st Qu.:103.0 1st Qu.:4800 1st Qu.:1985 Yes:61
Median :140.0 Median :5200 Median :2340
Mean :143.8 Mean :5281 Mean :2332
3rd Qu.:170.0 3rd Qu.:5750 3rd Qu.:2565
Max. :300.0 Max. :6500 Max. :3755
Fuel.tank.capacity Passengers Length Wheelbase
Min. : 9.20 Min. :2.000 Min. :141.0 Min. : 90.0
1st Qu.:14.50 1st Qu.:4.000 1st Qu.:174.0 1st Qu.: 98.0
Median :16.40 Median :5.000 Median :183.0 Median :103.0
Mean :16.66 Mean :5.086 Mean :183.2 Mean :103.9
3rd Qu.:18.80 3rd Qu.:6.000 3rd Qu.:192.0 3rd Qu.:110.0
Max. :27.00 Max. :8.000 Max. :219.0 Max. :119.0
Width Turn.circle Rear.seat.room Luggage.room
Min. :60.00 Min. :32.00 Min. :19.00 Min. : 6.00
1st Qu.:67.00 1st Qu.:37.00 1st Qu.:26.00 1st Qu.:12.00
Median :69.00 Median :39.00 Median :27.50 Median :14.00
Mean :69.38 Mean :38.96 Mean :27.83 Mean :13.89
3rd Qu.:72.00 3rd Qu.:41.00 3rd Qu.:30.00 3rd Qu.:15.00
Max. :78.00 Max. :45.00 Max. :36.00 Max. :22.00
NA's :2 NA's :11
Weight Origin Make
Min. :1695 USA :48 Acura Integra: 1
1st Qu.:2620 non-USA:45 Acura Legend : 1
Median :3040 Audi 100 : 1
Mean :3073 Audi 90 : 1
3rd Qu.:3525 BMW 535i : 1
Max. :4105 Buick Century: 1
(Other) :87

对于连续变量（如 RPM、马力等），summary 命令显示最小值、第一四分位数、均值、中位数、第三四分位数和最大值。对于分类变量（如汽车类型、安全气囊、手动变速箱可用性等），其单变量统计以频数表的形式表示。频数最高的类别被视为众数类别。

使用 fivenum() 和 describe() 等函数也可以生成类似的汇总统计信息，它们比 summary 函数提供的信息更多：

```
> fivenum(Cars93$Price)
[1] 7.4 12.2 17.7 23.3 61.9
> fivenum(Cars93$MPG.city)
[1] 15 18 21 25 46
> fivenum(Cars93$MPG.highway)
[1] 20 26 28 31 50
```

来自 Hmisc 库的 describe() 函数可用于更好地理解数据描述：

```
> library(Hmisc)
> describe(Cars93)
Cars93
27 Variables 93 Observations
Price
n missing unique Info Mean .05 .10 .25 .50 .75 .90
93 0 81 1 19.51 8.52 9.84 12.20 17.70 23.30 33.62
.95
36.74
lowest : 7.4 8.0 8.3 8.4 8.6, highest: 37.7 38.0 40.1 47.9 61.9
MPG.city
n missing unique Info Mean .05 .10 .25 .50 .75 .90
93 0 21 0.99 22.37 16.6 17.0 18.0 21.0 25.0 29.0
.95
31.4
lowest : 15 16 17 18 19, highest: 32 33 39 42 46
--------------------------------------------------
----------------
MPG.highway
n missing unique Info Mean .05 .10 .25 .50 .75 .90
93 0 22 0.99 29.09 22.0 23.2 26.0 28.0 31.0 36.0
.95
37.4
lowest : 20 21 22 23 24, highest: 38 41 43 46 50
--------------------------------------------------
----------------
AirBags
n missing unique
93 0 3
Driver & Passenger (16, 17%), Driver only (43, 46%)
None (34, 37%)
--------------------------------------------------
----------------
Man.trans.avail
n missing unique
93 0 2
No (32, 34%), Yes (61, 66%)
```

类似地，价格与高速公路里程之间的关系也可以用散点图来表示：

```r
> library(ggplot2)
> library(gridExtra)
> ggplot(Cars93,
 aes(Cars93$Price,Cars93$MPG.highway))+geom_point(aes(colour=(Cars93$Type)))
 +geom_smooth()
```

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_52_0.png)

图 2：价格与高速公路里程之间的关系

数值-分类关系和两个分类变量之间的关系将在第 3 章 *可视化钻石数据集* 中详细说明。

## 多变量分析

多变量关系是考察多个因变量和自变量及其关系的一种统计方法。在本章中，我们将简要讨论超过两个变量之间的多变量关系，但将在后续章节中详细讨论多变量分析。可以通过相关性方法以及交叉列表来了解各变量之间的多变量关系：

```
> pairs(n_cars93,main="Correlation Plot", col="blue")
```

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_53_0.png)

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_55_0.png)

从前面的图像中，我们可以得出结论：价格变量呈正偏态，因为分布右侧存在一些异常值。价格变量的均值因极端波动而被夸大，且大于众数。

现在让我们尝试理解一个可以使用正态分布来回答某个假设的例子。

假设高速公路上的每加仑英里数变量服从均值为 29.08、标准差为 5.33 的正态分布。那么一辆新车提供 35 英里/加仑里程的概率是多少？

```
> pnorm(35,mean(Cars93$MPG.highway),sd(Cars93$MPG.highway),lower.tail = F)
[1] 0.1336708
```

因此，一辆新车提供 35 英里/加仑里程所需的概率为 13.36%，由于期望均值高于实际均值，lower.tail 相当于 FALSE。

## 二项概率分布

二项分布被称为离散概率分布。它描述一次实验的结果。每次试验被认为只有两种结果：成功或失败，是或否。例如，在 Cars93 数据集的变量中，手动变速箱是否可用用“是”或“否”表示。

让我们举一个例子来说明二项分布的用途。在特定部件无法正常工作的情况下，汽车有缺陷的概率是 0.1%。你制造了 93 辆汽车。在当前这批 93 辆汽车中至少检测出 1 辆缺陷车的概率是多少：

```
> pbinom(1,93,prob = 0.1)
[1] 0.0006293772
```

因此，在 93 辆汽车的一批中识别出一辆缺陷汽车的概率为 0.0006，考虑到缺陷部件的概率为 0.10，这个概率非常小。

## 泊松概率分布

泊松分布适用于计数数据，在给定数据以及事件相关信息的情况下，你可以使用泊松概率分布预测该范围内任意数字出现的概率。

让我们举个例子。假设平均每分钟有 200 位顾客访问某个电子商务网站门户。那么求一分钟后有 250 位顾客访问同一网站的概率：

```
> ppois(250,200,lower.tail = F)
[1] 0.0002846214
```

因此，所需概率为 0.0002，这非常罕见。除了前面提到的经常使用的常见概率分布之外，还有许多其他分布可以在罕见情况下使用。

```
mean sd
0.5498938 0.3888336
> x$vcov
mean sd
mean 0.3023831 0.0000000
sd 0.0000000 0.1511916
> x$loglik
[1] -287.1104
> x$n
[1] 93
```

现在我们将了解如何以图形方式表示变量的正态性：

```
> qqnorm(Cars93$MPG.highway)
> qqline(Cars93$MPG.highway)
```

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_58_0.png)

以圆圈表示的数据点偏离了直线 t。

解释离散数据：其中所有类别如下：

```
> table(Cars93$Type)
Compact Large Midsize Small Sporty Van
16 11 22 21 14 9
```

```
Midsize 5 6 1
Small 0 3 11
Sporty 1 3 2
Van 0 1 3
```

用于检验所有因子独立性的 summary 命令可用于检验原假设：

```
> summary(contTable)
Number of cases in table: 93
Number of factors: 3
Test for independence of all factors:
Chisq = 65, df = 27, p-value = 5e-05
Chi-squared approximation may be incorrect
```

除了前面讨论的图形方法之外，还有一些数值统计检验可用于判断变量是否服从正态分布。有一个名为 norm.test 的库用于执行数据正态性检验，下面列出了该库中一些有助于评估数据正态性的函数：

| ajb.norm.test | 调整后的 Jarque-Bera 正态性检验 |
| frosini.norm.test | Frosini 正态性检验 |
| geary.norm.test | Geary 正态性检验 |
| hegazy1.norm.test | Hegazy-Green 正态性检验 |
| hegazy2.norm.test | Hegazy-Green 正态性检验 |
| jb.norm.test | Jarque-Bera 正态性检验 |
| kurtosis.norm.test | 峰度正态性检验 |
| skewness.norm.test | 偏度正态性检验 |
| spiegelhalter.norm.test | Spiegelhalter 正态性检验 |
| wb.norm.test | Weisberg-Bingham 正态性检验 |

| ad.test | Anderson-Darling 正态性检验 |
| cvm.test | Cramér-von Mises 正态性检验 |
| lillie.test | Lilliefors（Kolmogorov-Smirnov）正态性检验 |
| pearson.test | Pearson 卡方正态性检验 |
| sf.test | Shapiro-Francia 正态性检验 |

