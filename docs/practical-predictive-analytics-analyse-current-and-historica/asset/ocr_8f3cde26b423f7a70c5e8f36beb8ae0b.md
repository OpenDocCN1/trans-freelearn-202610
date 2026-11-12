As in previous examples, one needs to supply the predictions, time variable, and event variable as arguments to the function. We will also print the associated confidence intervals:

```
library(survcomp)
cindex_validation = concordance.index (pred_validation, surv.time
                                  surv.event=ChurnStudy.test
#
print(cindex_validation$c.index)
print(cindex_validation$upper)
print(cindex_validation$lower)
```

For our example, the computed index is 70%, with confidence intervals showing that it can range between 60 and 77%:

```
> print(cindex_validation$c.index)
[1] 0.6973212
> print(cindex_validation$upper)
[1] 0.7736508
> print(cindex_validation$lower)
[1] 0.6082843
```