

超过成员的数量。这是因为成员进行了多次购买：

```
join3 <- sqldf("select * from member left outer join purchases_fi
str(join3)
nrow(join3)
```

```
#View the output from join 3 for columns 1, and 8-16 correspondin
View(join3[,c(1,8:16)])
```

`View()` 函数是浏览结果部分以尝试协调任何异常的便捷方式。在对 `join3` 数据框的选定列发出 `View` 命令后，我们可以看到 memberid 4 有多次购买。我们还可以快速查看哪些成员没有购买（成员 ID 7 和 8）。在成员没有购买的情况下，购买信息作为变量包含在内，但购买变量字段中填充了 NA：

| 成员ID | 级别 | 等级 | 年份 | 州 | 邮编 | 购买 | purch.f | 总金额 | 产品 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 4 | 93.2 | 2011 | 乔治亚州 | 49835 | NA | NA | NA | NA |
| 2 | 2 | 4 | 85.8 | 1999 | 内布拉斯加州 | 91960 | 3 | 3 | 21428.89 | C |
| 3 | 3 | 2 | 85.1 | 1998 | 麻萨诸塞州 | 45727 | 1 | 1 | 21493.89 | B |
| 4 | 4 | 1 | 90.1 | 1996 | 德克萨斯州 | 82942 | 2 | 2 | 20029.25 | C |
| 5 | 4 | 1 | 90.1 | 1996 | 德克萨斯州 | 82942 | 2 | 2 | 20153.52 | B |
| 6 | 5 | 1 | 86.7 | 2005 | 佛罗里达州 | 45727 | 2 | 2 | 20166.95 | B |
| 7 | 5 | 1 | 86.7 | 2005 | 佛罗里达州 | 45727 | 3 | 3 | 21016.57 | C |
| 8 | 6 | 1 | 81.8 | 2007 | 纽约州 | 22801 | 1 | 1 | 20394.55 | B |
| 9 | 7 | 3 | 85.1 | 2006 | 阿拉巴马州 | 49835 | NA | NA | NA | NA |
| 10 | 8 | 3 | 91.4 | 1997 | 犹他州 | 49835 | NA | NA | NA | NA |

这种连接称为一对多连接。在此示例中，只能有一条成员记录，但可以有多条购买记录。

## 识别有多次购买的成员

在新脚本中自行尝试此查询，以确定哪些成员有多次购买。应该有 89 个。在 SQL 查询术语中，多次购买子句在查询中由 `count(*) > 1` 指示：

```
dups <- sqldf("select member.memberid,count(*) from
member left outer join
purchases_filtered on
member.memberid=purchases_filtered.memberid2
group by member.memberid having count(*) > 1")
```

## 消除重复记录

在许多情况下，您可能只想保留连接产生的一条记录。假设我们只想为每个 memberid 保留一条记录，并且希望它是每个成员的最高 TotalPurchase。要消除重复项，我们可以先按 memberid 和 TotalPurchase 金额对数据框进行排序。然后我们将使用 rev 和 duplicated 函数仅保留每个 memberid 的最后一条购买记录。这样，原始 1,000 个成员各只剩下一条记录：

```
join3 <- join3[order(join3$memberid, join3$TotalAmount),]
View(join3)
dedup <- join3[!rev(duplicated(rev(join3$memberid))),]
nrow(join3)
nrow(dedup)
```

```
> nrow(join3)
[1] 1105
> nrow(dedup)
[1] 1000
```

## 探索医院数据集

探索性数据分析是在数据建模之前的一个初步步骤，您需要查看数据的所有特征，以了解数据分布、相关性、缺失值、异常值以及任何其他可能影响未来分析的因素。这是一个非常重要的步骤，如果认真执行，日后将为您节省大量时间。

对于以下示例，我们将读取纽约市医院出院数据集（住院患者出院（SPARCS 去标识化）：2012，n.d.）。本示例使用 `read.csv` 函数输入分隔文件，然后使用 `View` 函数以图形方式显示输出。然后使用 `str` 函数显示刚创建的 df 数据框的内容，最后，`summary()` 函数显示所有变量的所有相关统计信息。这些都是初次查看数据时的典型步骤：

```
df <-read.csv("C:/PracticalPredictiveAnalytics/Data/NYC Hospital str(df)
```

### str(df) 函数的输出

在创建新数据对象后，运行 `str()` 函数是一个重要的步骤。如果您在日志中查看 `str(df)` 函数的输出，会发现您刚刚运行的默认 `read.csv` 函数已将数据中的每个变量定义为数值型或因子型。目前保持原样也可以，但最终我们会希望更改这些变量的数据类型，因为 `Length.of.Stay` 不是因子，而是整数。或者，我们可以执行另一个指定所需精确数据类型的 `read.csv` 函数，指定 `ColClasses` 向量。我通常喜欢先查看一个小数据集样本，然后确定数据类型应该是什么，再重新输入数据。

关于 `read.csv` 语句需要注意的另一点是，缺失值通过 na.strings 选项指定为空值。

`str()` 函数在提示您哪些变量可以从分析中排除方面也很重要，即使在运行频率分布之前也是如此。

例如，APR.Severity.of.Illness. Description 和 Payment.Typology.1 变量只有一个级别，因此我们将从分析中排除它们，因为数据中将没有变异性：

