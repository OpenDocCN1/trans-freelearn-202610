
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

Confusion Matrix and Statistics

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

Support vector machines, commonly known as **SVMs**, are another class of machine learning algorithm that are used to classify data into one or another category using a concept called **hyperplane**, which is used to demarcate a linear boundary between points.

For instance, given a set of black and white points on an x-y axis, we can find multiple lines that will separate them. The line, in this case, represents the function that delineates the category that each point belongs to. In the following image, lines H1 and H2 both separate the points accurately. In this case, how can we determine which one of H1 and H2 would be the optimal line?:

![](img/8ededb1e0b69e85687f6e488d1044b79_302_0.png)

Intuitively, we can say the line that is closest to the points - for instance, the vertical line H1 - might *not* be the optimal line to separate the points. Since the line is too close to the points, and so too specific to the points on the given dataset, a new point may be misclassified if it is even slightly off to the right or the left side of the line. In other words, the line is too sensitive to small changes in the data (which could be due to stochastic/deterministic noise, such as imperfections in the data).

On the other hand, the line H2 manages to separate the data whilst maintaining the maximum possible distance from the points closest to the line. Slight imperfections in the data are unlikely to affect the classification of the points to the extent line H1 may have done. This, in essence, describes the principle of the maximum margin of separation as shown in the image below.

![](img/8ededb1e0b69e85687f6e488d1044b79_303_0.png)

The points close to the line, also known as the hyperplane, are known as the 'support vectors' (hence the name). In the image, the points that lie on the dashed line are therefore the support vectors.

In the real world, however, not all points may be 'linearly separable'. SVMs leverage a concept known as the 'kernel trick'. In essence, points that might not be linearly separable can be projected or mapped onto a higher dimensional surface. For example, given a set of points on a 2D x-y space that are not linearly separable, it may be possible to separate them if we were to project the points on a 3-dimensional space as shown in the following image. The points colored in red were not separable by a 2D line, but when mapped to a 3-dimensional surface, they can be separated by a hyperplane as shown in the following image:

![](img/8ededb1e0b69e85687f6e488d1044b79_303_1.png)

There are several packages in R that let users leverage SVM, such as kernlab, e1071, klaR, and others. Here, we illustrate the use of SVM from the e1071 package, as shown as follow:

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

Confusion Matrix and Statistics

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

K-Means is one of the most popular unsupervised machine learning techniques that is used to create clusters, and so categorizes data.

An intuitive example could be posed as follows:

Say a university was offering a new course on American History and Asian History. The university maintains a 15:1 student-teacher ratio, so there is 1 teacher per 15 students. It has conducted a survey which contains a 10-point numeric score that was assigned by each student to their preference of studying American History or Asian History.

We can use the in-built K-Means algorithm in R to create 2 clusters and presumably, by the number of points in each cluster, it may be possible to get an estimate of the number of students who may sign up for each course. The code for the same is given as follows:

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

The following image could provide an intuitive estimate of the number of students who may sign up for each course (and thereby determine how many teachers may be required):

![](img/8ededb1e0b69e85687f6e488d1044b79_306_0.png)

There are several variations of the K-Means algorithm, but the standard and the most commonly used one is Lloyd's Algorithm. The algorithm steps are as follows:

- 1. Select k points at random from the dataset to represent the mid-points for k clusters (say, the *initial centroids*).
- 2. The distance from each of the other points to the selected k points (representing k clusters) is measured and assigned to the cluster that has the lowest distance from the point.
- 3. The cluster centers are recalculated as the mean of the points in the cluster.
- 4. The distance between the centroids and all the other points are again calculated as in Step 2 and new centroids are calculated as in Step 3. In this manner, Steps 2 and 3 are repeated until no new data is re-assigned.

Various *distance and similarity measures* exist for clustering, such as **Euclidean Distance** (straight-line distance), **Cosine Similarity** (Cosine of angles between vectors), **Hamming Distance** (generally used for categorical variables), **Mahalanobis Distance** (named after P.C. Mahalanobis; this measures the distance between a point and the mean of a distribution), and others.

Although the optimal number of clusters cannot always be unambiguously identified, there are various methods that attempt to find an estimate. In general, clusters can be measured by how close points within a cluster are to one another (within cluster variance, such as the sum of squares--WSS) and how far apart the clusters are (so higher distances between clusters would make the clusters more readily distinguishable). One such method that is used to determine the optimal number is known as the **elbow method**. The following chart illustrates the concept:

![](img/8ededb1e0b69e85687f6e488d1044b79_307_0.png)
