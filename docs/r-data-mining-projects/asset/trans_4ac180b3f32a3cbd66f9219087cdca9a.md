

Min.Guarantee.Cost, family = binomial(logit), data = train)
偏离残差：
Min 1Q Median 3Q Max
-2.1596 -0.4849 -0.4096 -0.3401 3.8760
系数：
Estimate Std. Error z value Pr(>|z|)
(Intercept) -6.734e-01 1.250e+00 -0.539 0.58995
Critic.Ratings 1.372e-01 1.119e-02 12.267 < 2e-16 ***
Acq.Cost -7.998e-06 1.867e-06 -4.284 1.83e-05 ***
CurrentAuctionAveragePrice -1.460e-05 1.360e-06 -10.731 < 2e-16 ***
Brush.Size1 -1.378e+00 1.246e+00 -1.107 0.26840
Brush.Size2 -1.684e+00 1.246e+00 -1.352 0.17647
Brush.Size3 -1.128e+00 1.252e+00 -0.902 0.36730
Brush.SizeNULL 1.599e+00 1.246e+00 1.283 0.19946
CollectorsAverageprice -2.420e-05 4.677e-06 -5.175 2.28e-07 ***
Is.It.Online.Sale1 -1.710e-01 9.621e-02 -1.777 0.07552 .
Min.Guarantee.Cost 9.830e-06 3.329e-06 2.953 0.00314 **
---
Signif. codes: 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
(Dispersion parameter for binomial family taken to be 1)
Null deviance: 40601 on 54500 degrees of freedom
Residual deviance: 34872 on 54490 degrees of freedom
AIC: 34894
Number of Fisher Scoring iterations: 5

为检查自变量之间的多重共线性，使用了 VIF 衡量。针对基于逐步 AIC 的最终模型计算的 VIF 如下：

```
> library(MASS);library(plyr);library(car)
> 
> vif(fit_step)
GVIF Df GVIF^(1/(2*Df))
Critic.Ratings 1.204411 1 1.097457
Acq.Cost 2.582229 1 1.606932
CurrentAuctionAveragePrice 2.527691 1 1.589871
Brush.Size 1.094967 4 1.011405
CollectorsAverageprice 1.084788 1 1.041532
Is.It.Online.Sale 1.007080 1 1.003534
Min.Guarantee.Cost 1.163249 1 1.078540
```

使用响应选项计算训练数据集的概率分数，该概率分数可用于在 AUC 曲线上显示分类结果，也称为接收器操作特征 ROC 曲线：

```
> train$prob=predict(fit_step,type=c("response"))
> library(pROC)
> g <- roc(IsGood.Purchase ~ prob, data = train)
> g
```

分类表的阈值为 0.50（50%）概率。如果有人想改变正确分类对象的百分比，则可以通过对概率分数设置过滤器来实现。以下代码用于使用不同的概率值创建混淆矩阵。从之前的分类表可以得出结论，90% 的人被正确分类。

## Cubic regression

三次回归是回归的另一种形式，其中线性回归模型中的参数增加到一到两个级别的多项式计算。使用 Cars93_1.csv 数据集，让我们了解三次回归：

```
> fit.6<-lm(MPG.Overall~ I(Price)^3+I(Horsepower)^3+I(RPM)^3+
+ Wheelbase+Width+Turn.circle, data=Cars93_1[-c(28,42,39,59,60,77),])
> summary(fit.6)
Call:
lm(formula = MPG.Overall ~ I(Price)^3 + I(Horsepower)^3 + I(RPM)^3 +
Wheelbase + Width + Turn.circle, data = Cars93_1[-c(28, 42,
39, 59, 60, 77), ])
Residuals:
Min 1Q Median 3Q Max
-5.279 -1.901 -0.006 1.590 8.433
Coefficients:
Estimate Std. Error t value Pr(>|t|)
(Intercept) 57.078025 12.349300 4.622 1.44e-05 ***
I(Price) -0.108436 0.065659 -1.652 0.1026
I(Horsepower) -0.024621 0.015102 -1.630 0.1070
I(RPM) 0.001122 0.000727 1.543 0.1268
Wheelbase -0.201836 0.079948 -2.525 0.0136 *
Width -0.104108 0.198396 -0.525 0.6012
Turn.circle -0.095739 0.158298 -0.605 0.5470
---
Signif. codes: 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
Residual standard error: 2.609 on 80 degrees of freedom
Multiple R-squared: 0.6974, Adjusted R-squared: 0.6747
F-statistic: 30.73 on 6 and 80 DF, p-value: < 2.2e-16
> vif(fit.6)
I(Price) I(Horsepower) I(RPM) Wheelbase Width Turn.circle
4.121923 7.048971 2.418494 3.701812 7.054405 3.284228
> coefficients(fit.6)
(Intercept) I(Price) I(Horsepower) I(RPM) Wheelbase Width
57.07802478 -0.10843594 -0.02462074 0.00112168 -0.20183606 -0.10410833
Turn.circle
-0.09573848
```

在其他所有参数保持不变的情况下，自变量的一个单位变化会使因变量产生乘性变化，而乘法因子是贝塔系数的立方。

## Penalized regression

当回归模型中包含大量预测变量或高度相关的预测变量或两者兼有时的最大似然估计问题出现了，其无法在回归问题中提供更高的准确性，从而导致数据挖掘中引入惩罚回归。最大似然的特性由于高变异性和不适当的解释而无法满足回归程序。为解决该问题，最相关的子集选择出现了。然而，子集选择程序有一些缺点。为再次解决该问题，可以引入一种新方法，即众所周知的惩罚最大似然估计方法。我们将讨论两种不同的惩罚回归变体：

- **Ridge regression:** 岭回归被称为 L2 二次惩罚，其方程可表示如下：

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_147_0.png)

与最大似然估计相比，L1 和 L2 估计方法将贝塔系数收缩至零。为了避免多重共线性问题和预测变量数量较多或两者兼有，通常在观测值较少的情况下，收缩方法可降低过拟合的贝塔系数数值。

以 `Cars93_1.csv` 数据集为例，我们可以测试模型及其结果。lambda 参数基本上在对数似然函数的惩罚和拟合之间取得平衡。lambda 值的选择对模型至关重要；如果其为较小值，则模型可能对数据过拟合，高方差将变得明显，而如果选择的 lambda 值为很大值，则可能导致有偏结果。

```
```r
> #installing the library
> library(glmnet)
```
```

```
> #removing the missing values from the dataset
> Cars93_2<-na.omit(Cars93_1)
> #independent variables matrix
> x<-as.matrix(Cars93_2[,-1])
> #dependent variale matrix
> y<-as.matrix(Cars93_2[,1])
> #fitting the regression model
> mod<-glmnet(x,y,family = "gaussian",alpha = 0,lambda = 0.001)
> #summary of the model
> summary(mod)
Length Class Mode
a0 1 -none- numeric
beta 13 dgCMatrix S4
df 1 -none- numeric
dim 2 -none- numeric
lambda 1 -none- numeric
dev.ratio 1 -none- numeric
nulldev 1 -none- numeric
npasses 1 -none- numeric
jerr 1 -none- numeric
offset 1 -none- logical
call 6 -none- call
nobs 1 -none- numeric
#Making predictions
pred<-predict(mod,x,type = "link")
#estimating the error for the model.
mean((y-pred)^2)
> #Making predictions
> pred<-predict(mod,x,type = "link")
> #estimating the error for the model.
> mean((y-pred)^2)
[1] 6.663406
```

鉴于前述背景，回归模型能够显示出 6.66% 的误差，这意味着模型给出 93.34% 的准确率。这不是最终迭代。模型需要测试几次，并且需要使用袋外多个子样本来得出模型的最终准确性。

- **Least Absolute Shrinkage Operator (LASSO)**: LASSO 回归被称为 L1 绝对值惩罚（Tibshirani, 1997）。LASSO 回归的方程可表示如下：

```
f(β) = Σ_{j=1}^{n} |β|
```

以 Cars93_1.csv 数据集为例，我们可以测试模型及其结果。lambda 参数基本上在对数似然函数的惩罚和拟合之间取得平衡。lambda 值的选择对模型至关重要；如果其为较小值，则模型可能对数据过拟合，高方差将变得明显，而如果选择的 lambda 值为很大值，则可能导致有偏结果。让我们尝试实现模型并验证结果：

```
> #installing the library
> library(lars)
> #removing the missing values from the dataset
> Cars93_2<-na.omit(Cars93_1)
> #independent variables matrix
> x<-as.matrix(Cars93_2[,-1])
> #dependent variale matrix
> y<-as.matrix(Cars93_2[,1])
> #fitting the LASSO regression model
> model<-lars(x,y,type = "lasso")
> #summary of the model
> summary(model)
LARS/LASSO
Call: lars(x = x, y = y, type = "lasso")
Df Rss Cp
0 1 2215.17 195.6822
1 2 1138.71 63.7148
2 3 786.48 21.8784
3 4 724.26 16.1356
4 5 699.39 15.0400
5 6 692.49 16.1821
6 7 675.16 16.0246
7 8 634.59 12.9762
8 9 623.74 13.6260
9 8 617.24 10.8164
10 9 592.76 9.7695
11 10 587.43 11.1064
12 11 551.46 8.6302
13 12 548.22 10.2275
14 13 547.85 12.1805
15 14 546.40 14.0000
```

