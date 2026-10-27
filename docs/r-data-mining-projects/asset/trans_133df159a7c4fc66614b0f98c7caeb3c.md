

让我们对`Cars93`数据集中的`Price`变量进行正态性检验：

```
> library(nortest)
> ad.test(Cars93$Price) # 安德森-达林检验
Anderson-Darling normality test
data: Cars93$Price
A = 3, p-value = 9e-07
> cvm.test(Cars93$Price) # 克拉默-冯·米塞斯检验
Cramer-von Mises normality test
data: Cars93$Price
W = 0.5, p-value = 6e-06
> lillie.test(Cars93$Price) # 利利福斯（KS）检验
Lilliefors (Kolmogorov-Smirnov) normality test
data: Cars93$Price
D = 0.2, p-value = 1e-05
> pearson.test(Cars93$Price) # 皮尔逊卡方检验
Pearson chi-square normality test
data: Cars93$Price
P = 30, p-value = 3e-04
> sf.test(Cars93$Price) # 夏皮罗-弗朗西亚检验
Shapiro-Francia normality test
data: Cars93$Price
```

从上述检验可以看出，`Price`变量不服从正态分布，因为所有统计检验的p值均小于0.05。如果我们在双变量关系中增加更多维度，就变成了多元分析。让我们尝试理解`Cars93`数据集中汽车马力与长度之间的关系：

```
> library(corrplot)
> o<-cor(Cars93[,c("Horsepower","Length")])
> corrplot(o,method = "circle",main="相关性图")
```

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_65_0.png)

当我们纳入更多变量时，就变成了多元关系。让我们尝试绘制`Cars93`数据集中多个变量之间的多元关系图。

```
```r
> library(corrplot)
> t<-
  cor(Cars93[,c("Price","MPG.city","RPM","Rev.per.mile","Width","Weight","Horsepower","Length")])
> corrplot(t,method = "ellipse")
```
```

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_65_1.png)

有多种方法可以作为参数传递给相关性图，分别是"circle"（圆形）、"square"（方形）、"ellipse"（椭圆）、"number"（数字）、"shade"（阴影）、"color"（颜色）和"pie"（饼图）。

## 假设检验

零假设表明什么都没发生，均值保持不变等等。而备择假设则表明发生了某些不同的事情，总体的均值存在差异。执行假设检验需要以下几个步骤：

- 1. **提出零假设**：对总体做出一个假设陈述；例如，城市内汽车的平均油耗为40。
- 2. **提出备择假设**：如果零假设被证明是错误的，那么还存在什么其他可能性？例如，如果城市内油耗不是40，那么是大于40还是小于40？如果它不等于40，则属于非方向性备择假设。
- 3. **计算样本检验统计量**：检验统计量可以是t检验、f检验、z检验等等。根据数据可得性和先前声明的假设选择合适的检验统计量。
- 4. **确定置信区间**：根据特定业务问题所需的精确程度，有三个不同的置信水平：90%、95%和99%。由研究人员/分析师自行决定选择哪个置信区间水平。
- 5. **定义阿尔法值**：如果选择的置信水平是95%，则阿尔法值为5%。因此，确定阿尔法值有助于计算检验的p值。
- 6. **决策**：如果所选的p值小于阿尔法水平，则有证据表明可以拒绝零假设；如果不是，则我们接受零假设。

## 总体均值检验

运用假设检验流程，让我们以`Cars93`数据集为例来检验总体均值。

## 已知方差下均值的单尾检验

假设研究人员声称样本中收集的所有汽车的平均油耗超过35。在93辆汽车的样本中，观察到所有汽车的平均油耗为29。你应该接受还是拒绝研究人员的说法？

这就是我们在已知方差的情况下，对样本数据进行的总体均值单尾和双尾检验分析方式。

## 比例的单尾和双尾检验

使用`Cars93`数据集，假设美国制造的汽车中有40%的RPM超过5000。从样本数据中，我们发现57辆汽车中有17辆的RPM高于5000。你从这个情境中能得出什么解读？

