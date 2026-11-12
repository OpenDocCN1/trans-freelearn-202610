
percent of the population living in urban areas. The `USArrests` dataset should be loaded automatically as part of the `datasets` package.

You can get description of the `USArrests` dataset by entering the help command for the `USArrests` dataset at the console.

To see the correlation between all of the variables, enter the following on the console line:

```
library(datasets)
pairs(USArrests)
```

The output is shown here:

![](img/672c0cf8ba3c000a6529b117ed96945e_178_0.png)

The `pairs()` function creates mini scatterplots for all of the different combinations of continuous variables in the dataframe. To examine a particular pair, look at the cell that intersects the appropriate row and column labels for the variables, which are shown in the diagonal row of the matrix.
For example, the scatterplots for **Murder** with **Assault** are shown by locating the cells that these labels intersect. That would be in two places: row 1, column 2 of the matrix, and row 2 and column 1. Note that most of the variables seem strongly correlated with each other. The urban population variable (UrbanPop) seems to be the least correlated variable, at least based upon the scatter that is represented visually. Murder and assault seems to have a stronger correlation. That makes me wonder if we would need to include all of the variables in a prediction model.

To run principal components on this data we will use the prcomp() function. Note the comments in the code that scaling (normalization) is appropriate, since the variables have different scales:

```
require(graphics)
```

```
## the variances of the variables in the
## USArrests data vary by orders of magnitude, so scaling is appropriate
prcomp(USArrests, scale = TRUE)
```

The output of the prcomp() function is shown here. This output describes the loadings of each of the variables on each of the principal components. The original variables are shown as rows, and the new variables, the principal components, are shown as columns. By default, they are named PC1, PC2, PC3, and PC4:

```
Standard deviations: (1, .., p=4):
[1] 1.5748783 0.9948694 0.5971291 0.4164494
```

```
Rotation (n x k) = (4 x 4):
                PC1        PC2        PC3        PC4
Murder   -0.5358995  0.4181809 -0.3412327  0.64922780
Assault  -0.5831836  0.1879856 -0.2681484 -0.74340748
UrbanPop -0.2781909 -0.8728062 -0.3780158  0.13387773
Rape     -0.5434321 -0.1673186  0.8177779  0.08902432
```

The numbers given under each column are the actual coefficients used, which are used to determine the linear combination. So, we can calculate the first, and most important principal component as follows:

```
PC1=Murder * 0.5358995 + Assault*-0.5831836 + UrbanPop*-0.2781909
```

Since the variables have been scaled (or standardized) to begin with, the magnitude of the coefficients can give a rough estimate of how much a variable contributes to each of the components. For example, PC1 could be described as the variation due to murder, rape, and assault, while PC2 is more about the variations in urban population. PC3 might be about the effect of rape within urban populations, and PC4 could be about murder when assault is not involved.

The overwhelming power of PC1 can be shown in the following plot. Each succeeding principal component measures the leftover variation unexplained by the previous components:

```
plot(prcomp(USArrests))
```

![](img/672c0cf8ba3c000a6529b117ed96945e_181_0.png)

The `prcomp summary` function shows the cumulative percentage of explained variation:

```
summary(prcomp(USArrests, scale = TRUE))
```

This is the following output:

Importance of components:

| | PC1 | PC2 | PC3 | PC4 |
|---|---|---|---|---|
| Standard deviation | 1.5749 | 0.9949 | 0.59713 | 0.41645 |
| Proportion of Variance | 0.6201 | 0.2474 | 0.08914 | 0.04336 |
| Cumulative Proportion | 0.6201 | 0.8675 | 0.95664 | 1.00000 |

In the last line of the output, `Cumulative Proportion`, you can see that the first component explains 62% of the variation, and the first two principal components explain a total of 87% of the variation in the data.

This was based on four original variables. Principal Components have reduced the number of variables by 50%. Think how many variables could be reduced if you started with a feature set of over 100 variables!

## All subsets regression

All subsets regression is another method you can use for variable selection. It works by running separate regressions for different combinations of one variable, two variables, three variables at a time, until it has used all of the variables or until a specified stopping point. As part of the output, the best model for each of the combination of variables is then calculated. That will then give you an idea of which is the best single one variable model, two variable model, and so on, so that you can pare down a large list of variables into a smaller number of important ones using some plots and output statistics.

### An example: airquality

In this example, we will use the `regsubsets()` function contained within the `leaps` package to determine which variables are important for predicting temperature:

```
install.packages("leaps")
library(leaps)
data(airquality)
str(airquality)
```

In the following `regsubsets()` function call, we specify that we want the best model for each combination of one, two, and three variables. By default, the function will compute the best model for all variables in the model. In the function call, I illustrate specifying a max value, since I want to be able to stop the algorithm after a certain number of variables has been reached. This becomes important when you use data that has a large number of variables, and you do not want to tie up precious memory:

```
out <- regsubsets(Temp ~ ., data = airquality, nbest = 1, nvmax = 99)
```

Run a `summary()` on the output:

```
summary(out)
```

The following output shows the variables contained in the best 1-5 variable model. Note that there are only five variables, so you cannot have more than a five-variable model:

```
Subset selection object
Call: regsubsets.formula(Temp ~ ., data = airquality, nbest = 1,
5 Variables (and intercept)
     Forced in Forced out
Ozone             FALSE       FALSE
Solar.R           FALSE       FALSE
Wind              FALSE       FALSE
Month             FALSE       FALSE
Day               FALSE       FALSE
1 subsets of each size up to 5
Selection Algorithm: exhaustive
              Ozone Solar.R Wind Month Day
1  ( 1 ) "**"   " "   " "  " "   " "   " "
2  ( 1 ) "**"   " "   " "  "**"  " "   " "
3  ( 1 ) "**"   "**"  " "  " "  "**"  " "
4  ( 1 ) "**"   "**"  " "  " "  "**"  "* "
5  ( 1 ) "**"   "**"  "**" "**" "**" "**"
```

It is useful to be able to interpret the stars in the output table. The row indicates the number of variables in the model, and the * in the column shows that the variable is part of that combination. So we can see that `Ozone` is the best single variable predictor, and `Ozone`, with `Month` is the best two-variable predictor:

| # | Variables in Model | Ozone | Solar.R | Wind | Month | Day |
|---|---|---|---|---|---|---|
| 1 | Ozone | ** | " " | " " | " " | " " |
| 2 | Ozone, Month | ** | " " | " " | ** | " " |
| 3 | Ozone, Month, Solar.R | ** | ** | " " | ** | " " |
| 4 | Ozone, Month, Solar.R, Day | ** | ** | " " | ** | * |
| 5 | Ozone, Month, Solar.R, Day, Wind | ** | ** | ** | ** | ** |

To see the adjusted R-squares for each of the combinations of variables, run the following at the command line:

```
summary(out)$adjr2
```

This is the following output:

```
[1] 0.4832625 0.5746686 0.5801736 0.5836452 0.5823619
```

You can see that the one-variable model has an adjusted R-Square of .48, while the two-variable model adjusted R-Square is .57. For three variables and beyond it seems as though the increase in R-Square is incremental. So, it would seem that a two-variable model of ozone and month would be adequate for predicting air quality.

## Adjusted R-square plot

The adjusted R-square plot is also helpful in selecting a model. Each row of the following plot represents a separate model, with the intercept and all of the variables as columns. The `plot` function shows how adjusted R-square (y-axis) changes for each of the variables in the model:

```
plot(out, scale = "adjr2")
```
