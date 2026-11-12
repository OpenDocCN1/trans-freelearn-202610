# 第 12 章. Spark 模型 – 基于规则的学习

在本节中，我们将学习如何实现一些基于规则的算法。这些算法的实现方法取决于您使用的语言接口以及正在运行的 Spark 版本。

对于 Spark 2.0，仅 Scala 和 Python 支持基于规则的决策树。因此，为了演示如何直接在 Spark 中构建决策规则，我们将举例说明如何使用 Python 来确定被搜身的规则。

对于其他语言，例如 R，目前无法直接在 Spark dataframe 上运行决策树算法；但是，可以使用其他方法来生成准确的树。

我们将演示如何先从 Spark 提取样本，下载到基础 R，然后运行我们常用的工具，例如 rpart。大型数据集通常包含比决策树所需多得多的数据，因此从大数据源中适当抽样、将数据从 Spark 提取到 R 并运行标准工具是非常合理的。

我们将介绍 One Rule (OneR) 包，以了解每个单变量如何预测结果。基于 OneR “少即是多”的理念，我们将演示 OneR 如何优化地对数值进行分箱，并使用决策树规则确定哪些是最主要的预测因子。您也可以使用 OneR 包进行特征选择。




## 加载停止和盘查数据集

我们将使用上一章构建的糖尿病数据集。对于其他一些决策树示例，我们需要加载停止和盘查数据集。您可以从以下 URL 获取此数据集：http://www1.nyc.gov/site/nypd/stats/reports-analysis/stopfrisk.page.

选择 2015 年 CSV 压缩归档，下载并提取文件到项目目录，例如 C:/PracticalPredictiveAnalytics/Data，并将文件命名为 2015_sqf_csv

## 将 CSV 文件导入 Databricks

Databricks 包含一个简单的用户界面，允许您将文件加载到 Databricks HDFS 文件系统。或者，您可以将文件直接加载到 **亚马逊云服务** (AWS) 并直接从 Databricks API 读取文件。

- 1. 切换到 Databricks 应用程序，选择 `Tables`，然后选择 `Data Import`。请注意，在某些版本的 Databricks 中，此功能位于 Data 菜单下：选择 "Tables"，然后单击 +。
- 2. 系统可能会提示您创建新集群。如果是，请先按照我们在 **第 9 章** 中创建新集群的说明进行操作。
- 3. 将名为 `2015_sqf_csv` 的文件从 PracticalPredictiveAnalytics 目录拖到数据导入框中，如下所示：

### 创建表
### 数据导入

- 4. 单击 **预览表**。
- 5. 将 **表名** 输入为 `stopfrisk`。
- 6. 观察变量的分配方式（它们全部分配为字符串）；但是，不要更改任何属性。只需单击 **创建表**（左侧的蓝色按钮）：

### 创建表
### 表详细信息

| _c0 | _c1 | _c2 | _c3 | _c4 | _c5 |
| --- | --- | --- | --- | --- | --- |
| STRING | STRING | STRING | STRING | STRING | STRING |
| year | pct | ser_num | datestop | timestop | recstat |
| 2015 | 61 | 18 | 1012015 | 315 | 1 |
| 2015 | 22 | 5 | 1152015 | 1747 | A |
| 2015 | 20 | 36 | 1292015 | 1745 | 1 |
| 2015 | 20 | 38 | 1292015 | 1745 | 1 |
| 2015 | 20 | 41 | 1292015 | 1745 | 1 |
| 2015 | 20 | 39 | 1292015 | 1745 | 1 |

## 读取表

完成上述步骤后，您刚刚创建的表将在 Databricks 系统中注册，并在会话间保持持久，即您每次登录时都不需要重新加载数据。

## 运行第一个单元格

首先运行第一个单元格（也称为代码块），它将获取按年份统计的记录数量。您可以通过从本书网站下载来访问本章中的代码。或者，您可以将以下代码的每个部分复制到新单元格，以此方式创建自己的笔记本。

由于停止和盘查已导入并已注册为表，我们可以开始使用 SQL 读取一些计数，以查看文件有多大：

```
#embed all SQL within the sql() function
yr <- sql("SELECT year,frisked,count(*) as year_cnt FROM stopfrisk")
display(yr)
```

几秒钟后，输出将以简单的格式化表格形式出现。使用 year_cat 列的简单计算表明，对于 2015 年，68% 被盘查的人也被搜身了：

- (5) Spark 作业

| year | frisked | year_cnt |
|---|---|---|
| 2015 | N | 7306 |
| 2015 | Y | 15257 |

## 将整个文件读入内存

现在我们有了一些汇总计数，对即将查看的内容有了一些了解。因此，我们将通过 SQL 将整个文件读入 Spark 数据帧并打印第一条记录（如果需要，可以打印更多）。

仅查看第一条（或几条）记录，您就会看到其中包含二进制是/否标志和一些数值变量：

```
df <- sql("SELECT * FROM stopfrisk")
head(df,1)
```

- (1) Spark 作业

```
year pct ser_num datestop timestop recstat inout trhsloc perobs crimsusp
1 2015 61 18 1012015 315 1 0 P 2.00 FELONY
perstop typeofid explnstp othpers arstmade arstoffn sumissue sumoffen
1 10 V Y N N N N
compyear comppct offunif offcrid frisked searched contrabn adtlrept pistol
1 0 0 N Y N N N N N
riflshot asltweap knifcuti machgun othrweap pf_hands pf_wall pf_grnd pf_drwep
1 N N N N N N N N N N
pf_ptwep pf_baton pf_hcuff pf_pepsp pf_other radio ac_rept ac_inves rf_vcrim
1 N N N N N N N Y N N
rf_othsw ac_proxm rf_attir cs_objs cs_descr cs_casng cs_lkout rf_vcact
1 N Y N N Y Y N N
cs_cloth cs_drgt r ac_evasv ac_assoc cs_furtv rf_rfcmp ac_cgdir rf_verbl
1 N N N N N N Y N N
cs_vcrim cs_bulge cs_other ac_incid ac_time rf_knowl ac_stsnd ac_other
1 N N N Y Y N N N
sb_hdobj sb_outln sb_admis sb_other repcmd revcmd rf_furt rf_bulg offverb
1 N N N N 186 186 N N V
offshld forceuse sex race dob age ht_feet ht_inch weight hairclr eyecolor
1 S M W 33 5 11 190 BR BR
build othfeatr addrtyp rescove premtype premname addrnum stname stinter
1 M L STREET AVENUE W
crosst aptnum city state zip addrpct sector beat post xcoord
1 EAST 28 STREET BROOKLYN 61 E 7 1000091
ycoord dettypCM lineCM detailCM
1 156314 CM 1 14
```

