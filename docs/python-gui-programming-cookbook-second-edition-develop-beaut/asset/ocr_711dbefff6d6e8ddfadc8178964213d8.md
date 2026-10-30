```
# the commas after t0, t1 and t2 are required
t0 = axis.plot(xValues, yValues0)      # no comma here
t1, = axis.plot(xValues, yValues1)
t2, = axis.plot(xValues, yValues2)
```

![](bbox=[0.045, 0.145, 0.9, 0.425])

# How it works...

We enhanced our chart by plotting three lines of data in the same chart and giving it a legend in order to distinguish the data that those three lines plot.

# Scaling charts

In the previous recipes, while creating our first charts and enhancing them, we hardcoded the scaling of how those values are visually represented.

While this served us well for the values we were using, we often plot charts from very large databases.

Depending on the range of that data, our hardcoded values for the vertical y-dimension might not always be the best solution, and may make it hard to see the lines in our charts.