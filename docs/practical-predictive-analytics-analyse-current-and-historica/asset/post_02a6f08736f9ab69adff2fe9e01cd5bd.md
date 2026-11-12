
## Plotting the data using lattice

The `lattice` package is a useful package to learn, especially for analysts who like to work in formula notation (y~x).

In this example, we will run a lattice plot in order to plot `Not.Covered.Pct` on the `y-axis`, `Year` on the `x-axis`, and produce separate plots by category.

The main call is specified by the following:

```
xyplot(Not.Covered.Pct ~ Year | cat, data = x3)
```

Since we are plotting the top 10 groups, we can specify `layout=c(5, 2)` to indicate we want to arrange the 10 plots in a 5*2 matrix. `Not.Covered.Pct` is to be arranged on the `y axis` (left side of the ~ sign), and `Year` is arranged along the `x-axis` (right side of ~ sign). The bar (`|`) indicates that the data is to be plotted separately by each category:

```
library(lattice)
x.tick.number <- 14
at <- seq(1, nrow(x3), length.out = x.tick.number)
labels <- round(seq(1999, 2012, length.out = x.tick.number))

p <- xyplot(Not.Covered.Pct ~ Year | cat, data = x3, type = "l",
    cex = 1, par.strip.text = list(cex = 0.5), scales = list(x = list(cex = 0.4, rot = 45)), layout = c(5, 2))

trellis.device()

print(p)
```

## Enrollment by Categories

Not Covered. Pct

Year

![](img/672c0cf8ba3c000a6529b117ed96945e_456_0.png)

## Plotting the data using ggplot

If you like using ggplot, a similar set of graphs can be rendered using facets:

```
require("ggplot2")

.df <- data.frame(x = x3$Year.1, y = x3$Not.Covered.Pct, z = x3$cat)
df <- .df[order(.df$x), ]
.plot <- ggplot(data = .df, aes(x = x, y = y, colour = z, shape = z))
+ geom_point()
+ geom_line(size = 1)
+ scale_shape_manual(values = seq(0, 15))
+ scale_y_continuous(expand = c(0.01, 0))
+ facet_wrap(~z)
+ xlab("Year.1")
+ ylab("Not.Covered.Pct")
+ labs(colour = "cat", shape = "cat")
+ theme(panel.margin = unit(0.3, "lines"), legend.position = "none")
print(.plot)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_458_0.png)

## Sending output to an external file

One benefit of assigning plots to a plot object is that you can later send the plots to an external file, such as a PDF, view it externally, and even view it in your browser directly from R. For example, for the Lattice graphs example, you can use `trellis.device` and specify the output parameters, and then print the object. As we illustrated in an earlier chapter, you can use `browseURL` to open the PDF in your browser:

```
# send to pdf
setwd("C:/PracticalPredictiveAnalytics/Outputs")
trellis.device(pdf, file = "x3.pdf")
print(p)
dev.off()
#NOT run
#browseURL('file:///c:///PracticalPredictiveAnalytics/Outputs//x3.pdf')
```

## Examining the output

If we examine our top plots, we can see that some groups seem to be trending more than others. For example, the under-18 age group shows a enrollment trend that is declining, while the 25-54 age groups are trending up.

## Detecting linear trends

In a linear trend model, one constructs a linear regression least squares line by running an `lm()` regression through the data points. These models are good for initially exploring trends visually. We can take advantage of our `lm()` function, which is available in base R, in order to specifically calculate the slope of the trend line.

For example, the first 14 rows show the data for the entire population group (ALL AGES). We can run a regression on Not.Covered.Pct for each of the years numbered 1-14 and see that the coefficient for the Year.1 variable is positive, indicating that there is a linear increase in the non-coverage percentage as time advances. We can also the coefficient output by itself, by wrapping the lm() function within a coef() function.

After running the regression using the `lm()` function, we can subsequently use the `coef` function to specifically extract the slope and the intercept from the `lm` model.

Since this is a regression in which there is only one independent variable (`Time`), there will only be one coefficient:

```
lm(Not.Covered.Pct ~ Year.1, data = x2[1:14, ])
>
> Call:
> lm(formula = Not.Covered.Pct ~ Year.1, data = x2[1:14, ])
>
> Coefficients:
> (Intercept)      Year.1
>  -4.102621    0.002119
coef(lm(Not.Covered.Pct ~ Year.1, data = x2[1:14, ]))
> (Intercept)      Year.1
> -4.102621443  0.002119058
```

## Automating the regressions

Now that we have seen how we can run a single time series regression, we can move on to automating separate regressions and extracting the coefficients over all of the categories.

There are several ways to do this. One way is by using the `do()` function within the `dplyr` package. Here is the sequence of events:

- The data is first grouped by category.
- Then, a linear regression (`lm()` function) is run for each category, with `Year` as the independent variable, and `Not.Covered.Pct` as the dependent variable. This is all wrapped within a `do()` function.
- The coefficient is extracted from the model. The coefficient will act as a proxy for the direction and magnitude of the trend.
- Finally, a dataframe of lists is created (`fitted.models`), where the coefficients and intercepts are stored for each regression run on every category. The categories that have the highest positive coefficients exhibit the greatest increasing linear trend, while the declining trend is indicated by negative coefficients:

```
library(dplyr)
fitted_models = x2 %>%
  group_by(cat) %>%
  do(model = coef(lm(Not.Covered.Pct ~ Year.1, data = .)))
```

All of the generated models are now in the `fitted_models` object.

The `kable` function from `knitr` gives a simple output which displays the intercept as the first number and the coefficient in the model column. As a check, we can see that the coefficients in the ALL AGES model are identical to those derived in the previous section:

```
library(knitr)
kable(fitted_models)
```

| Cat | Model |
| --- | --- |
| 18 to 24 YEARS | -0.4061834427, 0.0003367988 |
| 25 to 34 YEARS | -11.375187597, 0.005796182 |
| 35 to 44 YEARS | -10.916822084, 0.005534037 |
| 45 to 54 YEARS | -11.544566448, 0.005829194 |
| 55 to 64 YEARS | -4.709612146, 0.002409908 |
| 65 YEARS AND OVER | -1.2562375095, 0.0006334125 |
| ALL AGES | -4.102621443, 0.002119058 |
| FEMALE 18 to 24 YEARS | -2.677300003, 0.001455388 |
| FEMALE 25 to 34 YEARS | -9.990978769, 0.005088009 |
| FEMALE 35 to 44 YEARS | -9.564724041, 0.004850188 |
| FEMALE 45 to 54 YEARS | -10.36336551, 0.00523537 |
| FEMALE 55 to 64 YEARS | -4.102774957, 0.002108343 |
| FEMALE 65 YEARS AND OVER | -1.3674510779, 0.0006887743 |
| FEMALE ALL AGES | -3.817483824, 0.001970059 |
| FEMALE UNDER 18 YEARS | 3.267593386, -0.001578328 |
| MALE 18 to 24 YEARS | 4.036127727, -0.001862991 |
| MALE 25 to 34 YEARS | -9.715950286, 0.004983621 |
| MALE 35 to 44 YEARS | -7.706624821, 0.003941543 |
| MALE 45 to 54 YEARS | -10.975387917, 0.005549255 |
| MALE 55 to 64 YEARS | -5.370380544, 0.002738269 |
| MALE 65 YEARS AND OVER | -0.4834523450, 0.0002479691 |
| MALE ALL AGES | -4.315036958, 0.002232003 |
| MALE UNDER 18 YEARS | 2.914343264, -0.001401998 |
| UNDER 18 YEARS | 3.086509947, -0.001487938 |

## Ranking the coefficients

Now that we have the coefficients, we can begin to rank each of the categories by increasing trend. Since the results we have obtained so far are contained in embedded lists, which are a bit difficult to work with, we can perform some code manipulation to transform them into a regular data frame, with one row per category, consisting of the category name, coefficient, and coefficient rank:

```r
library(dplyr)
# extract the coefficients part from the model list, and then transform
# data frame so that the coefficient appear one per row, rather than
# column.

xx <- as.data.frame(fitted_models$model)
xx2 <- as.data.frame(t(xx[2, ]))

# The output does not contain the category name, so we will merge
# from the original data frame.

xx4 <- cbind(xx2, as.data.frame(fitted_models))[, c(1, 2)] #only

# rank the coefficients from lowest to highest. Force the format
# as length 2, with leading zero's

tmp <- sprintf("%02d", rank(xx4[, 1]))

# Finally prepend the rank to the actual category
xx4$rankcat <- as.factor(paste(tmp, "-", as.character(xx4$cat)))

# name the columns
names(xx4) <- c("lm.coef", "cat", "coef.rank")
# and View the results
View(xx4)
```

As you can see, columns 2, 3, and 4 now contain a neatly arranged representation of the `coefficients`, `category`, and `coef.rank`, which was derived by ranking `lm.coef` from smallest to largest, and then prepending the rank order to the category:
