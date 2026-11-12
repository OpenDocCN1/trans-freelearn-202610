

另一方面，CustomerID=2 只有一条记录，因为该会员仅存在了 5 个月，低于 cut=6 的截止值：

```
View(subset(SURV2, select = c(CustomerID,time0,period, Xsatisfaction
```

|    | CustomerID | time0 | period | Xsatisfaction | Xsatisfaction2 | Xtenure2 | Xmonthly.charges | Churn |
|---|---|---|---|---|---|---|---|---|
| 1174 | 1 | 0 | 0 | 1 | 3 | 6 | 233 | 0 |
| 2662 | 1 | 6 | 1 | 1 | 3 | 12 | 233 | 1 |
| 213 | 2 | 0 | 0 | 1 | 6 | 5 | 217 | 1 |
| 1307 | 3 | 0 | 0 | 4 | 2 | 6 | 269 | 0 |
| 2795 | 3 | 6 | 1 | 4 | 2 | 11 | 269 | 1 |
| 1266 | 4 | 0 | 0 | 1 | 2 | 6 | 168 | 0 |
| 2754 | 4 | 6 | 1 | 1 | 2 | 12 | 168 | 1 |
| 1268 | 6 | 0 | 0 | 5 | 3 | 6 | 164 | 0 |
| 2756 | 6 | 6 | 1 | 5 | 3 | 7 | 164 | 1 |
| 1372 | 7 | 0 | 0 | 1 | 2 | 6 | 89 | 1 |
| 681 | 8 | 0 | 0 | 1 | 5 | 6 | 175 | 0 |
| 2169 | 8 | 6 | 1 | 1 | 5 | 11 | 175 | 1 |
| 45 | 9 | 0 | 0 | 5 | 3 | 6 | 106 | 1 |

从分析角度来看，此时实际上并未发生任何变化。已创建两条信息完全相同的记录，只是其中一条记录显示的是第 6 期结束时的信息，另一条显示的是第 12 期结束时的信息。

我们可以通过运行一个简单的交叉表来查看第 1-12 个月与各期的对应关系：

```
> table(SURV2$period,SURV2$Xtenure2)
```

| | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 0 | 33 | 62 | 51 | 27 | 70 | 1245 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 0 | 0 | 0 | 0 | 136 | 170 | 185 | 230 | 210 | 225 |

## 调整记录以模拟干预

既然我们有了多条记录，就可以对第 6 个月后发生变化的新调查信息进行调整，并将其作为时间相关变量纳入模型。

请回顾，我们最初是通过将流失客户的满意度评分加 1（Xsatisfaction2）来模拟第二次调查数据的。这导致部分成员的满意度评分出现 6 分，这是不可能的。因此，我们将首先将评分为 6 的记录清理为 5 分：

```
#fix up some "6" satisfaction scores, that are not possible, and
SURV2$Xsatisfaction2 <- as.factor(ifelse(SURV2$Xsatisfaction2=="6
```

假设在第 5 个月，有一项针对初始调查中给出低满意度评分（1 分或 2 分）的成员开展的促销活动。这导致部分原本可能流失的成员的满意度评分提高了。

我们将通过将那些流失客户的满意度评分（Xsatisfaction2）从 1 分或 2 分提升至 3 分，来模拟对调查的积极回应。这是为了模拟满意度的略微提升。此操作仅针对 period=1（即调查后的期间）执行。从技术上讲，这是 survSplit() 函数在 cut 点之后分配的期间（第 1 期）。第 0 期是 cut 点之前的期间：

```
#an intervention increased low satisfaction scores to "Average".
SURV2$Xsatisfaction2 <- ifelse(SURV2$period==1 & SURV2$Churn==1 &
```

我们还将通过将这些第二次调查客户的 Churn 状态从 1（流失）改为 0（保留），来模拟其留存状态的变化，这表明促销活动产生了足够的效果，使他们没有离开：

```
#Simulate retaining these customers by changing their status to a
#change is detected when the new score is higher than the old score
SURV2$Churn2 <- ifelse(SURV2$period==1 & SURV2$Churn==1 &
       (as.integer(SURV2$Xsatisfaction2) > as.integer(
       0, SURV2$Churn)
SURV2$ChurnChanged <- ifelse(SURV2$Churn==SURV2$Churn2, "N", "Y")
```

现在，按 ID 对数据框进行排序并查看结果。拥有多个 ID 的记录已被算法拆分：

```
attach(SURV2)
tmp <- SURV2[order(CustomerID),]
str(tmp)
```

```
tmp2 <- subset(tmp, select = c(CustomerID,ChurnChanged,time0,period
```

```
View(tmp2)
```

