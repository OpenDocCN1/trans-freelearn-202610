# The Gradient Boosting Machines R example

In R, the gbm package can be used to train a GBM. GBMs have the tuning parameters of tree depth and shrinkage. As always, the beauty and danger of R is that it can tune these parameters for you. The following code uses the caret package to fit a GBM model and optimize the tuning parameters:

```
#make sure needed packages are installed, then load them
if (!require(gbm)) {
  install.packages("gbm")
}

library(gbm)

#reuse train control and training data from random forest example

#train the random forest model and specify the number of trees. Use caret
#to control cross-validation
gbmModel <- train (trainData[,predictors],
 trainData[,target],
 method = "gbm",
 trControl = ctrlCV,
 verbose = FALSE)

#run prediction on test data to get class probabilities
testPredGBMProb <- predict(gbmModel, testData, type = "prob")
#run prediction again to get predicted class
testData$GBMclass <- predict(gbmModel, testData)

#grab the positive class probability (hot) and the predicted classes
testData$GBMProb <- testPredGBMProb[,"hot"]

#Show confusion matrix for results
confusionMatrix(data = testData$GBMclass, reference = testData$tempClass,
 positive = "hot")
```

The confusion matrix will look similar to the following table. In our small sample, GBM is not doing so shabby: