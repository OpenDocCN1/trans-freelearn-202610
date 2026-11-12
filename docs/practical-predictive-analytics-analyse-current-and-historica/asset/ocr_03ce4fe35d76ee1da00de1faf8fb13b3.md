the test, which will demonstrate any significant difference between the two curves, along with the associated p-value.

For gender, there is a significant difference at the .01 level, as shown by the low p-value, which is much less than this cutoff:

```
> survdiff(SurvObj ~ Xgender, data = ChurnStudy)
Call:
survdiff(formula = SurvObj ~ Xgender, data = ChurnStudy)
```

| N | Observed | Expected | (O-E)^2/E | (O-E)^2/V |
|---|---|---|---|---|
| Xgender=F | 341 | 137 | 177 | 9.24 | 14.5 |
| Xgender=M | 1147 | 614 | 574 | 2.86 | 14.5 |

Chisq= 14.5 on 1 degrees of freedom, p= 0.000143

# Testing for the educational differences between survival curves

Now run the survdiff function for any differences in education:

```
> survdiff(SurvObj ~ Xeducation, data = ChurnStudy)
Call:
survdiff(formula = SurvObj ~ Xeducation, data = ChurnStudy)
```

| N | Observed | Expected | (O-E)^2/E | (O- |
|---|---|---|---|---|
| Xeducation=Bachelor's Degree | 1186 | 592 | 594.8 | 0.01291 | 0 |
| Xeducation=Doctorate Degree | 88 | 45 | 45.4 | 0.00301 | 0 |
| Xeducation=Master's Degree | 214 | 114 | 110.9 | 0.08896 | 0 |

Chisq= 0.1 on 2 degrees of freedom, p= 0.94

Now look at the p value. The p value for the chi-square test is 0.94. That means that we cannot conclude that a significant difference exists for the education survival curves.

This also suggests having a closer look at the following plot. We can see that, often, the three levels of education cluster within one standard deviation of the point estimate. Overlapping data points within confidence intervals suggest that there is not enough separation between the means to attain significance: