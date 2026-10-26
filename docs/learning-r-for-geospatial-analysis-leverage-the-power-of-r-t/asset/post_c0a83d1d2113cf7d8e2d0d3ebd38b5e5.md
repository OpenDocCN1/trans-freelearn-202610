
The core of the `gstat` package is a function named `gstat`, which is used to produce objects (of a class also named `gstat`) that hold all necessary information to perform spatial interpolation (as well as other operations such as cross-validation). Among the numerous parameters that the `gstat` function accepts, there is, in fact, no parameter where we can specify the desired interpolation method. Instead, when interpolation is triggered on a `gstat` object, the method is determined based on the input data the object contains. In this chapter, we are going to explore only a portion of the functionality of `gstat`, utilizing three of its parameters:

- `formula`: The formula that defines the dependent and independent variables (an object of the `formula` class)
- `data`: The measured point data (a `SpatialPointsDataFrame` object; it can also be a `data.frame` object but we will not use this option)
- `model`: The variogram model (an object of the `variogramModel` class)

These three parameters determine whether IDW, OK, or UK is used based on the following decision tree:

- If there is no variogram model (`model = NULL`, the default), IDW is used
- If a variogram model is passed to `model`, and the formula:
  - contains no independent variables, OK is used
  - contains at least one independent variable, UK is used

Our dependent variable column is `value` and an intercept-only model (that is, a model with no independent variables) formula in R is, by convention, specified by `~ 1`. Therefore, the following expression creates a `gstat` object (named `g`) that holds the necessary information to predict the minimum temperature based on the measured data in `dat` using IDW:

```
> library(gstat)
> g = gstat(formula = value ~ 1, data = dat)
```

Printing `g` shows a summary of the information it contains as follows:

```
> print(g)
data:
var1 : formula = value ~ 1 ; data dim = 75 x 5
```

![](img/ebfd6294770b225fdeceda9a7b8b560e_265_0.png)

> Note that this is just a summary; `g`, in fact, also contains the `dat` object itself, with the measured values and independent variables (if any), since it serves as the calibration data necessary to actually perform the interpolation.

## Spatial Interpolation of Point Data

To interpolate, what we now need is to provide the points for which we would like to make predictions. Conveniently, the `raster` function includes a function named `interpolate` that, given a raster (the `object` parameter) and a spatial prediction model (the `model` parameter), yields a new raster with predicted values. This way, we do not need to make the manual conversions to and from a raster, as we did in nearest-neighbor interpolation. For example, the following expression uses the model and calibration data held in `g` to make temperature predictions for the grid defined by `dem_spain`:

```
> z = interpolate(dem_spain, g)
```

There are two things to note here:

- First, as you remember, text messages are omitted in this book to save space. However, readers who run the preceding expression in R will see the message `[inverse distance weighted interpolation]` appearing on the screen. This reassures the user that indeed the expected type of model has been chosen by `gstat`.
- Second, it is important to understand that, if we do not use covariates (that is, independent variables in the model formula), the `dem_spain` raster serves only as a grid pointing to the locations where we would like to calculate predicted values (the raster values play no role in the interpolation). This is opposed to UK interpolation, where the raster serves both to point at predicted locations and provide covariate values for each location.

Plotting the predicted surface `z` will show that predicted values were generated for all raster cells (including NA cells). It is usually reasonable to make predictions for a more specific extent, such as the land area of Spain. We can use `mask` to remove the unnecessary predictions:

```
> z = mask(z, dem_spain)
```

Now let's plot the predicted value raster, and the station locations on top of it, with the following pair of expressions:

```
> plot(z)
> plot(dat, add = TRUE)
```

The attribute table columns of the cross-validation result refer to the following information:

- `var1.pred`: Predicted value
- `var1.var`: Variance (only for kriging)
- `observed`: Observed value
- `residual`: Residual, the difference between observed and predicted values
- `zscore`: Z-score (only for kriging)
- `fold`: Cross-validation count

The important column, for our purpose here, is `residual` — the difference between the observed and predicted values. Based on the residuals, we can assess how far predicted values are from the observed ones, or in other words, prediction accuracy. One of the simplest and most common metrics of agreement between observed and predicted values is the RMSE. Given a set of predicted-observed value pairs, the RMSE is calculated as follows:

$$RMSE = \sqrt{\frac{\sum_{i=1}^{n} (pred_i - obs_i)^2}{n}} \quad (3)$$

Here, RMSE is the root mean square error, n is the overall number of points, and pred_i and obs_i are the predicted and observed values at point i, respectively. As the difference between predicted and observed values is smaller, the RMSE will be lower, indicating that the model is more accurate. Given the same input data, we can compare the RMSE values among different models to select the most accurate one. Translating equation (3) to the R syntax, we can calculate RMSE based on the attribute table of `cv` as follows:

```
> sqrt( sum( (-cv$residual)^2 )/nrow(cv) )
[1] 1.88049
```

The RMSE in this case is equal to 1.88049. Note that the expression consists of the `-cv$residual` vector (equivalent to `cv$var1.pred - cv$observed`) squared, then summed, and divided by the total number of points. Finally, the square root of the result is extracted.

For convenience, we can wrap this expression into our own function, which calculates RMSE, given a `gstat.cv` output:

```
> rmse = function(x) sqrt(sum((-x$residual)^2)/nrow(x))
> rmse(cv)
[1] 1.88049
```

We will use this function later.

![](img/ebfd6294770b225fdeceda9a7b8b560e_271_0.png)

You may have noticed that the `gstat` object we initially defined (`g`) was hereby used for two distinct and independent purposes. First, to calculate predicted values (using the `interpolate` function), then to perform cross-validation (using the `gstat.cv` function). This is not incidental, but rather a characteristic object-oriented behavior common to all, or most, of R's statistical procedures. This approach is advantageous over the menu-based approach commonly encountered in statistical or GIS software, where we carry out a given analysis by making selections in a set of dialog boxes, and receive a set of results (some of which we do not need, while others are lacking since the tool's creator did not include them). Conversely, when a statistical function is applied in R, we usually get an object that holds all the data that is necessary to carry out the analysis (such as `g`) or an object that holds a comprehensive set of results (such as `cv`). From such objects, we can then derive the results we are interested in by applying specific functions. Moreover, we can create functions of our own to carry out calculations or extract meaningful data, for which no built-in methods have been defined (such as the `rmse` function we defined earlier). Readers who are interested in using R as a general statistical analysis toolbox will repeatedly encounter this approach. For example, the `lm` function is used to perform ordinary linear regression in R; on its returned object, of class `lm`, numerous functions (such as `summary`, `residuals`, and `plot`) may be applied to derive the information we are interested in.

The following screenshot shows how the plot appears in our case:

![](img/ebfd6294770b225fdeceda9a7b8b560e_273_0.png)

We can see that semivariance increases with distance. This means that temperature values are more diverse when considering stations further apart. In other words, temperature is spatially autocorrelated.
