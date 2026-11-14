
```python
stl = MultiSeasonalDecomposition(seasonal_model="fourier", seasonality_periods=["day_of_year", "day_of_week", "hour"], model="additive", n_fourier_terms=10)
res_new = stl.fit(pd.Series(ts, index=ts_df.index))
```

The key parameters here are as follows:

1.  `seasonality_periods` is the list of expected seasonalities. For `STL`, it is a list of seasonal periods, while for `FourierDecomposition`, it is a list of strings that denotes `pandas` datetime properties.

2.  `seasonality_model` takes `fourier` or `averages` as arguments to determine the type of seasonality decomposition.

3.  `model` takes `additive` or `multiplicative` as arguments to determine the type of decomposition.

4.  `n_fourier_terms` determines the number of Fourier terms to be used to extract the seasonality. As we increase this parameter, the more complex the seasonality that is extracted from the data.

5.  `lo_frac` is the fraction of the data that will be used to fit the LOESS regression.

6.  `lo_delta` is the fractional distance within which we use linear interpolation instead of weighted regression. Using a non-zero `lo_delta` significantly decreases computation time.

Let's see what the decomposition looks like when using Fourier decomposition:

![Figure 3.17 – Multiple seasonality decomposition using Fourier terms](media/file51.jpg)

Figure 3.17 – Multiple seasonality decomposition using Fourier terms

Here, we can see that the `day_of_week` seasonality has been extracted. To see the `day_of_week` and `hour` seasonal components, we need to zoom in a bit:

![Figure 3.18 – Multiple seasonality decomposition using Fourier terms (zoomed-in for a month)](media/file52.jpg)

Figure 3.18 – Multiple seasonality decomposition using Fourier terms (zoomed-in for a month)

Here, we can observe that the `hour` seasonality has been extracted well and that it has also isolated the `day_of_week` seasonal component, which peaks on weekends. The `discrete step` nature of the `day_of_week` seasonal component is because the frequency of the data is half-hourly, and for 48 data points, `day_of_week` will be the same.

> **NOTE**
>
> `MSTL` has also been implemented in `statsmodels` and the accompanying notebook has the code to use that. The key difference between `statsmodels` and the implementation bundled within the code for the book is that the one in the book also has the option for using Fourier series based decomposition.

We have summarized the four techniques we've covered in the following table:

![Table 3.1 – Different seasonal decomposition techniques](media/file53.jpg)

Table 3.1 – Different seasonal decomposition techniques

## Detecting and treating outliers

An `outlier`, as its name suggests, is an observation that lies at an abnormal distance from the rest of the observations. If we are looking at a `data generating process` (`DGP`) as a stochastic process that generates the time series, the outliers are the points that have the least probability of being generated from the `DGP`. This can be for many reasons, including faulty measurement equipment, incorrect data entry, and black-swan events, to name a few. Being able to detect such outliers and *treat* them may help your forecasting model understand the data better.

Outlier/anomaly detection is a specialized field itself in time series, but in this book, we are going to restrict ourselves to simpler techniques of identifying and treating outliers. This is because our main aim is not to detect outliers, but to clean the data for our forecasting models to perform better. If you want to learn more about anomaly detection, head over to the *Further reading* section for a few resources to get started.

Now, let's look at a few techniques for identifying outliers.

> **NOTEBOOK ALERT**
>
> To follow along with the complete code for detecting outliers, use the `03-Outlier Detection.ipynb` notebook in the `chapter03` folder.

### Standard deviation

This is a rule of thumb that almost everyone who has worked with data for some time would have heard of – if ![](media/file54.png) is the mean of the time series and ![](media/file55.png) is the standard deviation, then anything that falls beyond ![](media/file56.png) is an `outlier`. The underlying theory is deeply rooted in statistics. If we assume that the values of the time series follow a normal distribution (which is a symmetrical distribution with very desirable properties), using probability theory, we can derive that 68% of the area under the normal distribution lies within one standard deviation on either side of the mean, about 95% of the area within two standard deviations, and about 99% of the area within three standard deviations. So, when we make the bounds as three standard deviations (by using the rule of thumb), what we are saying is that if any observation whose probability of belonging to the probability distribution is less than 1%, then they are an outlier. Moving slightly to more practical issues, this cutoff of three standard deviations is in no way sacrosanct. We need to try out different values of this multiple and determine the right multiple by subjectively evaluating the results we get. The higher the multiple is, the fewer outliers there will be.

For highly seasonal data, the naïve way of applying the rule to the raw time series will not work well. In such cases, we must deseasonalize the data using any of the techniques we discussed earlier and then apply the outliers to the residuals. If we don't do that, we may flag a seasonal peak as an outlier, which is not what we want.

Another key assumption here is the normal distribution. However, in reality, a lot of the time series we come across may not be normal and hence the rule will lose its theoretical guarantees fast.

### Interquartile range (IQR)

Another very similar technique is using the `IQR` instead of the standard deviation to define the bounds beyond which we mark the observations as outliers. `IQR` is the difference between the 3rd quartile (or the 75th percentile or 0.75 quantile) and the 1st quartile (or the 25th percentile or 0.25 quantile). The upper and lower bounds are defined as follows:

*   Upper bound = Q3 + n x IQR
*   Lower bound = Q1 - n x IQR

Here, `IQR = Q3 - Q1`, and `n` is the multiple of IQRs that determines the width of the acceptable area.

For datasets where we observe high occurrences of outliers and wild variations, this is slightly more robust than the standard deviation. This is because the standard deviation and the mean are highly influenced by individual points in the dataset. If `2σ` was the rule of thumb in the earlier method, here, it is 1.5 times the `IQR`. This also ties back to the same normal distribution assumption, and 1.5 times the `IQR` is equivalent to ~3σ (2.7σ to be exact). The point about deseasonalizing before applying the rule applies here as well. It applies to all the techniques we will see here.

### Isolation Forest
