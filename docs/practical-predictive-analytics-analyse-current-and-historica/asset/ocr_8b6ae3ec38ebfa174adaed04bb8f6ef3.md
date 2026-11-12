# Plotting outside of Spark

If you wish to use other tools to plot the data, you can first take a sample of the Spark data and plot it using another package such as ggplot. Â Note that some versions of Spark may now have ggplot integrated and available for use within Spark. Â However this example will show another example of extracting data which can be used by other packages.

# Collecting a sample of the results

We will take a 2% sample of all of the predictions and then print some of the results. Note that the Spark sample function has a different syntax from the base R sample function we used earlier. You could also specify this as SparkR::sample to make sure you are invoking the correct function:

```
local = collect(sample(preds, F, .02))
head(local)
```

```
sample_bin outcome pregnant glucose pressure triceps insulin mass pedigree
1 -265 0 1.75 83.4 48.4 15.1 55.3 29.1 0.298
2 -339 0 2.28 75.3 67.4 25.4 86.0 31.8 0.166
3 -367 0 4.21 87.6 72.2 31.6 73.8 33.3 0.701
4 -109 0 1.32 83.6 60.5 25.8 84.8 35.6 0.801
5 -239 0 2.08 110.9 60.4 33.6 269.4 39.9 0.459
6 -111 0 6.89 115.9 87.4 29.5 108.9 32.3 0.839

age label prediction grp totrows error errorsqr correct errbin
1 16.8 0 0.0144 1 1238000 0.0144 0.000207 Y 0
2 36.5 0 0.0232 1 1238000 0.0232 0.000538 Y 0
3 26.9 0 0.1055 1 1238000 0.1055 0.011136 Y 1
4 21.3 0 0.0473 1 1238000 0.0473 0.002234 Y 0
5 36.0 0 0.2974 1 1238000 0.2974 0.088423 Y 3
6 38.8 0 0.3789 1 1238000 0.3789 0.143568 Y 4
```

# Examining the distributions by outcome

Next, you can run ggplot to graphically display the errors grouped by outcome. The resulting boxplots show that the three quartiles for diabetes are