## 将某些变量转换为整数

如果在数据帧的变量上运行 str() 函数，您会看到它们都是 chr（字符）类型。我们在导入文件对话框中也看到了这一点：

```
str(df)

'SparkDataFrame': 112 variables:
 $ year  : chr "2015" "2015" "2015" "2015" "2015" "2015"
 $ pct   : chr "61" "22" "20" "20" "20" "20"
 $ ser_num : chr "18" "5" "36" "38" "41" "39"
 $ datestop: chr "1012015" "1152015" "1292015" "1292015" "1292015" "1292015"
 $ timestop: chr "315" "1747" "1745" "1745" "1745" "1745"
```

我们希望其中一些变量为整数，因此可以使用 cast() 函数将其中一些变量更改。还要使用 trim() 函数清理一些前导和尾随空格：

```
df$age <- cast(trim(df$age),'integer')
df$perstop <- cast(df$perstop,'integer')
df$perobs <- cast(df$perobs,'integer')
df$ser_num <- cast(df$ser_num,'integer')
df$datestop <- cast(df$datestop,'integer')
df$timestop <- cast(df$timestop,'integer')
df$repcmd <- cast(df$repcmd,'integer')
df$revcmd <- cast(df$revcmd,'integer')
df$compyear <- cast(df$compyear,'integer')
df$comppct <- cast(df$comppct,'integer')
df$ht_feet <- cast(df$ht_feet,'integer')
df$ht_inch <- cast(df$ht_inch,'integer')
```

```
df$height <- df$ht_feet*12+df$ht_inch
df$weight <- cast(df$weight,'integer')
```

```
str(df)
```

在代码末尾，我们运行 str() 函数，以验证这些变量已更改为整数。您还会看到创建了一个额外变量（height），计算方式为英尺*12 + 英寸。出于我们的目的，无需分别保留英尺高度和英寸高度的变量：以下是 str() 输出的第一页。请注意，ser_num、datestop、timestop、perobs 和 perstop 已映射为整数。

```
'SparkDataFrame': 113 variables:
$ year : chr "2015" "2015" "2015" "2015" "2015" "2015"
$ pct : chr "61" "22" "20" "20" "20" "20"
$ ser_num : int 18 5 36 38 41 39
$ datestop: int 1012015 1152015 1292015 1292015 1292015 1292015
$ timestop: int 315 1747 1745 1745 1745 1745
$ recstat : chr "1" "A" "1" "1" "1" "1"
$ inout : chr "0" "0" "0" "0" "0" "0"
$ trhsloc : chr "P" "P" "P" "p" "p" "p"
$ perobs : int 2 1 1 1 1 1
$ crimsusp: chr "FELONY" "FELONY" "MISD" "MISD" "MISD" "MISD"
$ perstop : int 10 4 16 16 16 16
```

## 发现重要特征

我们将现在介绍 OneR 包，以发现数据集中的一些重要特征。OneR 包将为每个特征生成一条决策规则，然后按准确性对它们进行排名。准确性定义为正确分类结果的概率，可以表示为混淆矩阵或错误矩阵，我们在之前的章节中见过。OneR 包还有一些其他很好的功能，例如能够以最佳方式对整数变量进行分箱，从而产生最佳预测器。

OneR 包不在 Spark 上原生运行，因此我们首先需要使用 collect() 和 sample() 函数对 Spark 数据帧进行 95% 采样，然后通过 collect() 函数将其移动到本地 R 数据帧。

虽然这个 Spark 数据帧足够小，可以不进行采样就完成示例，但了解如何从数据帧进行采样很重要，因为如果您按预期使用 Spark，您的数据帧将太大，无法使用 collect 函数将整个数据帧带入本地 R 实例。对于非常大的 Spark 表，您可能希望在初始分析时使用 5%-10% 左右的采样百分比。但是，对于我们的示例，我们将使用 95% 的采样百分比以捕获大部分数据，因为 2015 年停止和盘查数据相对较小。但是，如果您要导入所有年份的数据并将其连接到一个大的 Spark 数据帧中，则会考虑将采样百分比降低到更合理的数字。否则，您可能最终会得到基础 R 无法处理的更多行。

另请注意，OneR 包当前不是默认安装的包。但是，您可以使用 devtools 从 cran 安装最新版本：

```
devtools::install_github("cran/OneR")
library(OneR)
```




### 将名为 "df" 的 Spark DataFrame 中 95% 的行加载到本地
local = collect(sample(df, 0.95))

### 消除具有大量水平的某些因子

由于数据集在某些特征（如邮政编码和 GPS 位置变量）上非常细致，我们将首先通过仅包含最多 20 个水平的特征/变量来执行降维。这将有助于我们之后构建模型，而无需担心由于某些具有高维度的变量而导致过拟合。不过，数字 20 并非一成不变；你可以更改默认值，并且无论水平如何，始终保留你认为模型中需要的变量。

OneR 具有一个名为 maxlevels() 的函数，可以完成此操作。任何水平数超过此值的变量都不会包含在输出中。

