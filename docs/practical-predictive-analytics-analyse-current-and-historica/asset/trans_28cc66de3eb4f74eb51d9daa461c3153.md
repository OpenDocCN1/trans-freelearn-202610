

| 受试者ID | 年龄 | 性别 | 收入 |
|---|---|---|---|
| 1 | 29 | 男 | $40,000 |
| 2 | 45 | 男 | $36,000 |
| 3 | 81 | 男 | --缺失-- |
| 4 | 22 | --缺失-- | $16,000 |
| 5 | 41 | 男 | $98,000 |
| 6 | 33 | 女 | $60,000 |
| 7 | 22 | 女 | $24,000 |
| 8 | --缺失-- | 女 | $81,000 |
| 9 | 33 | 女 | $55,000 |
| 10 | 45 | 女 | $80,000 |

但是，逐条删除法的副作用是，您可能最终会删除一些可能对模型产生重要影响的非缺失变量。但是，由于缺失数据被假定为随机，删除一些行不会产生重大影响，前提是您的假设是正确的。

另一个需要考虑的因素是您可用的数据量。您不希望因为某个缺失值而导致不得不丢弃绝大部分数据！如果您打算采用此方法，请看看是否可以从考虑中排除该变量，并从一份相当完整的变量列表开始。

例如，如果大部分记录都有邮政编码信息，但州变量仅有50%填充，那么您就不希望在执行逐条删除（例如回归分析）的算法中同时包含邮政编码和州，因为这至少会剔除50%的数据。因此，首要的解决办法是看这些变量是否一开始就有必要（可通过变量重要性度量来判断），或者是否存在填充情况更好的替代变量。主成分分析或基本的成对相关性可以帮助您识别这种情况。

## 插补方法

对缺失值进行插补，是指用另一个合理的值来替换缺失值。什么是合理的，可以从将缺失值替换为变量的均值、中位数或众数，到回归技术，再到涉及蒙特卡洛模拟的更高级技术，不等。

## 使用 'mice' 包对缺失值进行插补

在本示例中，我们将使用 `mice` 包对 `all.df` 数据框中 `age` 变量的某些缺失值进行插补。`age` 变量的值将由另外两个现有变量 `gender` 和 `education` 进行插补。

首先，安装并加载 `mice` 包：

```
install.packages("mice")
library(mice)
```

我们现在将运行 `md.pattern()` 函数，它将显示数据框中其他列上的缺失值分布。`md.pattern()` 函数的输出有助于推断哪些变量可能适合用于插补缺失值：

```
md.pattern(all.df)
```

`md.pattern()` 函数的输出如下所示。每一行显示观测值的计数，以及一个1或0标志，指示该计数是否包含完全填充的值：

- 第一行表示，有1,920个观测值中，**性别**、**教育** 和 **年龄** 均不缺失
- 第二行显示，其余的 **80** 个观测值也有 **性别** 和 **教育** 的填充值，但 **年龄** 没有

检查 `md.pattern()` 函数非常重要，以查看其他变量中是否有足够多的非缺失值来进行缺失值插补：

```
> md.pattern(all.df)
             Gender Education Age
1920           1        1      1  0
 80             1        1      0  1
               0        0     80 80
> |
```

要开始插补过程，请调用 mice() 函数并将其分配给一个新的R对象：

```
imp <- mice(all.df,m=5,maxit=50, seed=1010,printFlag = TRUE)
```

- 在函数调用中，请确保提供一个随机种子值，以便如果您再次运行，可以复现相同的结果
- m=5 参数指定您最终会得到该变量的五个合理的插补值
- maxit=50 参数指定算法在收敛到解之前最多进行50次迭代，此值可根据所需精度向上或向下调整

运行 mice() 函数后，您将实时看到插补过程，因此运行时间可能会较长，具体取决于您指定的迭代次数。

完成后，您可以使用 head() 函数查看 Age（或其他值）的一些插补值：

```
head(imp$imp$Age)
```

```
> head(imp$imp$Age)
             1   2   3   4   5
38   29  35  20  33  27
49   25  23  30  32  21
79   25  23  25  25  25
99   31  30  22  26  24
157  24  30  24  23  22
180  29  22  26  28  29
```

