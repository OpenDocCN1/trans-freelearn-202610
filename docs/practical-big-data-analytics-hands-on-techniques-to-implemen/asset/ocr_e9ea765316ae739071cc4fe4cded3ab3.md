Reading from the top, the graph shows that that there are 500 cases of diabetes=neg in the dataset (out of a total of 768 records).

```
> sum(PimaIndiansDiabetes$diabetes=="neg")
[1] 500
```

Of the total number of records in the dataset (768) with value of glucose < 128, there were 485 records marked as negative. Of these, the model correctly predicted 391 cases as negative (Node Number 2, the first one on the left from the bottom).

For the records which had a glucose reading of > 128, there were 283 records marked as positive (Node Number 3, the node immediately below the topmost/root node). The model correctly classified 174 of these cases.

Another, more recent package for intuitive decision trees with comprehensive visual information is FFTrees (Fast and Frugal Decision Trees). The following example has been provided for informational purposes:

```
install.packages("FFTrees")
library(caret)
library(mlbench)
library(FFTrees)
set.seed(123)

data("PimaIndiansDiabetes")
diab<- PimaIndiansDiabetes
diab$diabetes<- 1 * (diab$diabetes=="pos")

train_ind<- createDataPartition(diab$diabetes,p=0.8,list=FALSE,times=1)

training_diab<- diab[train_ind,]
test_diab<- diab[-train_ind,]

diabetes.fft<- FFTrees(diabetes ~.,data = training_diab,data.test =
test_diab)
plot(diabetes.fft)

# The plot below illustrates the decision tree representing diabetes.fft using the FFTrees package.
```