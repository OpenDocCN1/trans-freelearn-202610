
怀孕次数、胰岛素和葡萄糖水平的相关性最高：

```
> subset(df, abs(Freq) > 0.5 & abs(Freq) <1)
           Var1    Var2      Freq
8     age   pregnant 0.5443412
13 insulin   glucose 0.5811862
30    mass   triceps 0.6482139
34 glucose   insulin 0.5811862
44 triceps     mass 0.6482139
```

你也可以用多种不同的方式创建这些变量的散点图。

使用 pairs() 函数，仅使用基础 R 即可实现这一点：

```
pairs(PimaIndians[c(1, 2, 4, 5, 6, 8)], pch=21)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_527_0.png)

## 计算列均值

在接下来的模拟部分，我们需要通过向量提供列均值，因此现在让我们计算它们，并将其显示为水平条形图。我们将首先为糖尿病阳性组计算列均值，然后为糖尿病阴性组计算列均值。最后，我们将计算总体的均值，尽管我们不会在下一部分中使用它们：

```
means.pos = colMeans(PimaIndians[PimaIndians$diabetes =='pos',1:8]
means.neg = colMeans(PimaIndians[PimaIndians$diabetes =='neg',1:8]
means.all = colMeans(PimaIndians[,1:8],na.rm=TRUE)
barplot(means.all[c(1, 2,3,4,5,6,7,8)],cex.axis=.75,cex.names=.70,
```

下方的条形图仅供参考，显示计算出的均值。由于它们的量级各不相同，我们不进行比较。

![](img/672c0cf8ba3c000a6529b117ed96945e_528_0.png)

## 模拟数据

一旦我们计算了所有列的均值和协方差矩阵，我们就可以模拟任何我们想要的观测数量的大型数据集了。

## 使用哪些相关性？

对于协方差矩阵，我们可以针对两种糖尿病结果（1,0）使用单独的矩阵，也可以使用汇总协方差矩阵，后者显示变量之间的相关性，与结果无关。

我们将使用单独的相关性或协方差矩阵，因为每种结果都有足够的观测值（n=500 和 n=268）。如果其中任何一个类别相对于另一个类别要小得多，我们可以改用汇总（或总体）协方差矩阵，因为这将涵盖更大的观测值集合。

关于以下代码的一些注意事项：

- 提醒一下，在模拟之前始终先设置随机种子。这将确保每次运行代码时都能获得相同的随机结果。
- `cor()` 函数将计算所有变量之间的相关矩阵，而 `cov()` 函数计算协方差矩阵。相关矩阵与协方差矩阵的主要区别在于，协方差的单位与变量相同，而相关性将关系标准化为 0 到 1 之间的值。然而，它们本质上衡量的是同一事物。
- `nbins` 变量将数据分成每个包含 400 个观测值的单独 bin。这对于提取较小的数据样本（而不是对整个数据集进行分析）非常有用。同样，这是另一种表明抽样是你朋友的方式。
- 数据集是使用 `mvrnorm` 函数生成的，该函数接受均值输入（列 1-8 的均值），以及我们刚刚生成的协方差矩阵。注意：要获取有关此函数的更多信息，你可以在本 notebook 中的任何位置创建一个新单元格并运行此命令，这将调用 R 帮助系统：help(mvrnorm, package="MASS")
- 要增加数据框的大小，我们生成包含 n1 个观测值的数据框，然后由 rep_times 变量指定重复指定次数。如果你觉得模拟耗时太长，可以将此变量降低到更小的数字，模拟将快得多完成。
- 最后，我们需要区分 data.frame 和 as.DataFrame 函数：
- data.frame 是一个普通的 R 函数，用于从函数内的参数创建数据框。
- as.DataFrame 是一个 SparkR 函数，用于将结果转换为 Spark 数据框。
- 如果不确定，请在函数前加上 base:: 或 SparkR:: 前缀，以明确指定其来源的包。否则，你可能会收到难以调试的警告或错误消息。
- 最后一行代码使用 nrow() 函数打印 Spark 数据框的行数。

运行下一代码块：

```
#generate spark dataframe
set.seed(123)
n1=28
n2=500
#data loaded into spark
#use separate correlation matrix for now. May want to use 1 pooled
correlationMatrix <- cor(PimaIndians[PimaIndians$diabetes =='pos'
covarianceMatrix <- stats::cov(PimaIndians[PimaIndians$diabetes =
require(MASS)
nbins1=base::round(n1/400,0)
out_sd1 <- as.DataFrame(data.frame(data.frame(data.frame(
sample.bin=base::sample(1:nbins1,n1,replace=TRUE) * (+1),
outcome=1,
mvrnorm(n1, mu = means.pos, Sigma = matrix(covarianceMatrix, ncol))[rep(1:n1, times=200), ])

nrow(out_sd1)
```

```
(1)Spark Jobs
Loading required package: MASS
Attaching package: 'MASS'
The following object is masked from 'package:SparkR':
    select
[1] 536000
Command took 2.88 minutes -- by r_winters at 2/13/2017, 3:15:24 PM
```

## 检查对象类型

你可以通过发出 str() 函数来查看生成的对象是 Spark 数据框。输出将明确说明 SparkDataFrame，后跟变量和数据类型的描述。但是，它不会列出行数：

```
str (out_sd1)
```

```
> (1) Spark Jobs
'Spark DataFrame': 10 variables:
 $ sample_bin : num 78 212 110 237 253 13
 $ outcome : num 1 1 1 1 1
 $ pregnant : num 1.94252031814449 4.36423863864429 0.858675776495492 1.855456883345 9.17100400066027 1.1186379328449
 $ glucose : num 157.123189654394 148.943347099497 172.842735207286 107.366227123907 105.302689867468 188.881198462925
 $ pressure : num 61.5029879083116 71.69507301027 74.4269535626088 74.3844944342854 78.1355932536701 61.884344482885
 $ triceps : num 15.6917644205514 41.5591691064547 30.0192322177519 34.9364879662772 33.0035759583851 28.1923108753192
 $ insulin : num 129.597011349131 240.029631091656 379.471775333015 86.1971329956682 -67.8238532621351 181.68231106527
 $ mass : num 29.4401765315476 24.953572101178 32.846294484615 42.3403328136571 35.6341563357795 42.5202651318031
 $ pedigree : num 1.22263036150258 0.313509501680247 0.445328027648939 0.52963571807743 0.385501592130751 0.25120511045
 $ age : num 25.8341329573308 19.4383095827238 46.3093049981371 16.6689622302294 26.5530890692054 26.1512830183403
```

## 模拟阴性病例

我们刚刚模拟了阳性病例。现在，让我们设置一些类似的代码来模拟非糖尿病患者（outcome=0）。

对于阴性病例，我们还将 `sample.bin` 乘以 -1，以便在将来，我们知道所有正的 `sample.bin` 实例都对应阳性病例，而所有负的 `sample.bin` 实例都对应阴性病例：

```
set.seed(123)
nbins2=base::round(n2/400,0)
correlationMatrix <- cor(PimaIndians[PimaIndians$diabetes =='neg'
covarianceMatrix <- stats::cov(PimaIndians[PimaIndians$diabetes =
out_sd2 <- as.DataFrame(data.frame(data.frame(
   sample.bin=base::sample(1:nbins2,n2,replace=TRUE) * (-1),
   outcome=0,
   mvrnorm(n2, mu = means.neg, Sigma = matrix(covarianceMatrix, ncol))[rep (1:n2, times=2000), ]]))
nrow(out_sd2)
```

输出表明生成了 500,000 行。请注意，它还表明运行了两个 Spark 作业才能获得结果。对于好奇者，你还可以点击三角形图标，从处理角度显示有关 Spark 作业的详细信息，（尽管它不包含任何结果输出）。

- > (2) Spark 作业

[1] 5e+05

## 将阳性和阴性病例合并为单个 Spark 数据框

我们现在有两个分别对应阳性和阴性病例的独立 Spark 数据框。对于某些类型的分析，保持结果分开是合理的；但是，为了说明目的，我们将使用 unionAll() 函数将它们合并为一个单一数据集。

```
out_sd <- unionAll(out_sd1, out_sd2)
nrow(out_sd)
```

nrow 的输出表明总共有 768,000 行。这个数字代表我们原来的 768 行，已乘以 1000 的因子：

```
(1) Spark Jobs

[1] 768000
```
