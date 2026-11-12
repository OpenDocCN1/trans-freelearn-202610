```
# create a new variable and plot it against on the original point
t2$z = (t2$Latitude^2*t2$High.Low.Temp^2)
plot(t2$High.Low.Temp,t2$z)
```

![](bbox=[0.119, 0.211, 0.881, 0.502])

Typically, SVMs are used with data that is very high dimensional. That could include applications involving object and facial recognition. SVM can be very accurate, but its disadvantage includes interpretability and performance. SVMs can also overfit the data, since the decision boundaries are highly dependent upon the choice of the kernel that is used to perform the mapping. There are a large number of kernel choices available and the choice of a kernel should not be made based upon the best model fit. This would not be unlike using higher order polynomial functions to overfit a regression model.

# Analyzing consumer complains data using SVM

In this example, we will use a text classification model to define a linear SVM model. Unstructured text data is a perfect way to illustrate SVM since the data is very high dimensional.

To illustrate SVM classifiers, we will begin by reading in some consumer complaint data regarding student loans. We will be using two packages,