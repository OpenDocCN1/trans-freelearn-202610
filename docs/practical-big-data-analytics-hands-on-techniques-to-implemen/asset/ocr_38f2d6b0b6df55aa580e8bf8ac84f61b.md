We can also use functions such as the following to directly find the correlated variables without plotting the correlograms:

```
correlated_columns<- findCorrelation(cor(diab[,-ncol(diab)]), cutoff = 0.5)
correlated_columns
```

# Other common data transformations

Several other data transformations are available and applicable to different situations. A summary of these transformations can be found at the documentation site for the caret package under Pre-Processing at https://topepo.github.io/caret/pre-processing.html.

The options available in the pre-process function of caret can be found from its help section, by running the command ?preProcess in RStudio. The code for it is given here:

```
Method

a character vector specifying the type of processing.

Possible values are "BoxCox", "YeoJohnson", "expoTrans", "center", "scale", "range", "knnImpute", "bagImpute", "medianImpute", "pca", "ica", "spatialSign", "corr", "zv", "nzv", and "conditionalX" (see Details below)
```

# Data sampling

You may encounter datasets that have a high level of imbalanced outcome classes. For instance, if you were working with a dataset on a rare disease, with your outcome variable being true or false, due to the rarity of the occurrence, you may find that the number of observations marked as false (that is, the person did not have the rare disease) is much higher than the number of observations marked as true (that is, the person did have the rare disease).

Machine learning algorithms attempt to maximize performance, which in many cases could be the accuracy of the predictions. Say, in a sample of 1000 records, only 10 are marked as true and the rest of the 990 observations are false.

If someone were to randomly assign all observations as false, the accuracy rate would be:

```
(990/1000) * 100 = 99%
```