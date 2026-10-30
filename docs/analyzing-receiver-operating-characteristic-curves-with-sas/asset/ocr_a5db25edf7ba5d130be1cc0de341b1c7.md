It is easy to imagine the utility of these probabilities as occasional consumers of a weather forecast. If you drive from Leeds to Hull on the M62 only a few nights during a winter, all you care about is whether the forecast will be accurate on those few nights. On the other hand, the agency responsible for keeping the motorway free of frost might be more interested in sensitivity and specificity when deciding whether to pay for these forecasts since that speaks to the entire “cohort” of nights in the upcoming winter, and the decision of the agency should be based on the performance of the forecast in the long run.

It is easy to recognize that sensitivity, specificity, and positive and negative predictive values are all binomial proportions if the corresponding denominators are considered fixed. The technical term is conditioning on the denominators. This gives easy rise to the use of binomial functionality within PROC FREQ to compute interval estimates.

The following code uses two separate calls to PROC FREQ to obtain estimates of sensitivity and specificity along with confidence intervals and a test of hypothesis about a specific null value. Note the use of the WHERE statement to choose the appropriate denominator for each calculation:

```
title "Sensitivity";
proc freq data=m62(where=(observed='Frost'));
  table forecast / binomial(p=0.8);
  weight weight;
  exact binomial;
run;
```

The values specified in parentheses following the BINOMIAL keyword are the null values for the test of hypothesis. A null value of 80% is specified. This value is chosen only to demonstrate the hypothesis testing feature of the BINOMIAL option. It does not correspond to an actual value of interest in the example.

Output 2.2 shows the sensitivity, which is estimated to be 87.88% or 29 out of 33. Each of these 33 nights of frost can be thought of as independent Bernoulli variates: 29 of them were positive (1) and 4 were negative (0). Their sum (29) is the binomial variate with a sample size (denominator) of 33, with sensitivity as the binomial proportion of the variate.