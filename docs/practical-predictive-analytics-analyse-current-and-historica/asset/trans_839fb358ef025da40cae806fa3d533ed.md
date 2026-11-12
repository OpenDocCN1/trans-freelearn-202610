

![](img/672c0cf8ba3c000a6529b117ed96945e_339_0.png)

![](img/672c0cf8ba3c000a6529b117ed96945e_339_1.png)

## 模型比较

尽管生存曲线相似，但我们可以看到，在12个月结束时，56%的客户被保留下来，而原来的比例为27%。我们可以将此归因于在第6个月进行的干预。

使用`summary(survfit)`函数来比较模型：

| 时间 | 风险数 | 事件数 | 生存率 | 标准误 | 95% CI下限 | 95% CI上限 | 时间 | 风险数 | 事件数 | 生存率 | 标准误 | 95% CI下限 | 95% CI上限 |
|------|--------|---------|----------|---------|--------------|--------------|------|--------|---------|----------|---------|--------------|--------------|
| 1 | 1488 | ... | 0.994 | 0.00157 | 0.991 | 0.997 | 1 | 1488 | ... | 0.993 | 0.00185 | 0.989 | 0.996 |
| 2 | 1455 | ... | 0.973 | 0.00359 | 0.966 | 0.980 | 2 | 1455 | ... | 0.967 | 0.00404 | 0.960 | 0.975 |
| 3 | 1393 | ... | 0.958 | 0.00461 | 0.949 | 0.967 | 3 | 1393 | ... | 0.950 | 0.00505 | 0.940 | 0.960 |
| 4 | 1342 | ... | 0.950 | 0.00518 | 0.940 | 0.960 | 4 | 1342 | ... | 0.940 | 0.00559 | 0.929 | 0.951 |
| 5 | 1315 | 39 | 0.932 | 0.00624 | 0.920 | 0.945 | 5 | 1315 | 39 | 0.919 | 0.00655 | 0.906 | 0.932 |
| 6 | 1245 | 42 | 0.913 | 0.00736 | 0.898 | 0.927 | 6 | 1245 | 42 | 0.896 | 0.00752 | 0.881 | 0.911 |
| 7 | 1156 | 24 | 0.898 | 0.00801 | 0.883 | 0.914 | 7 | 1156 | 60 | 0.861 | 0.00884 | 0.844 | 0.879 |
| 8 | 1020 | 32 | 0.877 | 0.00902 | 0.859 | 0.895 | 8 | 1020 | 68 | 0.818 | 0.01033 | 0.798 | 0.838 |
| 9 | 850 | 40 | 0.846 | 0.01052 | 0.825 | 0.866 | 9 | 850 | 89 | 0.754 | 0.01233 | 0.731 | 0.779 |
| 10 | 665 | 51 | 0.797 | 0.01293 | 0.772 | 0.822 | 10 | 665 | 115 | 0.657 | 0.01500 | 0.628 | 0.687 |
| 11 | 435 | 54 | 0.721 | 0.01688 | 0.688 | 0.755 | 11 | 435 | 105 | 0.536 | 0.01789 | 0.502 | 0.572 |
| 12 | 225 | 55 | 0.569 | 0.02518 | 0.522 | 0.621 | 12 | 225 | 112 | 0.278 | 0.02226 | 0.237 | 0.325 |

## 变量选择

我们刚才使用的模型变量数量有限，因此在处理大量变量时，机械式变量选择方法并不十分适用。我们能够通过回归模型找出重要的变量。然而，对于变量数量庞大的模型，我们可以使用`glmulti`包来进行变量选择。

对于生成的流失示例，我们的变量数量较少，因此演示变量选择变得容易，也不太耗时。

在以下代码中，我们将最佳回归中包含的项的最大数量设置为10，以限制执行穷举搜索所需的计算时间。我们还将使用遗传算法选项（`method = "g"`），在处理较大数据集时速度可能快得多，因为它只考虑所有组合中的最佳子集。

如果您希望执行穷举搜索，请使用`method = "h"`。但是，需要提前警告，这可能会占用您的机器很长时间：

- 运行以下代码以开始变量选择过程：

```
library(glmulti)
glmulti.coxph.out <-
  glmulti(Surv(Xtenure2, Churn) ~ Xeducation + Xgender +
    Xsatisfaction + Xservice.calls +
    Xpurch.last.month + Xmonthly.charges, data =
    ChurnStudy,
    maxsize=10,
    level = 2,
    method = "g",
    crit = "aic",
    confsetsize = 5,
    plotty = T, report = F,
    fitfunction = "coxph")
```

- 当算法完成时，您将在控制台中收到以下消息。它应该在5分钟或更短时间内完成：

