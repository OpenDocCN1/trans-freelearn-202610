

```
eta = 0.1)

xgb_model <- xgb.train(data = diab_train_dmatrix,
param_diab, nrounds = 1000,
watchlist = list(train = diab_train_dmatrix, test = diab_test_dmatrix),
print_every_n = 10)

predicted <- predict(xgb_model, diab_test_dmatrix)
predicted <- predicted > 0.5

actual <- test_diab$diabetes == "pos"
confusionMatrix(actual,predicted)
```

## RESULT

混淆矩阵和统计量

| Prediction | FALSE | TRUE |
|---|---:|---:|
| FALSE | 80 | 17 |
| TRUE | 21 | 36 |

Accuracy : 0.7532
95% CI : (0.6774, 0.8191)
No Information Rate : 0.6558
P-Value [Acc > NIR] : 0.005956

Kappa : 0.463
Mcnemar's Test P-Value : 0.626496

Sensitivity : 0.7921
Specificity : 0.6792
PosPredValue : 0.8247
NegPredValue : 0.6316
Prevalence : 0.6558
Detection Rate : 0.5195
Detection Prevalence : 0.6299
Balanced Accuracy : 0.7357

'Positive' Class : FALSE

## Support vector machines

支持向量机

支持向量机，通常称为 **SVMs**，是另一类机器学习算法，用于使用称为 **hyperplane** 的概念将数据分类到一个或另一个类别，该概念用于在点之间划定线性边界。

例如，给定 x-y 轴上的黑白点集，我们可以找到多条线来将它们分开。在这种情况下，线代表了界定每个点所属类别的函数。在下面的图像中，线 H1 和 H2 都能准确地分离点。在这种情况下，我们如何确定 H1 和 H2 中哪一个是最佳线？：

![](img/8ededb1e0b69e85687f6e488d1044b79_302_0.png)

直观地说，我们可以说离点最近的线——例如垂直线 H1——可能*不是*分离点的最佳线。由于线离点太近，并且对给定数据集上的点过于具体，如果新点稍微偏离线的右侧或左侧，就可能被错误分类。换句话说，该线对数据的微小变化过于敏感，而这些变化可能是由于随机/确定性噪声引起的，例如数据中的不完美。

另一方面，线 H2 在分离数据的同时保持了与离线最近的点最大可能的距离。数据中的轻微不完美不太可能影响点的分类程度，就像线 H1 可能发生的那样。这本质上描述了图像下所示的最大间隔分离原理。

![](img/8ededb1e0b69e85687f6e488d1044b79_303_0.png)

靠近线的点，也称为超平面，被称为“支持向量”（因此得名）。在图像中，位于虚线上的点因此是支持向量。

然而，在现实世界中，并非所有点都可能是“线性可分”的。SVM 利用了称为“核技巧”的概念。本质上，可能无法线性分离的点可以投影或映射到更高维的表面。例如，给定 2D x-y 空间中一组无法线性分离的点，如果我们将点投影到三维空间中，如下图所示，则可能将它们分开。红色着色的点无法通过 2D 线分离，但在映射到三维表面后，可以通过如下图所示的超平面分离：

![](img/8ededb1e0b69e85687f6e488d1044b79_303_1.png)

R 中有多个包可让用户利用 SVM，例如 kernlab、e1071、klaR 等。在这里，我们说明了使用 e1071 包中的 SVM 的用法，如下所示：

```
library(mlbench)
library(caret)
library(e1071)
set.seed(123)

data("PimaIndiansDiabetes")
diab<- PimaIndiansDiabetes

train_ind<- createDataPartition(diab$diabetes,p=0.8,list=FALSE,times=1)

training_diab<- diab[train_ind,]
test_diab<- diab[-train_ind,]

svm_model<- svm(diabetes ~ ., data=training_diab)
plot(svm_model,training_diab, glucose ~ mass)

# The plot below illustrates the areas that are classified 'positive' and 'negative'
```

