```
Confusion Matrix and Statistics

        Reference
Prediction cold hot
    cold   16  5
    hot    4  20

                Accuracy : 0.8
                  95% CI : (0.654, 0.9042)
    No Information Rate : 0.5556
    P-Value [Acc > NIR] : 0.0005445

                    Kappa : 0.597
Mcnemar's Test P-Value : 1.0000000

            Sensitivity : 0.8000
            Specificity : 0.8000
             Pos Pred Value : 0.8333
             Neg Pred Value : 0.7619
                 Prevalence : 0.5556
             Detection Rate : 0.4444
   Detection Prevalence : 0.5333
      Balanced Accuracy : 0.8000

    'Positive' Class : hot
```

Resulting confusion matrix summary from gradient boosting machines.

# Ensemble

Ensemble methods are a way to combine ML models together to generate a prediction. Think of it as an ML model committee where each ML model casts its vote and the tallied result is the prediction.

There are various methods to tally the votes, and you will want to experiment with them to see if you can increase the performance of your ensemble model. However, this is outside the scope of this book.

Research and real world usages have shown that ensemble methods often perform better than any of the incorporated models alone. Ensemble methods are a way to improve real-world performance by reducing the prediction variation of any one model. They should be in your data science toolkit for IoT analytics.