```
TASK: Genetic algorithm in the candidate set.
Initialization...
Algorithm started...
Improvements in best and average IC have been below specified goals.
Algorithm is declared to have converged.
Completed.
```

- 打印输出对象的摘要。摘要包括候选集中找到的最佳模型。请注意，最佳模型不包含教育变量：

```
> print(glmulti.coxph.out)
glmulti.analysis
Method: g / Fitting: coxph / IC used: aic
Level: 2 / Marginality: FALSE
From 5 models:
Best IC: 9214.89853563887
Best model:
[1] "Surv(Xtenure2, Churn) ~ 1 + Xgender + Xsatisfaction + Xservice.calls + "
[2] " Xmonthly.charges + Xpurch.last.month:Xservice.calls + Xmonthly.charges:Xservice.calls + "
[3] " Xgender:Xmonthly.charges + Xsatisfaction:Xmonthly.charges"
Evidence weight: 0.257101917192717
Worst IC: 9216.1617086134
5 models within 2 IC units.
4 models to reach 95% of evidence weight.
Convergence after 120 generations.
Time elapsed: 55.2994699478149 minutes.
```

- 由于我们指定模型中应考虑交互作用，算法已找到几个交互作用，以便进一步改进模型：

```
Xpurch.last.month:Xservice.calls
Xmonthly.charges:Xservice.calls
Xmonthly.charges:Xpurch.last.month
Xgender:Xpurch.last.month
Xsatisfaction:Xpurch.last.month
Xsatisfaction:Xmonthly.charges
```

- 其中两个重要的交互作用是：性别与上月购买次数的交互作用，以及满意度与月度费用的交互作用。这两个交互作用均在最佳模型中指定。

## 合并交互项

要将这些交互项纳入未来的模型，我们可以直接在模型公式的右侧指定它们。例如，此规范将服务呼叫，以及服务呼叫与上月购买次数之间的交互作用作为模型项：

```
CoxModel.2 <- coxph(Surv(time0,Xtenure2, Churn2) ~
Xeducation + Xgender + Xsatisfaction2 + Xservice.calls +
Xpurch.last.month + Xmonthly.charges,
data=SURV2)
```

### 显示公式子列表

如果您不想输入所有项，可以显示一个公式子列表，它将列出前5个模型的所有公式。这有助于剪切和粘贴：

```
> glmulti.coxph.out@formulas
[[1]]
Surv(Xtenure2, Churn) ~ 1 + Xgender + Xsatisfaction + Xservice.calls + Xmonthly.charges + Xpurch.last.month:Xservice.calls + Xmonthly.charges:Xservice.calls + Xgender:Xmonthly.charges + Xsatisfaction:Xmonthly.charges
<environment: 0x0000000034599328>

[[2]]
Surv(Xtenure2, Churn) ~ 1 + Xgender + Xsatisfaction + Xservice.calls + Xmonthly.charges + Xpurch.last.month:Xservice.calls + Xmonthly.charges:Xpurch.last.month + Xgender:Xmonthly.charges + Xsatisfaction:Xmonthly.charges
<environment: 0x0000000034599328>

[[3]]
Surv(Xtenure2, Churn) ~ 1 + Xgender + Xsatisfaction + Xservice.calls + Xmonthly.charges + Xpurch.last.month:Xservice.calls + Xmonthly.charges:Xpurch.last.month + Xsatisfaction:Xmonthly.charges
<environment: 0x0000000034599328>

[[4]]
Surv(Xtenure2, Churn) ~ 1 + Xgender + Xsatisfaction + Xservice.calls + Xpurch.last.month + Xmonthly.charges + Xpurch.last.month:Xservice.calls + Xmonthly.charges:Xservice.calls + Xgender:Xmonthly.charges + Xsatisfaction:Xmonthly.charges
<environment: 0x0000000034599328>

[[5]]
Surv(Xtenure2, Churn) ~ 1 + Xgender + Xsatisfaction + Xservice.calls + Xpurch.last.month + Xmonthly.charges + Xpurch.last.month:Xservice.calls + Xmonthly.charges:Xservice.calls + Xmonthly.charges:Xpurch.last.month + Xgender:Xmonthly.charges + Xsatisfaction:Xmonthly.charges
<environment: 0x0000000034599328>
```

## 比较候选模型中的AIC

如前几章所述，AIC是一个可帮助您选择模型的度量指标。`plotty='T'`选项还生成了所找到的顶级模型的AIC图。在函数调用中，我们表明希望查看最多五个模型。

根据该包作者的说法，一个合理的经验法则是考虑低于水平红线的模型。

根据该定义，将排除模型4和5，保留模型1、2和3供考虑。

以下是前五个模型的AIC图：