|    | CustomerID | ChurnChanged | time0 | period | Xsatisfaction | Xsatisfaction2 | Churn | Churn2 |
|---|---|---|---|---|---|---|---|---|
| 1174 | 1 | N | 0 | 0 | 1 | 1 | 0 | 0 |
| 2662 | 1 | Y | 6 | 1 | 1 | 3 | 1 | 0 |
| 213 | 2 | N | 0 | 0 | 1 | 1 | 1 | 1 |
| 1307 | 3 | N | 0 | 0 | 4 | 4 | 0 | 0 |
| 2795 | 3 | N | 6 | 1 | 4 | 4 | 1 | 1 |
| 1266 | 4 | N | 0 | 0 | 1 | 1 | 0 | 0 |
| 2754 | 4 | Y | 6 | 1 | 1 | 3 | 1 | 0 |
| 1268 | 6 | N | 0 | 0 | 5 | 5 | 0 | 0 |
| 2756 | 6 | N | 6 | 1 | 5 | 5 | 1 | 1 |
| 1372 | 7 | N | 0 | 0 | 1 | 1 | 1 | 1 |
| 681 | 8 | N | 0 | 0 | 1 | 1 | 0 | 0 |
| 2169 | 8 | Y | 6 | 1 | 1 | 3 | 1 | 0 |
| 45 | 9 | N | 0 | 0 | 5 | 5 | 1 | 1 |

请注意，任何任职时间超过 6 个月的客户都会添加一条额外记录。这条额外记录反映了客户的最新状态，并更新了发生变化的变量值。

在此示例中，我们在 6 个月处执行了一次切分。数据还包含两个新变量：`time0`，表示起始期间；以及一个我们命名为 period 的事件变量，每当某个基于时间的变量发生变化时，它就会按顺序递增。我们仅展示了一个基于时间变量发生一次变化的示例。对于更长的观察期，可能会出现更多变化。

## 运行基于时间的模型

既然我们已重新格式化数据，现在可以运行第二个基于时间的模型了。

`CoxModel.2` 变量的创建方式与 `CoxModel.1` 类似，但使用 Xsatisfaction2 替代了 Xsatisfaction，以反映追踪研究中满意度的变化。新的 time0 变量也替换了原始的任期变量，Churn2 替换了原始的流失响应变量：

```
CoxModel.2 <- coxph(Surv(time0,Xtenure2, Churn2) ~
 Xeducation + Xgender + Xsatisfaction2 + Xservice.calls + Xpurch.last.month + Xmonthly.charges,
 data=SURV2)
```

避免覆盖现有变量名。请在现有数据框中创建新变量，或者保留原有数据框并新建一个数据框。

运行模型后，可以使用 `stargazer` 命令（或 `summary(CoxModel.2)`）来查看模型输出。

性别、满意度和月费后面的星号表明它们是模型中最显著的变量。

.13 的 R-square 不是标准线性回归类型的度量，而是我们之前讨论过的伪 R-square 度量，它是 0 到 1 之间的一个数值，用于衡量拟合模型与零模型之间的差异。如果你想了解针对 Cox 回归计算此特定伪 R-square 的方法，可以运行以下命令：

```
getS3method("summary", "coxph")
```

然而，它不如原始 `CoxModel.1` 高。这可能是由于对数据进行了人为修改。无论如何，依赖单一指标并非良好做法，我们将很快看到该模型如何改善留存率。

```
library(stargazer)
stargazer(CoxModel.2,single.row=TRUE,type="text")
```

```
> stargazer(CoxModel.2,single.row=TRUE,type="text")
```

| | time0 |
|---|---|
| XeducationDoctorate Degree | -0.160 (0.210) |
| XeducationMaster's Degree | -0.200 (0.140) |
| XgenderM | 0.091 (0.120) |
| Xsatisfaction22 | 0.160 (0.200) |
| Xsatisfaction23 | -0.600*** (0.200) |
| Xsatisfaction24 | 0.520*** (0.180) |
| Xsatisfaction25 | 0.580*** (0.180) |
| Xservice.calls | 0.013 (0.044) |
| Xpurch.last.month | -0.007 (0.029) |
| Xmonthly.charges | 0.009*** (0.001) |
| Observations | 2,644 |
| R2 | 0.130 |
| Max. Possible R2 | 0.900 |
| Log Likelihood | -2,869.000 |
| Wald Test | 370.000*** (df = 10) |
| LR Test | 380.000*** (df = 10) |
| Score (Logrank) Test | 399.000*** (df = 10) |
| 注： | *p<0.1; **p<0.05; ***p<0.01 |

## 让我们先生成另一条生存曲线：

```
par(mfrow=c(1,1))
autoplot(survfit(CoxModel.1), surv.linetype = 'dashed', surv.color
conf.int.fill = 'dodgerblue3', conf.int.alpha = 0.5, censor
dev.copy(jpeg,'Ch5 - time based variable 1.jpg');
dev.off()

autoplot(survfit(CoxModel.2), surv.linetype = 'dashed', surv.color
conf.int.fill = 'orange', conf.int.alpha = 0.5, censor =
dev.copy(jpeg,'Ch5 - time based variable 2.jpg');
dev.off()
savehistory (file="Ch5 time based variable.txt")
```

以下是原始图（左侧）与基于时间的生存图并排显示：