运行该函数后，使用 str() 函数验证某些变量已被消除。我们最初有 113 个变量，现在减少到 97 个。一些变量（例如 x 和 y 坐标）已被消除，因为 GPS 坐标显然具有超过 20 个水平：

```
frisked_df <- maxlevels(local, maxlevels = 20, na.omit = TRUE)
#check the output to see if the number of levels were reduced
str(frisked_df)
```

观察输出的前 3 行，显示变量数量已从 113 减少到 97：

```
'data.frame': 21359 obs. of 97 variables:
$ year : chr "2015" "2015" "2015" "2015" ...
$ ser_num : int 18 5 38 41 39 122 42 9 141 2 ...
```

### 测试和训练数据集

当然，我们会希望将数据划分为测试数据集和训练数据集。

我们已经介绍了许多将数据分为测试集和训练集的方法。下一种划分方法将采用 80%/20% 的训练/测试分割，具体步骤如下：

- 首先从训练数据集中提取索引编号。我们将使用 base::sample 函数来完成此操作
- 根据这些索引构建训练数据集
- 从不包含在索引中的行构建测试数据集

一旦我们获得训练索引，将使用 OneR 包中的 optbin 函数，该函数将根据变量预测 frisked 的 Yes 或 No 结果的能力，对数值变量进行最优划分。我们之前在决策树中已经见过这种最优划分。在下面的代码中，optbin 被指示为所有数值变量找到最佳分割点，方法是指定 frisked ~ 。

```
set.seed(123)
train.index <- base::sample(1:base::nrow(frisked_df), 0.80 * base::nrow(frisked_df))
train_data <- optbin(frisked_df[train.index,], frisked ~ .)
test_data <- optbin( frisked_df[-train.index,], frisked ~ .)
#after partitioning, run a summary of the first 15 rows to see what
summary(train_data[1:15])
```

```
year ser_num datestop
2015:16034 (-0.523,265] :10299 (1e+06,6.13e+06] :8689
(265,1.53e+03]: 5735 (6.13e+06,1.23e+07]:7345

timestop recstat inout trhsloc perobs
(-2.36,1.37e+03] :6410 : 2043 I: 2955 H: 2387 (-0.36,2.74]:12670
(1.37e+03,2.36e+03]:9624 1:10454 0:13079 P:13009 (2.74,360] : 3364
9: 8 T: 638
A: 3529

perstop typeofid explnstp othpers arstmade sumissue compyear
(0.911,8.1]:9942 0: 308 N: 20 N:11285 N:13211 N:15591 0:16034
(8.1,90.1] :6092 P:9279 Y:16014 Y: 4749 Y: 2823 Y: 443
R: 454
V:5993
```

### 检查分桶数据

在前面的输出中，观察到每个数值变量已转换为两个分类桶中的一个。这两个分桶类别由用 (,) 分隔的起始和结束范围表示。由于 summary 使用起始 ( 和结束 ] 来标识它们，因此很容易识别：

使用特殊的 Databricks display 命令查看 train_dataset。

```
`display(train_data)`
```

» (2) Spark 作业

| year | ser_num | datestop | timestop | recstat | inout | trhsloc | perobs | perstop | typeofid | explnstp | othpers | arstmade |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 2015 | (-0.523,265] | (1e+06,6.13e+06] | (1.37e+03,2.36e+03] | A | O | P | (-0.36,2.74] | (0.911,8.1] | P | Y | N | N |
| 2015 | (-0.523,265] | (6.13e+06,1.23e+07] | (1.37e+03,2.36e+03] | I | O | P | (-0.36,2.74] | (0.911,8.1] | V | Y | N | N |
| 2015 | (265,1.53e+03] | (1e+06,6.13e+06] | (-2.36,1.37e+03] | I | O | P | (2.74,360] | (8.1,90.1] | P | Y | N | N |
| 2015 | (-0.523,265] | (6.13e+06,1.23e+07] | (1.37e+03,2.36e+03] | O | O | P | (-0.36,2.74] | (0.911,8.1] | V | Y | N | Y |
| 2015 | (-0.523,265] | (6.13e+06,1.23e+07] | (1.37e+03,2.36e+03] | I | I | H | (-0.36,2.74] | (8.1,90.1] | P | Y | N | N |
| 2015 | (-0.523,265] | (1e+06,6.13e+06] | (-2.36,1.37e+03] | I | I | H | (-0.36,2.74] | (0.911,8.1] | P | Y | N | N |
| 2015 | (-0.523,265] | (1e+06,6.13e+06] | (-2.36,1.37e+03] | I | I | P | (2.74,360] | (0.911,8.1] | P | Y | Y | N |
| 2015 | (-0.523,265] | (6.13e+06,1.23e+07] | (-2.36,1.37e+03] | I | O | P | (2.74,360] | (8.1,90.1] | V | Y | Y | N |

显示前 1000 行。

### 运行 OneR 模型

OneR 模型的语法应该很熟悉。结果变量 frisked 指定在公式 (~) 的左侧，特征指定在右侧。如你所记得，元字符 (.) 指定将使用所有特征作为预测器：

```
model <- OneR(train_data, frisked ~ ., verbose = TRUE)
summary(model)
```

（部分）摘要输出显示基于仅选择一个变量作为预测器及其分类率的准确率。显著变量带有星号。

接下来显示前 7 个变量的属性和准确率指标。请注意，一旦准确率达到 67.61%，它就不会下降：

| | 属性 | 准确率 |
|---|---|---|
| 1 | * sex | 68.56% |
| 2 | eyecolor | 67.68% |
| 3 | haircolor | 67.62% |
| 4 | year | 67.61% |
| 4 | ser_num | 67.61% |
| 4 | datestop | 67.61% |
| 4 | timestop | 67.61% |

函数调用在日志中显示，决策树规则已显示。

