In this DATA step, a uniform random variable Unif is generated using the RANUNI function. Each observation is then assigned to either the training or test data set, depending on the corresponding realization of the Unif random variable. Essentially, the Unif variable represents a biased coin flip that determines whether the observation under consideration will be used for training the model or testing it.

The threshold for Unif is chosen as 0.333; therefore, approximately one-third of the records will have unif<0.333. Unif is a random variable, however, and each realization of this random variable produces a slightly different proportion of records where unif<0.333. For this reason, this method does not always produce an exact 2:1 match. If an exact 2:1 split is required, then you can sort the data on Unif first and then the desired number of records (in this example, exactly two-thirds of all records) for the training data can be taken from the beginning of the sorted sequence while the rest of the records are assigned to the test set. In most cases, an exact split is not necessary and the code given earlier will be sufficient.

After creating the training and test sets, the first task is to fit a model using the training data, as follows:

```
proc logistic data=training outmodel=model1;
    model complications=age_at_op comorb lobeormore_code
        bilat_resec_code numsegs_resec / selection=s;
run;
```

The primary difference in this call to PROC LOGISTIC is the OUTMODEL= option, which saves the details of the fitted model such as the parameter estimates and standard errors. The strength of the OUTMODEL= option is often more evident when it is combined with its sister option, INMODEL=, which accepts a model saved by the OUTMODEL= option as seen here:

```
proc logistic inmodel=model1;
    score data=test out=testscore;
run;
```

Note the absence of a MODEL statement. This is because a new model fit is not desired. Instead, all the calculations are based on the model specified by the INMODEL= option. The SCORE statement generates predictions on the test data set using the model fit specified in the INMODEL= option. The OUT= option of the SCORE statement generates a data set that contains predicted probabilities in addition to all the original variables in the data set specified in the DATA= option.

At this point, you can use the techniques developed in Chapter 3 to plot an ROC curve using the %PLOTROC macro or you can compute the area under the curve using the %ROC macro. The ROC curve and the area under it generated from the test set are considered unbiased estimates of the true ROC curve and the AUC. The key observation is that the predicted probabilities obtained by the SCORE statement serve as the marker values. For example, the following call to the %ROC macro produces the split-sample estimate of the AUC curve (see Output 8.2), which turns out to be 0.677, roughly 0.02 less than the resubstitution estimate of 0.696, illustrating the optimism in the resubstitution estimates:

```
%roc(data=testscore, var=p_1, response=complications, contrast=1,
        details=no, alpha=.05);
```