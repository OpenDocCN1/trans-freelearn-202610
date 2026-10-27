From the preceding Q-Q plot, it seems that the data is fairly normally distributed, except few outlier values such as 83rd, 91st, 32nd, 55th, and 5th data points:

```
> ## Diagnostic Plots ###
> influenceIndexPlot(fit.2, id.n=3)
> influencePlot(fit.2, id.n=3, col="blue")
StudRes Hat CookD
5 2.1986298 0.2258133 0.30797166
8 -0.1448259 0.4156467 0.03290507
10 -0.7655338 0.3434515 0.14847534
83 3.7650470 0.2005160 0.45764995
91 -2.3672942 0.3497672 0.44770173
```

![](bbox=[0.13, 0.39, 0.82, 0.68])

The problem of multicollinearity, that is, the correlation between predictor variables, needs to be verified for the final regression model. Variance Inflation Factor (VIF) is the measure typically used to estimate the multicollinearity. The formula to compute the VIF is 1/ (1-R²). Any independent variable having a VIF value of more than 10 indicates multicollinearity; hence such variables need to be removed from the model. Deleting one variable at a time and then again checking the VIF for the model is the best way to do this:

```
> ### Variance Inflation Factors
> vif(fit.2)
Price EngineSize Horsepower RPM
4.799678 20.450596 18.872498 5.788160
Rev.per.mile Fuel.tank.capacity Length Wheelbase
3.736889 5.805824 15.200301 11.850645
```