```
Call:
OneR(data = train_data, formula = frisked ~ ., verbose = TRUE)

Rules:
If sex = F then frisked = N
If sex = M then frisked = Y
If sex = Z then frisked = Y

Accuracy:
11728 of 17105 instances classified correctly (68.56%)

Contingency table:
    sex
frisked   F     M     Z  Sum
  N   * 668  4822   50 5540
  Y   505  * 10968  * 92 11565
  Sum 1173 15790  142 17105
---
Maximum in each column: '*'

Pearson's Chi-squared test:
X-squared = 348.29, df = 2, p-value < 2.2e-16
```

### 解读输出

摘要输出很好地体现了每个变量作为单独预测器的重要性。所有准确率指标的范围在 67.61% 到 68.56% 之间，因此没有哪个单一变量明显优于其他变量来预测是否被盘查。

不过，排名靠前的变量是 sex，其准确率为 68.56%。将结果分类为被盘查或未被盘查的唯一简单规则是：

如果 sex 为男性或未知，则 68.56% 的时间结果被正确预测。

这在下面两个手动计算的列联表中有所体现，其中星号 * 表示正确分类（以下背景为绿色）。我手动添加了第二个列联表，它给出的是百分比，而不是输出中提供的原始计数。

以下是一个包含计数和百分比的混淆矩阵：

| frisked | F | M | Z | 总计 | 正确分类总数 |
|---|---|---|---|---|---|
| N | 668 | 4,822 | 50 | 5,540 | |
| Y | 505 | 10,968 | 92 | 11,565 | 11,728 |
| Sum | 1,173 | 15,790 | 142 | 17,105 | |

| frisked | F | M | Z | 总计 | 正确分类总数 |
|---|---|---|---|---|---|
| N | 4% | 28% | 0% | 32% | 69% |
| Y | 3% | 64% | 1% | 68% | |
| Sum | 7% | 92% | 1% | 17,105 | |

- 如果 sex=M，则 64% 的时间被盘查被正确预测。
- 如果 sex=F，则 4% 的时间未被盘查被正确预测。

虽然百分比很小，但你可以看到，一般来说，男性被盘查的次数远多于女性。你还可以由此推导一些条件概率：如果你是女性，则有 3/7 或 43% 的几率会被盘查。但是，如果你是男性，则有 64/92 或 70% 的几率会被盘查。

如果 sex=Z（未知），则 1% 的时间被盘查被正确预测。

### 构建新变量

尽管 OneR 通过寻找单个最佳预测器来工作，但我们也可以通过构建一些新变量并诱使 OneR 认为它是一个变量，来尝试改进预测。

在下面的代码中，我们通过组合 build、eyecolor 和 hair color 等单个变量的属性来创建新变量。再次运行 OneR 模型后，你可以看到，在预测准确率方面它有所帮助，并且其中一个新变量（eye_hair_build）现在排名第二。




这是一个演示如何执行“特征工程”的简单示例。

```
tmp <- train_data
tmp$eye_hair_build <- paste(train_data$eyecolor, train_data$haircolor)
tmp$weapon <- paste(train_data$knifcuti, train_data$riflshot, train)
model <- OneR(tmp, frisked ~ ., verbose = TRUE)
```

| 属性 | 准确率 |
|---|---|
| 1 * 性别 | 68.56% |
| 2 eye_hair_build | 68.07% |
| 3 eyecolor | 67.68% |
| 4 haircolor | 67.62% |

### 在测试样本上运行预测

eval_model 函数将预测规则应用于测试数据，
并生成混淆矩阵，包含绝对计数和相对
百分比，因此无需手动计算百分比，如我们
上面所做的那样。

将模型结果应用于测试样本显示准确率与
训练样本相似，这在一定程度上验证了结果。

错误率是错误分类的百分比，相当于
1 减去准确率：

```
prediction <- predict(model, test_data,type="class")
#Evaluate prediction statistics

eval_model(prediction, test_data)
```

```
Confusion matrix (absolute):
        actual
prediction   N   Y  Sum
N           153 122 275
Y          1269 2722 3991
Sum        1422 2844 4266

Confusion matrix (relative):
        actual
prediction   N     Y     Sum
N           0.04  0.03  0.06
Y           0.30  0.64  0.94
Sum         0.33  0.67  1.00
```

准确率：
0.6739 (2875/4266)

错误率：
0.3261 (1391/4266)

### 另一个 OneR 示例

此示例使用更大的糖尿病数据集。由于该数据集中的大多数变量都是数值型，OneR 可以对所有变量进行分箱：

- 首先，使用 SQL 读取 Spark 糖尿病表，该表已在前一章中注册。
- 从数据中收集 15% 的随机样本，并将其分配给名为 "local" 的 R（非 Spark！）数据框。
- 根据所有可用变量预测结果的能力对其进行分箱，并将其分配给名为 "data" 的 R 数据框：

```
library(OneR)
df = sql("SELECT outcome, age, mass, triceps, pregnant,
glucose, pressure, insulin, pedigree
FROM global_temp.df_view")

local = collect(sample(df, F, .15))

data <- optbin(local,outcome~.)
summary(data)
```

| 年龄 | 质量 | 肱三头肌 | 怀孕 | 葡萄糖 | 压力 | 胰岛素 | 系谱 | 结果 |
|---|---|---|---|---|---|---|---|---|
| (-1.54,37.1]:118790 | (12.4,35]:121754 | (0.703,32.8]:128435 | (-5.29,4.91]:117506 | (38.9,139]:139019 | (35.3,75.6]:114164 | (-149,205]:131628 | (-0.548,0.529]:111914 | 0:121724 |
| (37.1,74]: 67698 | (35,54.2]: 64734 | (32.8,58.3]: 58053 | (4.91,13.8]: 68982 | (139,220]: 47469 | (75.6,112]: 72324 | (205,447]: 54860 | (0.529,1.69]: 74574 | 1: 64764 |

