The interaction effects can be shown using plot with the `effects()` function:

```
plot(effect("Treatment*Age", output, xlevels=list(age=0:99)),
ticks=list(at=c(.001, .005, .01, .05, seq(.1,.9,by=.2), .95, .99,
```

As the plots indicate, there is no interaction between TreatmentP (Placebo) and Age, which is what you would expect in a controlled study. The plots also indicate the relationship between Age and Pain for the other two treatments, but there is not enough of a significant difference (at the 0.05 level) to justify including this as an individualized coefficient:

![](bbox=[0.116, 0.312, 0.845, 0.628])

# Goodness of fit statistics

Goodness of fit tests determine how well a model fits the data. There are several methods of judging how well the data fits the logistic regression model:

+   - **Data partitioning**: One way we learned how to do this was by dividing the data into training and testing datasets and measuring the degree to which they produce similar results. That method was demonstrated in Chapter 2, The Modeling Process.