![](img/8ededb1e0b69e85687f6e488d1044b79_304_0.png)

```
# Creating and evaluating the Confusion Matrix for the SVM model

svm_predicted<- predict(svm_model,test_diab[,-ncol(test_diab)])
confusionMatrix(svm_predicted,test_diab$diabetes)
```

混淆矩阵和统计量

Reference

| Prediction | neg | pos |
|---|---:|---:|
| neg | 93 | 26 |
| pos | 7 | 27 |

Accuracy : 0.7843
95% CI : (0.7106, 0.8466)
No Information Rate : 0.6536
P-Value [Acc > NIR] : 0.0003018

Kappa : 0.4799
McNemar's Test P-Value : 0.0017280

Sensitivity : 0.9300
Specificity : 0.5094
PosPredValue : 0.7815
NegPredValue : 0.7941
Prevalence : 0.6536
Detection Rate : 0.6078
Detection Prevalence : 0.7778
Balanced Accuracy : 0.7197
'Positive' Class : neg

## The K-Means machine learning technique

K-Means 机器学习技术

K-Means 是最受欢迎的无监督机器学习技术之一，用于创建聚类，从而对数据进行分类。

一个直观的例子可以如下提出：

假设一所大学正在开设美国历史和亚洲历史的新课程。大学保持 15:1 的师生比例，因此每 15 名学生有 1 名教师。它进行了一项调查，其中包含每个学生对学习美国历史或亚洲历史的偏好所分配的 10 分制数值评分。

我们可以使用 R 内置的 K-Means 算法创建 2 个聚类，并且根据每个聚类中的点数，可能可以估计可能报名参加每门课程的学生人数。代码如下所示：

```
library(data.table)
library(ggplot2)
library()

historyData<- fread("~/Desktop/history.csv")
ggplot(historyData,aes(american_history,asian_history)) + geom_point() + geom_jitter()

historyCluster<- kmeans(historyData,2) # Create 2 clusters
historyData[,cluster:=as.factor(historyCluster$cluster)]
ggplot(historyData, aes(american_history,asian_history,color=cluster)) + geom_point() + geom_jitter()

# The image below shows the output of the ggplot command. Note that the effect of geom_jitter can be seen in the image below (the points are nudged so that overlapping points can be easily visible)
```

下面的图像可以直观地估计可能报名参加每门课程的学生人数（从而确定可能需要的教师人数）：

![](img/8ededb1e0b69e85687f6e488d1044b79_306_0.png)

K-Means 算法有几种变体，但标准且最常用的方法是 Lloyd 算法。算法步骤如下：

- 1. 从数据集中随机选择 k 个点作为 k 个聚类的中点（即 *初始质心*）。
- 2. 测量其他每个点到所选 k 个点（代表 k 个聚类）的距离，并将其分配给与该点距离最小的聚类。
- 3. 聚类中心重新计算为聚类中点的平均值。
- 4. 再次按步骤 2 计算质心与所有其他点之间的距离，并按步骤 3 计算新的质心。以这种方式，重复步骤 2 和 3，直到没有新的数据被重新分配。

存在多种用于聚类的 *距离和相似性度量*，例如 **Euclidean Distance**（直线距离）、**Cosine Similarity**（向量之间角度的余弦）、**Hamming Distance**（通常用于分类变量）、**Mahalanobis Distance**（以 P.C. Mahalanobis 命名；该度量点与分布均值之间的距离），等等。

尽管最优聚类数并不总是可以明确识别，但有各种方法试图找到估计值。一般来说，聚类可以通过聚类内点的接近程度来衡量（聚类内方差，如平方和之和--WSS）和聚类之间的距离（因此聚类之间的距离越大，聚类就越容易区分）。确定最优数量的一种方法称为 **elbow method**。下图说明了该概念：

![](img/8ededb1e0b69e85687f6e488d1044b79_307_0.png)