4. 使用所有变量运行 OneR 模型来预测结果。请记住，结果是指示是否患有糖尿病：

```
model <- OneR(data, outcome~., verbose = TRUE)
summary(model)
prediction <- predict(model, data)
```

5. 评估预测统计信息。请注意，与其他预测变量相比，葡萄糖和胰岛素具有更好的单变量准确率结果：

```
eval_model(prediction, data)
```

| 属性 | 准确率 |
|---|---|
| * 葡萄糖 | 76.54% |
| 胰岛素 | 72.25% |
| 肱三头肌 | 65.73% |
| 年龄 | 65.27% |
| 质量 | 65.27% |
| 怀孕 | 65.27% |
| 压力 | 65.27% |
| 系谱 | 65.27% |

由于准确率和并列方法（如适用）而选择的属性：'*'

### 规则部分

既然我们知道了重要变量，是时候检查 OneR 包如何输出决策规则了。由于 glucose 被认为是最佳单一预测变量，预测糖尿病的决策规则显示在 Rules: 部分下，而 glucose 的最优截止点（139）显示在下面的列联表中。

```
Call:
OneR(data = data, formula = outcome ~ ., verbose = TRUE)
```

```
Rules:
If glucose = (38.9,139] then outcome = 0
If glucose = (139,220] then outcome = 1
```

准确率：
186488 个实例中有 142741 个被正确分类（76.54%）

列联表：

| 结果 | (38.9,139] | (139,220] | 总计 |
|---|---|---|---|
| 0 | * 108498 | 13226 | 121724 |
| 1 | 30521 | * 34243 | 64764 |
| 总计 | 139019 | 47469 | 186488 |

---
每列最大值：'*'

Pearson 卡方检验：
X-squared = 39311, df = 1, p-value < 2.2e-16

现在，查看混淆矩阵列联表——一个用于绝对计数，另一个用于相对百分比。

在我们的示例中，葡萄糖水平大于或等于 139 在 18% 的总病例中预测糖尿病。但是，请注意，大部分预测能力来自预测非事件。也就是说，56% 的正确预测来自小于 139 的葡萄糖读数，这预测了糖尿病的不存在。这很有价值，因为它可能有助于确定预防糖尿病的因素，但我们可能也对能够预测糖尿病的模型的准确率感兴趣。

这将涉及更深入地挖掘变量之间的交互作用，并需要更复杂的算法。但是，OneR 确实为变量选择提供了一个有趣的框架。

混淆矩阵（绝对计数）：

| 预测 | 实际 0 | 实际 1 | 总计 |
|---|---|---|---|
| 0 | 108498 | 30521 | 139019 |
| 1 | 13226 | 34243 | 47469 |
| 总计 | 121724 | 64764 | 186488 |

混淆矩阵（相对百分比）：

| 预测 | 实际 0 | 实际 1 | 总计 |
|---|---|---|---|
| 0 | 0.58 | 0.16 | 0.75 |
| 1 | 0.07 | 0.18 | 0.25 |
| 总计 | 0.65 | 0.35 | 1.00 |

准确率：
0.7654 (142741/186488)

错误率：
0.2346 (43747/186488)

### 使用 Rpart 构建决策树

虽然 OneR 非常擅长确定简单的分类规则，但它无法构建完整的决策树。但是，我们可以从 Spark 提取样本并将其路由到任何 R 决策树算法，例如 rpart。

### 首先收集样本

为了说明这一点，让我们首先从 stop and frisk 数据框中提取 50% 的样本。我们还希望确保提取的数据量可以被 base R 轻松处理，base R 有取决于 CPU 大小的内存限制。

- 下面的代码将首先从 Spark 提取 50% 的样本，并将其存储在名为 `dflocal` 的本地 R 数据框中。
- 然后它将运行 `str()` 命令来验证行数和元数据：

```
dflocal = collect(sample(df, F, .50, 123))
str(dflocal)
```

输出表明有 11,311 行，大约是 Stop and Frisk 数据中 22,563 行的 50%。

```
'data.frame': 11311 obs. of 113 variables:
 $ year  : chr "2015" "2015" "2015" "2015" ...
 $ pct   : chr "20" "20" "67" "68" ...
 $ ser_num : int 41 39 122 9 141 2 1 5 2 1 ...
 $ datestop: int 1292015 1292015 2062015 2142015 3142015 1012015 1012015 1012015 1012015 1012015 ...
 $ timestop: int 1745 1745 2155 200 15 10 50 115 229 230 ...
 $ recstat : chr "1" "1" "A" ...
 $ inout   : chr "O" "O" "O" "O" ...
 $ trhsloc : chr "P" "P" "P" "P" ...
 $ perobs  : int 1 1 2 1 1 2 5 1 1 ...
 $ crimsusp: chr "MISD" "MISD" "FEL" "FEONY" ...
 $ perstop : int 16 16 5 10 15 5 45 1 5 ...
 $ typeofid: chr "Y" "Y" "R" "V" ...
 $ explnstp: chr "Y" "Y" "Y" "Y" ...
 $ othpers : chr "Y" "Y" "N" "Y" ...
 $ arstmade: chr "N" "N" "N" "N" ...
 $ arstoffn: chr " " " " " " " " ...
 $ sumissue: chr "N" "N" "N" "N" ...
 $ sumoffen: chr " " " " " " " " ...
```

### 使用 Rpart 构建决策树

我们将把 rpart 算法作为回归树运行。请记住，当输出变量是数值形式而非名义形式时，使用回归树。在这样做之前，我们需要将 frisked=Y/N 映射为 frisked=1/0，并在调用中指定 method="anova"。

代码的最后一行 (fit) 将把决策规则以文本形式打印到控制台。

height、sex、age 和 city 参数作为主要分割出现：

```
set.seed(123)
library(rpart)
dflocal$frisked_bin <- ifelse(dflocal$frisked=="Y",1,0)
fit <- rpart(frisked_bin ~ sex + age + weight + height + perstop
fit
```




