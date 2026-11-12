

调用：
survdiff(formula = SurvObj ~ Xservice.calls, data = ChurnStudy)

  N Observed Expected (O-E)^2/E (O-E)^2/V
Xservice.calls=0 1103 507 542.87 2.371 10.322
Xservice.calls=1 222 154 116.77 11.873 16.992
Xservice.calls=2 58 23 32.72 2.888 3.711
Xservice.calls=3 51 33 28.43 0.734 0.923
Xservice.calls=4 47 31 25.95 0.985 1.215
Xservice.calls=5 7 3 4.26 0.373 0.448

卡方 = 23.3，自由度 = 5，p = 0.000301

最后两列给出卡方值，可以看到，大多数较高的值对应较少的服务呼叫次数。Xservice.calls 1 和 2 之间似乎存在一个断点。因此，我们可以将其视为自然断点。

或者，我们可以制定一个假设，即低服务呼叫和高服务呼叫之间存在差异；我们可以创建一个新的二元变量，仅用于指示是否有任何服务呼叫。

然后我们可以绘制结果，并再次使用 log-rank 检验来检验差异：

```
ChurnStudy$called.binary <- as.factor(ifelse(ChurnStudy$Xservice.calls=
survdiff(SurvObj ~ ChurnStudy$called.binary, data = ChurnStudy)
plotsurv(ChurnStudy$called.binary,c(1:2),c('none'))
title(main = "5 KM Curve Called")
dev.copy(jpeg,'Ch5 - 5 KM Curve Called.jpg'); dev.off()

survdiff(SurvObj ~ ChurnStudy$called.binary, data = ChurnStudy)
```

此处指定的 plotsurv 调用生成以下图表：

## 5 KM 曲线：已呼叫

![](img/672c0cf8ba3c000a6529b117ed96945e_312_0.png)

## 检验呼叫客户与未呼叫客户之间的差异

调用 survdiff() 函数生成卡方检验：

```
> survdiff(SurvObj ~ ChurnStudy$called.binary, data = ChurnStudy)
Call:
survdiff(formula = SurvObj ~ ChurnStudy$called.binary, data = ChurnStudy)

                    N Observed Expected (O-E)^2/E (O-E)^2/V
ChurnStudy$called.binary=CALLED   385    244     208     6.18
ChurnStudy$called.binary=NONE    1103    507     543     2.37

Chisq= 10.3  on 1 degrees of freedom, p= 0.00131
```

这显示了客户是否呼叫之间存在明显的断点，并表明客户呼叫可能需要干预计划，以便解决任何潜在的流失问题。

## Cox 回归建模

KM 检验在许多情况下都可以令人满意，尤其是在初步分析期间；然而，KM 检验是非参数的，通常不如参数等价方法功效强大。Cox 回归将生存分析扩展到参数回归类型框架下，从而假设更大的功效。如果有几个自变量需要纳入模型，并且其中一些是连续的，那么执行 Cox 比例风险建模比 KM 更有优势。

### 我们的第一个模型

Cox 建模也始于创建生存对象，就像我们在之前的示例中所做的那样。除此之外，Cox 模型与标准回归模型非常相似，响应变量指定在 ~ 的左侧，自变量指定在右侧。

在 Cox 回归建模中，我们使用 coxph() 函数而不是 surv() 函数来指定因变量。可以直接在公式中完成，也可以将其分配给新变量并将新变量指定在 ~ 的左侧。