```
> mileage<-subset(Cars93,Cars93$RPM > 5000)
> table(mileage$Origin)
美国 非美国
17 40
> p1<-17/57
> p0<- 0.4
> n <- length(mileage)
> z <- (p1-p0)/sqrt(p0*(1-p0)/n)
> z
[1] -1.079
> #计算5%阿尔法水平下的临界值
> alpha = .05
> z1 = qnorm(1-alpha)
> z1
[1] 1.645
> ifelse(z > z1,"拒绝零假设","接受零假设")
[1] "接受零假设"
```

如果备择假设不是方向性的，则属于比例的双尾检验情形；除了临界值的计算方式不同之外，前面的计算没有任何变化。详细的脚本如下：

```
> mileage<-subset(Cars93,Cars93$RPM > 5000)
> table(mileage$Origin)
美国 非美国
17 40
> p1<-17/57
> p0<- 0.4
> n <- length(mileage)
> z <- (p1-p0)/sqrt(p0*(1-p0)/n)
> z
[1] -1.079
> #计算5%阿尔法水平下的临界值
> alpha = .05
> z1 = qnorm(1-alpha/2)
> c(-z1,z1)
[1] -1.96 1.96
```

```
> ifelse(z > z1 | z < -z1,"拒绝零假设","接受零假设")
[1] "接受零假设"
```

- - **连续数据的双样本配对检验：**双样本配对检验中所检验的零假设是，某个处理对受试者没有影响、该治疗方法对受试者没有效果等等。备择假设则是，某个处理、治疗方法或药物对受试者具有统计学上显著的影响。

虽然`Cars93`数据集中没有这样的变量，但我们仍然可以假设不同品牌汽车的最低价格与最高价格之间存在配对关系。

- - **双样本t检验的零假设：**平均价格没有差异。
- - **备择假设：**平均价格存在差异：

```
> t.test(Cars93$Min.Price, Cars93$Max.Price, paired = T)
配对t检验
data: Cars93$Min.Price and Cars93$Max.Price
t = -9.6, df = 92, p-value = 2e-15
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
-5.765 -3.781
sample estimates:
mean of the differences
-4.773
```

p值小于0.05。因此，可以得出结论，平均最低价格与平均最高价格之间的差异在95%置信水平下具有统计学显著性。

- - **连续数据的双样本非配对检验：**从`Cars93`数据集来看，高速公路上的油耗与城市内的油耗被假定为不同的。这种差异是否具有统计学显著性，可以通过使用独立样本t检验来比较均值进行检验。
- - **零假设：**高速公路上的MPG与城市内的MPG没有差异。
- - **备择假设：**高速公路上的MPG与城市内的MPG存在差异：

```
韦尔奇双样本t检验
data: Cars93$MPG.city and Cars93$MPG.highway
```

```
t = -8.4, df = 180, p-value = 1e-14
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
-8.305 -5.136
sample estimates:
mean of x mean of y
22.37 29.09
```

从双样本t检验可以看出，当两个样本独立时，p值小于0.05；因此，我们可以拒绝“高速公路与城市内的平均油耗没有差异”的零假设。城市内与高速公路上的平均油耗之间存在统计学上显著的差异。这可以用稍微不同的方式来表述，比如设定一个零假设：手动挡与自动挡汽车在城市内的平均油耗差异是不同的：

```
, data=Cars93)
韦尔奇双样本t检验
data: Cars93$MPG.city by Cars93$Man.trans.avail
t = -6, df = 84, p-value = 4e-08
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
-6.949 -3.504
sample estimates:
mean in group No mean in group Yes
18.94 24.16
```

因此，上述检验的结论是：自动挡与手动挡车型之间的平均油耗存在统计学上显著的差异；这是因为p值小于0.05。

在应用t检验之前，检查数据的正态性是很重要的；可以使用Shapiro检验函数来评估变量的正态性：

```
> shapiro.test(Cars93$MPG.city)
Shapiro-Wilk正态性检验
data: Cars93$MPG.city
W = 0.86, p-value = 6e-08
)
> qqline(Cars93$MPG.city)
```