### 绘制树图

查看树的可视化表示要容易得多。因此，您需要安装 rpart.plot（如果尚未安装），以便能够查看它。

可以看到，第一条规则对应于主要规则（按 Gender 拆分），该规则之前由本章的 OneR 包生成。

```
chr [1:11311] "N" "N" "Y" "N" "Y" "Y" "Y" "Y" "Y" "Y" ...
n= 11311
```

```
node), split, n, deviance, yval
* denotes terminal node
```

```
1) root 11311 2482.374000 0.6747414
 2) sex=F 753  185.620200 0.4409031
   4) height< 65.5 418   98.988040 0.3851675  *
   5) height>=65.5 335   83.713430 0.5104478  *
 3) sex=M,Z 10558 2252.643000 0.6914188
   6) city=BROOKLYN,MANHATTAN,QUEENS,STATEN IS 8339 1841.411000 0.6708238
    12) age>=28.5 2908  683.906500 0.6217331  *
    13) age< 28.5 5431 1146.745000 0.6971092  *
   7) city=BRONX 2219  394.402000 0.7688148
    14) age>=60.5 26    6.346154 0.4230769  *
    15) age< 60.5 2193  384.911100 0.7729138  *
```

```
devtools::install_github("cran/rpart.plot")
library(rpart.plot)
rpart.plot(fit,cex=.75)
```

```
0.67
100%

sex = F
yes                 no

0.44               0.69
7%                 93%

height < 66   city = BROOKLYN,MANHATTAN,QUEENS,STATEN IS

0.39   0.51           0.62   0.7     0.42   0.77
4%     3%            26%    48%     0%    19%

           age >= 28       age >= 60
```

### 在 Python 中运行替代模型

在此示例中，我们通过从 Spark DataFrame 提取样本并在 R 基础包中运行树模型，在 R 中运行了决策树。虽然这完全可以接受（因为它迫使您思考采样问题），但在许多情况下，直接在 Spark DataFrame 上使用 `MLlib` 包或等效工具运行模型会更高效。

对于您正在使用的 Spark 版本（2.1）；决策树算法无法在 R 下运行。幸运的是，原生 Spark 决策树已经在 Python 和 Scala 中实现。我们将使用 Python 来说明示例，以便您可以看到可用的选项。如果您关注 Spark 中的算法开发，您会发现算法通常首先用 Scala 编写，因为那是 Spark 的原生语言。

### 运行 Python 决策树

以下是关于下面出现的 Python 决策树代码的一些说明。

对于第一个代码块，请注意代码第一行中的“魔法”指令。魔法指令以 '%' 开头，并指定您将使用哪种语言或 API。因此，由于我们将使用 Python 而不是 R，需要将 `%python` 指定为第一行。这是因为笔记本最初设置为 R 笔记本。混合语言时，需要将使用的语言指定为第一行，除非您正在使用默认笔记本语言编写。

对于不熟悉 Python 的人，代码顶部的 import 指令导入所需的库和模块，这些库和模块将由 Python 使用，类似于 R 中使用 library 函数。如果不提供 import 指令，通常会收到错误。

### 读取 Stop and Frisk 表格

第一个代码块将读取 StopFrisk DataFrame，类似于本节前面使用 spark.sql 执行的操作。请注意，使用 Python 编写的 SQL 语法与我们之前在 R 中使用的非常相似。

在 SQL 调用中，结果变量 frisked 使用 CASE 语句映射为二进制变量。这样做的原因是 MLlib 算法处理整数数据比字符数据好得多。如果使用字符数据，通常需要将其映射为整数或标记点。

然后使用 show(5) 函数显示生成的 DataFrame（df2），这是 R 中 head(df2, 5) 函数的 Python 等效项：

```
%python
from pyspark.mllib.tree import DecisionTree, DecisionTreeModel
from pyspark.mllib.util import MLUtils
import pyspark.mllib
import pyspark.mllib.regression
from pyspark.mllib.regression import LabeledPoint
from pyspark.sql.functions import *
from pyspark.ml.feature import StringIndexer
df2 = spark.sql("SELECT case when frisked='Y' THEN 1 else 0 END a
df2.show(5)
```

|frisked|sex|race|age|weight|
|---|---|---|---|---|
| 1| 0| W| 33| 190|
| 1| 0| B| 14| 140|
| 0| 0| B| 14| 140|
| 0| 0| B| 14| 180|
| 0| 0| B| 13| 160|

### 索引分类特征

索引用于优化数据访问，并以可接受的格式向特定机器学习算法提供参数。

我们将把 race 变量纳入决策树模型，因此第一步是确定 race 的不同值有哪些。我们将再次使用 SQL 按 race 统计频率。请注意，可以说 "Group by Race" 或 "Group by 1"，这是对 select 语句中指定的第一列（即 race）的简写引用：

```
%python
dfx = spark.sql("SELECT race,count(*) FROM stopfrisk group by 1")
dfx.show()
```

请注意，有八个值：Q、B、U、Z、A、W、I 和 P：

| race | count(1) |
|---|---|
| Q | 5090 |
| B | 11950 |
| U | 122 |
| Z | 298 |
| A | 1103 |
| W | 2514 |
| I | 77 |
| P | 1409 |

接下来，使用 `indexer.fit(df2) transform`。这将把字符因子（`race`）映射为数字索引（`race_indexed`）：

```
%python
indexer = StringIndexer(inputCol="race", outputCol="race_indexed")
df3 = indexer.fit(df2).transform(df2)
```

```
df3.show(15)
#drop race for the final dataframe
df4 = df3.drop("race")
```

查看 `race` 和 `race_indexed` 的对，以了解数字将如何替代字符：

### (2) Spark 作业

| frisked2 | sex2 | race | age | weight | race_indexed |
|---|---|---|---|---|---|
| 1 | 0 | W | 33 | 190 | 2.0 |
| 1 | 0 | B | 14 | 140 | 0.0 |
| 0 | 0 | B | 14 | 140 | 0.0 |
| 0 | 0 | B | 14 | 180 | 0.0 |
| 0 | 0 | B | 13 | 160 | 0.0 |
| 0 | 0 | W | 13 | 130 | 2.0 |
| 1 | 0 | B | 25 | 160 | 0.0 |
| 1 | 0 | B | 15 | 150 | 0.0 |
| 1 | 0 | B | 23 | 160 | 0.0 |
| 0 | 0 | W | 36 | 150 | 2.0 |
| 1 | 0 | B | 30 | 160 | 0.0 |
| 1 | 0 | B | 18 | 160 | 0.0 |
| 1 | 0 | Q | 48 | 160 | 1.0 |
| 1 | 0 | B | 16 | 160 | 0.0 |
| 1 | 0 | B | 26 | 155 | 0.0 |

仅显示前 15 行

我们只是查看输出的少量记录。我们真正想看到的是 race 的所有值及其计数。我们可以使用 PySpark SQL 中的 CountDistinct 子句来获取该信息：

```
%python
df3.registerTempTable("df3")
```

