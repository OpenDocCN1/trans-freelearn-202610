# Trying out other learning algorithms

This is the line we edit to switch to a different approach. Consult the sklearn documentation for details, but they are designed to be easily switchable. Test each one to see which works best for you.

Most of the time, you will replace GaussianNB with the name of another algorithm. We do that in Chapter 12 to demonstrate decision trees.

8. The cell also trains our model in one simple line:

```
model.fit(features, label)
```

# Is that it? Training a model in one line

You may be surprised that training our model takes only one simple line. Most of the work in training an ML model is in gathering and preparing the data beforehand.

9. The cell containing for loop in range(1000): makes 1,000 predictions based on random data and captures the results for us to easily graph in the next step. We won’t highlight this code sample, since it’s quite long. The key line is this:

```
predicted= str(model.predict([[age,country_code]]))
```

This line calls our model to make the prediction (and convert the prediction into text).

10. The cell starting %matplotlib inline generates a chart like the one shown in Figure 11.16 to visualize this set of predictions:

![](bbox=[0.25, 0.58, 0.68, 0.82])

Figure 11.16 – A plot of the predictions made by naïve Bayes