请回顾，在定义我们最初的生存对象时，我们将 Xtenure2 定义为时间变量，将 Churn 定义为感兴趣的结局。在我们的示例中，我们将指定 coxph(Surv(Xtenure2, Churn) 作为预测模型的左侧。

继续进行本练习的下一部分，运行以下代码，该代码将运行一个 Cox 回归模型，使用教育、性别、客户满意度、服务呼叫次数、月费和上月购买次数作为自变量来预测流失：

```
clearhistory()
#start CoxModel.1====
rm(CoxModel.1)
```

```
ChurnStudy$SurvObj <- with(ChurnStudy, Surv(Xtenure2, Churn == 1))
CoxModel.1 <- coxph(Surv(Xtenure2, Churn) ~
    Xeducation + Xgender + Xsatisfaction + Xservice.calls +
    Xpurch.last.month + Xmonthly.charges,
    data=ChurnStudy)
```

要查看结果，我们将使用 stargazer 库：

```
library(stargazer)
stargazer(CoxModel.1,single.row=TRUE,multicolumn=TRUE,font.size='
no.space = TRUE,column.separate = c(1,2,3),
out=c("CoxModel.html"),type='html')
browseURL("CoxModel.html")
```

browseURL 命令将在浏览器中显示从 stargazer 写入的输出：

| 因变量： | |
|---|---|
| | Xtenure2 |
| XeducationDoctorate Degree | -0.007 (0.157) |
| XeducationMaster's Degree | -0.041 (0.103) |
| XgenderM | 0.281*** (0.095) |
| Xsatisfaction2 | -0.003 (0.101) |
| Xsatisfaction3 | -0.231* (0.124) |
| Xsatisfaction4 | -0.250** (0.111) |
| Xsatisfaction5 | -0.188 (0.125) |
| Xservice.calls | 0.030 (0.034) |
| Xpurch.last.month | -0.016 (0.024) |
| Xmonthly.charges | 0.008*** (0.0004) |
| Observations | 1,488 |
| R² | 0.298 |
| Max. Possible R² | 0.999 |
| Log Likelihood | -4,611.885 |
| Wald Test | 494.750*** (df = 10) |
| LR Test | 526.196*** (df = 10) |
| Score (Logrank) Test | 551.558*** (df = 10) |
| 注： | * p<0.1; ** p<0.05; *** p<0.01 |

或者，你可以运行 `summary(CoxModel.1)` 命令在控制台中打印更详细的版本：

```
> summary(CoxModel.1)
Call:
coxph(formula = Surv(Xtenure2, Churn) ~ Xeducation + Xgender +
              Xsatisfaction + Xservice.calls + Xpurch.last.month + Xmonthly.charges,
          data = ChurnStudy)

n= 1488, number of events= 751

             coef exp(coef) se(coef)
XeducationDoctorate Degree -0.0067422  0.9932804 0.1568365 -0.04
XeducationMaster's Degree -0.0411774  0.9596589 0.1031115 -0.39
XgenderM                   0.2809885  1.3244384 0.0954112  2.94
Xsatisfaction2            -0.0031029  0.9969019 0.1010203 -0.03
Xsatisfaction3            -0.2305492  0.7940974 0.1240880 -1.85
Xsatisfaction4            -0.2503191  0.7785523 0.1105464 -2.26
Xsatisfaction5            -0.1879801  0.8286312 0.1247098 -1.50
Xservice.calls             0.0303329  1.0307976 0.0336934  0.90
Xpurch.last.month         -0.0160869  0.9840418 0.0243041 -0.66
Xmonthly.charges           0.0082750  1.0083093 0.0003928 21.06
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

## 检查 Cox 回归输出

由于我们本质上是在运行一个改进的逻辑回归，Cox 模型中的系数始终以对数形式表示。要将其转换为似然比，需要取指数。这也是摘要输出的一部分。

首先，查看列在 `exp(coef)` 列下的指数化系数的值。任何显著大于 1 的系数表明客户更有可能流失而非不流失。另一方面，`exp(coef)` < 1 表明该变量流失的可能性较低。与 1 的差异幅度将指示某一方向的更高概率。

模型结果表明，`gender (males)`、`satisfaction` 和 `monthly charges` 是显著变量。

男性以及月费较高的客户流失倾向更高，这由其高于 1 且 p 值较低的可能性得分所表明。

满意度得分为 4 的客户流失倾向较低，这由低于 1 且 p 值较低的可能性得分所表明。尽管满意度得分为 5 的客户未被发现显著，但将其也纳入是有意义的，因为他们的系数表明他们可能会留下，并且他们的 p 值相当低（但在 .05 水平上不显著）。单独提取这些客户并查看是否有其他因素导致其中一些人流失也会很有趣。我们指出模型中可能需要交互项。

## 比例风险检验

运行 Cox 回归后，需要进行一些后续检验。Cox 回归需要检验的一个假设是，事件的风险仅取决于变量，而不取决于时间。如果是这样，时间就不能被视为自变量。因此，我们需要检验所谓的比例风险。这可以使用 cox.zph 函数来完成。

运行以下代码部分，该代码列在标签 #coxproptext=== 下：

它将执行以下操作：
- 将比例风险检验的输出分配给 `temp` 对象
- 将检验结果打印到控制台
- 遍历 `temp` 对象中包含的所有模型变量并绘制比例风险：

```
#coxproptext===
#test for proportional hazards
temp <- cox.zph(CoxModel.1, transform = 'log')
print(temp)
par(mfrow=c(2,5))
for (i in 1:10) {
plot(temp[i])
}
```