```
'data.frame': 4052 obs. of 24 variables:
 $ Hospital.County : Factor w/ 5 levels "Bronx","Kings",...: 1 1 1 1 1 1 1 1 1 1 ...
 $ Facility.Name : Factor w/ 54 levels "Bellevue Hospital Center",...: 13 13 13 13 13 13 13 13 13 13 ...
 $ Age.Group : Factor w/ 5 levels "0 to 17","18 to 29",...: 4 4 4 3 4 5 3 4 4 4 ...
 $ Zip.Code...3.digits : Factor w/ 40 levels "100","101","103",...: 4 4 4 1 4 4 1 3 4 4 ...
 $ Gender : Factor w/ 2 levels "F","M": 1 2 2 2 2 2 2 1 2 2 ...
 $ Race : Factor w/ 6 levels "Asian","Black/African American",...: 1 4 4 2 2 2 2 2 2 2 ...
 $ Ethnicity : Factor w/ 3 levels "Not Span/Hispanic",...: 1 2 2 2 1 2 1 2 1 ...
 $ Length.of.Stay : Factor w/ 120 levels "1","10","100",...: 24 55 25 25 50 42 59 35 51 ...
 $ Admit.Day.of.Week : Factor w/ 7 levels "FRI","MON","SAT",...: 2 3 3 1 5 6 4 2 2 5 ...
 $ Patient.Disposition : Factor w/ 7 levels "Expired","Home or Self",...: 11 11 12 17 18 7 4 ...
 $ Discharge.Day.of.Week : Factor w/ 7 levels "FRI","MON","SAT",...: 3 7 6 6 5 6 2 1 1 ...
 $ CCS.Diagnosis.Description : Factor w/ 217 levels "ABDOMINAL HERNIA",...: 3 37 30 190 136 190 95 113 21 190 ...
 $ CCS.Procedure.Description : Factor w/ 192 levels "ABDOMINAL PARACENTESIS",...: 93 33 36 66 76 36 39 111 182 ...
 $ APR.MDR.Description : Factor w/ 260 levels "ABDOMINAL PAIN",...: 49 82 246 230 172 99 246 99 57 247 ...
 $ APR.MDC.Description : Factor w/ 20 levels "DRUGS/ALCOHOL/MENTAL HEALTH",...: 20 20 1 11 10 1 11 1 11 1 ...
 $ APR.Severity.of.Illness.Description : Factor w/ 1 level "Extreme": 1 1 1 1 1 1 1 1 1 1 ...
 $ APR.Risk.of.Mortality : Factor w/ 4 levels "Extreme","Major",...: 1 1 1 1 1 1 2 1 1 1 ...
 $ APR.Medical.Surgical.Description : Factor w/ 2 levels "Medical",Surgical: 1 1 2 1 1 2 1 1 2 2 ...
 $ APR.MDC.Code : Factor w/ 25 levels "01","03","04",...: 5 5 5 5 5 5 5 4 4 ...
 $ Payment.Typology.1 : Factor w/ 9 levels "Blue Cross/Blue Shield",...: 5 NA 5 5 5 5 5 5 4 4 ...
 $ Payment.Typology.2 : Factor w/ 9 levels "Blue Cross/Blue Shield",...: NA NA 4 4 4 4 4 NA NA ...
 $ Emergency.Department.Indicator : Factor w/ 2 levels "N","Y": 2 2 2 2 2 2 2 2 2 ...
```

### View 函数的输出

除了使用 head 和 tail 命令外，使用 View 命令快速查看数据也很有帮助。向上、向下、向左和向右滚动，检查具有代表性的值类型以及是否填充了数据。查看数据还可以让您了解数据可能如何排序或分组：

![](img/672c0cf8ba3c000a6529b117ed96945e_138_0.png)

### colnames 函数

运行 `colnames()` 函数是获取每个变量索引编号的便捷方法。在 R 函数中使用索引编号是引用变量的便捷且快速的方式。

但是，文件格式的更改可能会改变顺序，因此仅建议在交互式使用中采用此方法：

```
> colnames(df)
 [1] "Hospital.County"                  "Facility.Name"                    "Age.Group"                        "zip.code...3.digits"
 [5] "Gender"                           "Race"                             "Ethnicity"                        "Length.of.Stay"
 [9] "Admit.Day.of.week"                "Patient.Disposition"              "Discharge.Day.of.week"            "CCS.Diagnosis.Description"
[13] "CCS.Procedure.Description"        "APR.DRG.Description"              "APR.MDC.Description"              "APR.Severity.of.Illness.Description"
[17] "APR.Risk.of.Mortality"            "Emergency.Department.Indicator"   "Total.Charges"                    "Total.Costs"
[21] "Payment.Typology.3"
```

为了简化示例，我们将从分析中排除变量 12、13、16、19、20 和 21。

### summary 函数

`summary()` 函数是获取变量分布快照的便捷方式。对于数值变量，它将提供六个重要的分布统计量（均值、最小值、最大值、第一四分位数、第五四分位数和第三四分位数）。该函数将快速返回大量信息。对于大量变量，`summary()` 函数的输出可能会让人不知所措（而且不好看！）。

为了限制输出，我们将使用之前获得的列号，运行一个排除列 12、13、16、19、20 和 21 的 `summary()` 函数：

```
summary(df[,-c(12, 13, 16, 19, 20, 21)],maxsum=7)
```

或者，如果您不想使用索引编号而更喜欢使用变量名，可以使用以下代码获得相同的结果，该代码使用非号（!）来排除向量中指定的变量：

```
exclude_vars <- names(df) %in% c('CCS.Diagnosis.Description','CCS
```

```
summary(df[!exclude_vars],maxsum=7)
```

查看 `summary()` 函数的输出，并观察变量的分布。查找似乎代表性不足或过度的变量。医院国家的分布是否反映了五个

