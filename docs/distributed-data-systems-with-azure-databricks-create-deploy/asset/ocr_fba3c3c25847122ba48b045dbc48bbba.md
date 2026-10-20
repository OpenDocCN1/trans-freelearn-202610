11. We will create an objective function to be minimized that will be passed to Hyperopt. This will run the training and calculate the cross-validation performance metric over several types of models, defined as 'type' parameters. One thing to bear in mind about this function is that because we are using the fmin() Hyperopt function, this will try to minimize the objective function. So, to improve the performance metric – which is accuracy, in this case – we must use the negative accuracy. Otherwise, Hyperopt will try to minimize the accuracy, which is something that we obviously don't want to do:

```
def objective(params):
    classifier_type = params['type']
    del params['type']
    if classifier_type == 'svm':
        clf = SVC(**params)
    elif classifier_type == 'rf':
        clf = RandomForestClassifier(**params)
    elif classifier_type == 'logreg':
        clf = LogisticRegression(**params)
    else:
        return 0
    accuracy = cross_val_score(
        clf,
        scaled_features,
        target_discrete).mean()
    return {'loss': -accuracy, 'status': STATUS_OK}
```

Notice that the output of this function is a key-valued dictionary with the two required parameters. These are the 'loss', which is the negative accuracy, and the 'status', which is defined using one of the Hyperopt status strings that we imported previously.