```
#### Perform the same query as the DataFrame above and return `expl
countDistinctDF_sql = sqlContext.sql("SELECT race, race_indexed, c
countDistinctDF_sql.show()
```

### (5) Spark 作业

| race | race_indexed | count(race) |
|---|---|---|
| W | 2.0 | 2514 |
| B | 0.0 | 11950 |
| P | 3.0 | 1409 |
| Z | 5.0 | 298 |
| Q | 1.0 | 5090 |
| A | 4.0 | 1103 |
| I | 7.0 | 77 |
| U | 6.0 | 122 |

我们仍然需要一本码本来解码 race 的值，以确保我们了解它们所代表的含义。

```
race
A          ASIAN/PACIFIC ISLANDER
B          BLACK
I          AMERICAN INDIAN/ALASKAN NATIVE
P          BLACK-HISPANIC
Q          WHITE-HISPANIC
W          WHITE
X          UNKNOWN
Z          OTHER
```

### 映射到 RDD

我们将使用的决策树算法来自标准 Spark MLlib 库。此实现要求输入格式化为标记点格式。标记点有助于指定哪些变量是目标变量，哪些是特征。

在此示例中，目标变量（frisked2）是列出的第一个变量（我们将第一个变量称为列 0），因此目标变量在 LabeledPoint 调用中指定为 line[0]。

自变量（或特征）包含在其余列中，并指定为 line[1:]。这是列 1 及之后所有列的简写，直到没有更多列为止。





在模型中引用特征时，它们也按顺序编号，对应其在数据框中出现的顺序。对于我们的模型，它们的命名如下：

- 特征 0 - 性别
- 特征 1 - 年龄
- 特征 2 - 体重
- 特征 3 - 种族

由于这是一个基于 RDD 的模型，首先使用标记点将数据框映射到 RDD，先指定目标变量，然后是特征：

```
%python
rdd1 = df4.rdd.map(lambda line:LabeledPoint(line[0],[line[1:]]))
```

打印生成的 RDD 的一部分。调试 RDD 的输出可能比较困难；然而，您可以从输出中看到，每个标记点的第一个键值对是 1 或 0。第二个键值对（括号内）是特征向量，您可以将其与预测器进行匹配并仔细检查，以确保您的标记点正确：

```
print rdd1.take(5)
```

```
(1) Spark Jobs
[LabeledPoint(1.0, [0.0, 33.0, 190.0, 2.0]), LabeledPoint(1.0, [0.0,
```

### 指定决策树模型

还记得我们讨论过的分类特征映射吗？使用分类特征信息参数来指定哪些变量是真正的分类变量，也就是说，不仅仅是映射到索引。

在我们的示例中，Frisked 有两个级别，因此指定为 0:2（具有 2 个级别的目标变量），而 Race 有 8 个级别，因此指定为 3:8（具有 8 个级别的第三个特征）：

设置适合决策树模型的其他参数：

- numClasses：这适用于结果的数量；在我们的例子中，它是 2。
- maxDepth：您希望决策树有多深？为了说明目的，我们将其设置为 2 层深，但对于实际问题，您应该尝试更深一些。请记住，如果太深，模型会过拟合。
- maxBins：在 sparkR 中，您最多只能有 32 个桶。

现在一切就绪，我们可以运行模型了。我们将创建一棵简单的树，深度不超过两层：

```
%python
model_train = DecisionTree.trainClassifier(rdd1,numClasses=2,maxDepth=2, categoricalFeaturesInfo={0:2,3:8})
```

模型完成后，使用 DebugString() 函数将其打印为文本文件。这将为您提供决策树规则：

```
print(model_train.toDebugString())
```

```
(6) Spark Jobs
DecisionTreeModel classifier of depth 2 with 7 nodes
If (feature 0 in {1.0})
If (feature 2 <= 154.0)
Predict: 0.0
Else (feature 0 > 154.0)
Predict: 1.0
Else (feature 0 not in {1.0})
If (feature 3 in {5.0,6.0,2.0,7.0,3.0,4.0})
Predict: 1.0
Else (feature 3 not in {5.0,6.0,2.0,7.0,3.0,4.0})
Predict: 1.0
```

为了对特征 0、1、2 和 3 的含义进行编码，我们需要使用之前运行过的代码本和 SQL 代码，将特征编号映射到特征名称，并映射特征的生成值。

由于决策树输出有时会给出无意义或冗余的规则，我们可以手动将输出浓缩为简单的英文描述。试着在前面的代码中发现这个问题！

```
If sex is equal to 1 (Female)
    if weight <= 154 then subject was NOT frisked
    Else if weight > 154 then Subject WAS frisked
If sex is NOT equal to 1 (Male)
    BOTH race paths lead to Subject WAS frisked
```

### 生成更大的树

有时，对于小树，决策规则可能过于琐碎或显而易见。在这种情况下，增加树中的节点数量通常是有意义的。这里我们将生成一棵 15 层深的树：

```
%python
rdd1 = df4.rdd.map(lambda line:LabeledPoint(line[0],[line[1:]]))
rdd1.take(15)
model_train = DecisionTree.trainClassifier(rdd1,numClasses=2,maxDepth=2)
print(model_train.toDebugString())
```

同样，一些规则看起来是冗余的，因此需要更多工作来将此输出转换为有意义的业务规则。

```
DecisionTreeModel classifier of depth 15 with 4291 nodes
If (feature 0 in {1.0})
If (feature 2 <= 154.0)
If (feature 1 <= 54.0)
If (feature 2 <= 129.0)
If (feature 1 <= 37.0)
If (feature 3 in {5.0,1.0,6.0,2.0,3.0,4.0})
If (feature 1 <= 23.0)
If (feature 3 in {4.0})
If (feature 1 <= 16.0)
If (feature 1 <= 15.0)
Predict: 0.0
Else (feature 1 > 15.0)
Predict: 1.0
Else (feature 1 > 16.0)
If (feature 1 <= 22.0)
If (feature 1 <= 18.0)
If (feature 1 <= 17.0)
Predict: 0.0
Else (feature 1 > 17.0)
If (feature 2 <= 117.0)
```

### 可视化树

除非您能可视化输出，否则决策树规则可能难以解释。除非您用 Scala 语言实现决策树（该语言具有使用 databricks `Display` 命令的树可视化方法），否则您的一些选择是：

首先将 Spark 输出对象解析为 JSON 格式，然后使用 `D3.js` 输入以可视化树。Github 上有一些预构建的包可以帮助您完成此操作。

将您创建的 RDD 写入文件，在 Scala 中输入 RDD，并运行 `DecisionTreeClassifier()`。然后，拟合模型并在模型上使用 databricks display 命令来显示树。

### 比较训练和测试决策树

决策树没有预测方法。但是，我们仍然可以从测试数据集和训练数据集生成树并将其进行比较以检查合理性。

使用我们的糖尿病示例，我们可以首先在训练数据上运行一个 Pyspark 决策树。同样，我们的结果变量是第一列（#0），在 LabeledPoint 引用中被引用，特征由所有后续变量（1：）指定：

```
%python
df = spark.sql("SELECT outcome, age, mass, triceps, pregnant, glucose")
#sqlDF.show()
rdd1 = df.rdd.map(lambda line:LabeledPoint(line[0],[line[1:]]))
model_train = DecisionTree.trainClassifier(rdd1,numClasses=2,maxDepth=2,maxBins=32, categoricalFeaturesInfo={})
print(model_train.toDebugString())
```

决策树已将特征 4（葡萄糖）和特征 6（胰岛素）确定为两个最重要的分割变量。这两个变量是不错的选择，因为它们都影响血糖水平：

```
> (6) Spark Jobs
DecisionTreeModel classifier of depth 2 with 7 nodes
If (feature 4 <= 144.2881556590044)
If (feature 6 <= 244.2896167908184)
Predict: 0.0
Else (feature 6 > 244.2896167908184)
Predict: 1.0
Else (feature 4 > 144.2881556590044)
If (feature 6 <= 233.49668637807514)
Predict: 1.0
Else (feature 6 > 233.49668637807514)
Predict: 1.0
```

现在，在测试数据上运行相同的代码。结果相似；但是，树反转了特征 6 和 4 的顺序。在现实世界中这是可以接受的，但如果我们生成了完全不同的树，我们需要查看两个模型为何不同，并尝试调和模型的不同版本。如果您无法解释差异，如果存在巨大差异，您可能需要重新处理甚至放弃模型：

```
%python
test = spark.sql("SELECT outcome, age, mass, triceps, pregnant, glucose")
rdd2 = test.rdd.map(lambda line:LabeledPoint(line[0],[line[1:]]))
model_test = DecisionTree.trainClassifier(rdd2,numClasses=2,maxDepth=2, categoricalFeaturesInfo={})
#model.save(spark, "/myDecisionTreeClassificationModel")
print(model_test.toDebugString())
```

(6) Spark Jobs

```
DecisionTreeModel classifier of depth 2 with 7 nodes
If (feature 6 <= 258.7717216438844)
If (feature 4 <= 134.95577326132448)
Predict: 0.0
Else (feature 4 > 134.95577326132448)
Predict: 1.0
Else (feature 6 > 258.7717216438844)
If (feature 6 <= 303.47523507134343)
Predict: 1.0
Else (feature 6 > 303.47523507134343)
Predict: 1.0
```

## 总结

至此，本章和本书结束。我一开始就说，这是一本不同于以往的预测分析书籍，我从技术和概念两个角度，涵盖了许多不同的主题。希望您从中学到了很多，它为您提供了一些可用的新算法，并让您了解了一些“旧”工具，例如 SQL，它们非常有能力完成有时需要的繁重工作。我也试图强调“小数据”、元数据和采样，希望这能帮助您更好地理解数据，仅仅因为能够单独查看单个部分。我还希望本书中的一些材料能让您与拥有不同技能的团队成员协作。这可能包括代码优化专家、统计学专家，甚至是与贵企业所有关键人员合作过并能够传达业务需求和愿望的人。当然，样样精通很棒，但预测分析既关乎倾听，也关乎行动。

我所讨论的大部分内容适用于您将参与的任何类型的分析项目，无论选择何种语言或平台。这就是为什么我想在最后包含一些云计算和 SQL 示例，以及一个 Python 示例，以证明预测分析从业者可以凭借不同的技能集聚在一起，将他们的工作整合到本书前面讨论的 6 个步骤中，以实施成功的分析方法论和框架。

建模愉快！

