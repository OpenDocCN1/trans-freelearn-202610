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

# # RESULT

# Confusion Matrix and Statistics

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