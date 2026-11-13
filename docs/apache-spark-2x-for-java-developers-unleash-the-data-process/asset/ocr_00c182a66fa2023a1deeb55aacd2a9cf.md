An estimator has a fit() method that is to be invoked on learning dataframe in order to generate a model that can transform the input data.

+   • Pipeline: A pipeline chains together multiple stages of machine learning workflow each comprising of a transfomer or an estimator. The sequence of job chaining is specific to an algorithm or use case and hence the order of PipelineStages, that is, transformer and estimator, as being passed to a pipeline is important.

Now that we understand these common terminologies, let's write our first Machine Learning algorithm and understand how these parameters are related to each other. For simplicity we will try and understand a very basic regression model called linear regression. Please be forewarned that the results will not be perfect as our intention here is to only understand major components in an ML workflow.

A simple linear regression model can be represented as an equation of the form:

```
y = ax + b
```

Where x is a variable or in ML parlance a feature vector and a is the slope of the line. b is a constant that represents the intercept on the y axis when x=0.

![](bbox=[0.35, 0.64, 0.65, 0.86])

Using this model of linear regression, we will try and predict the values of