要实际完成插补，您需要运行 complete() 函数并将结果分配给一个新的数据框。该版本的 complete() 函数将通过 "long" 参数将所有插补收集到所分配的数据框中：

```
all_imputed_df <- complete(imp, "long", include=TRUE)
```

在新数据框上运行 `table()` 函数，以计算所有插补（加上原始值）的缺失值计数。包含 NAs 的原始数据框由 `imp=0` 表示。

年龄有另外五个插补，标记为 imp 1-6：

```
table(all_imputed_df$imp,is.na(all_imputed_df$Age))
```

```
> table(all_imputed_df$imp,is.na(all_imputed_df$Age))

FALSE TRUE
0 1920 80
1 2000 0
2 2000 0
3 2000 0
4 2000 0
5 2000 0
```

为了观察插补的实际效果，我们将筛选新数据框中包含缺失值的原始ID之一（#216）。

然后，我们可以查看每个年龄插补中年龄值如何变化。插补值可能存在一些变化。在本示例中，其范围可从25起始值一直到34：

```
all_imputed_df %>% filter(.id == 216)
>all_imputed_df %>% filter(.id == 216)

.imp .id Age Gender Education
1 0 216 NA M Some College, 1 or More Years, No Degree
2 1 216 25 M Some College, 1 or More Years, No Degree
3 2 216 29 M Some College, 1 or More Years, No Degree
4 3 216 22 M Some College, 1 or More Years, No Degree
5 4 216 34 M Some College, 1 or More Years, No Degree
6 5 216 34 M Some College, 1 or More Years, No Degree
```

当然，这只是针对单个观测值。如果您查看 imp=1-6 的均值，它们都非常相似：

```
all_imputed_df %>% group_by(.imp) %>% summarize(MeanAge=mean(Age)
> all_imputed_df %>% group_by(.imp) %>% summarize (MeanAge=mean(Ag
# A tibble: 6 × 2
.imp MeanAge
<fctr> <dbl>
1 0 NA
2 1 27.5300
3 2 27.5780
4 3 27.5230
5 4 27.5855
6 5 27.5325
```

## 使用插补值运行回归分析

现在您已为年龄插补了数值，就可以运行线性回归等模型，而无需丢弃缺失值。

让我们尝试使用插补 #2 进行回归分析。

首先，提取插补 #2：

```
impute.2 <- subset(all_imputed_df, .imp=='2')
```

接下来，在控制台运行 summary() 函数，确保不再存在 NAs：

```
> summary(impute.2)
.imp .id Age Gender
0: 0 1 : 1 Min. :20.00 M:1000
1: 0 10 : 1 1st Qu.:23.00 F:1000
2:2000 100 : 1 Median :28.00
3: 0 1000 : 1 Mean :27.58
4: 0 1001 : 1 3rd Qu.:32.00
5: 0 1002 : 1 Max. :35.00
(Other):1994
Education
Regular High School Diploma :522
Bachelor's Degree :323
Some College, 1 or More Years, No Degree:295
9th Grade to 12th Grade, No Diploma :169
Master's Degree :150
Associate's Degree :140
(Other) :401
```

最后，运行回归：

```
lm(Age ~ Education + Gender,data=impute.2)
```

```
> lm(Age ~ Education + Gender,data=impute.2)

Call:
lm(formula = Age ~ Education + Gender, data = impute.2)

Coefficients:
(Intercept)
26.85358
EducationNursery School to 8th Grade
0.86901
Education9th Grade to 12th Grade, No Diploma
1.02452
EducationRegular High School Diploma
0.33593
EducationGED or Alternative Credential
1.26967
EducationSome College, Less than 1 Year
1.53750
EducationSome College, 1 or More Years, No Degree
0.69232
EducationAssociate's Degree
0.95965
EducationBachelor's Degree
0.82072
EducationMaster's Degree
0.84928
EducationProfessional School Degree
0.11744
EducationDoctorate Degree
0.87017
GenderF
-0.03037
```

