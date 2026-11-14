
## Interpolation Techniques

Another family of imputation techniques covers interpolation:

*   **`Linear Interpolation`**: Linear interpolation is just like drawing a line between the two observed points and filling the missing values so that they lie on this line. This is how we do it:

```
df['pm2_5_1_hr'].interpolate(method="linear")
```

*   **`Nearest Interpolation`**: This is intuitively like a combination of the forward and backward fill. For each missing value, the closest observed value is found and is used to fill in the missing value:

```
df['pm2_5_1_hr'].interpolate(method="nearest")
```

Let's plot the two interpolated lines:

![Figure 2.5 – Imputed missing values using linear and nearest interpolation](media/file18.jpg)

Figure 2.5 – Imputed missing values using linear and nearest interpolation

There are a few non-linear interpolation techniques as well:

*   **`Spline, Polynomial, and Other Interpolations`**: In addition to linear interpolation, `pandas` also supports non-linear interpolation techniques that call a `SciPy` routine at the backend. Spline and polynomial interpolations are similar. They fit a spline/polynomial of a given order to the data and use that to fill in missing values. While using `spline` or `polynomial` as the method in `interpolate`, we should always provide `order` as well. The higher the order, the more flexible the function that is used will be to fit the observed points. Let's see how we can use spline and polynomial interpolation:

```
df['pm2_5_1_hr'].interpolate(method="spline", order=2)
df['pm2_5_1_hr'].interpolate(method="polynomial", order=5)
```

Let's plot these two non-linear interpolation techniques:

![Figure 2.6 – Imputed missing values using spline and polynomial interpolation](media/file19.jpg)

Figure 2.6 – Imputed missing values using spline and polynomial interpolation

For a complete list of interpolation techniques supported by `interpolate`, go to [https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.interpolate.html](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.interpolate.html) and [https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.interp1d.html#scipy.interpolate.interp1d](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.interp1d.html#scipy.interpolate.interp1d).

Now that we are more comfortable with the way `pandas` manages datetime, let's go back to our dataset and convert the data into a more manageable form.

> **NOTEBOOK ALERT**
>
> To follow along with the complete code for pre-processing, use the `02 - Preprocessing London Smart Meter Dataset.ipynb` notebook in the `chapter01` folder.

## Converting the Half-Hourly Block-Level Data (hhblock) into Time Series Data

Before we start processing, let's understand a few general categories of information we will find in a time series dataset:

*   **Time Series Identifiers**: These are identifiers for a particular time series. It can be a name, an ID, or any other unique feature – for example, the SKU name or the ID of a retail sales dataset or the consumer ID in the energy dataset that we are working with are all time series identifiers.
*   **Metadata or Static Features**: This information does not vary with time. An example of this is the ACORN classification of the household in our dataset.
*   **Time-Varying Features**: This information varies with time – for example, the weather information. For each point in time, we have a different value for weather, unlike the Acorn classification.

Next, let's discuss formatting of a dataset.

### Compact, Expanded, and Wide Forms of Data

There are many ways to format a time series dataset, especially a dataset with many related time series, like the one we have now. Apart from the standard terminology of `wide` data, we can also look at two non-standard ways of formatting time series data. Although there is no standard nomenclature for those, we will refer to them as `compact` and `expanded` in this book.

Compact form data is when any particular time series occupies only a single row in the `pandas` DataFrame – that is, the time dimension is managed as an array within a DataFrame row. The time series identifiers and the metadata occupy the columns with scalar values and then the time series values; other time-varying features occupy the columns with an array. Two additional columns are included to extrapolate time – `start_datetime` and `frequency`. If we know the start datetime and the frequency of the time series, we can easily construct the time and recover the time series from the DataFrame. This only works for regularly sampled time series. The advantage is that the DataFrames take up much less memory and are easy and faster to work with:

![Figure 2.7 – Compact form data](media/file20.jpg)

Figure 2.7 – Compact form data

The expanded form is when the time series is expanded along the rows of a DataFrame. If there are *n* steps in the time series, it occupies *n* rows in the DataFrame. The time series identifiers and the metadata get repeated along all the rows. The time-varying features also get expanded along the rows. And instead of the start date and frequency, we have the `datetime` as a column:

![Figure 2.8 – Expanded form data](media/file21.jpg)

Figure 2.8 – Expanded form data

If the compact form had a time series identifier as the key, the time series identifier and the `datetime` column would be combined and become the key.

Wide-format data is more common in traditional time series literature. It can be considered a legacy format, which is limiting in many ways. Do you remember the stock data we saw earlier (Figure 2.2)? We have the date as an index or as one of the columns and the different time series as different columns of the DataFrame. As the number of time series increases, they become wider and wider, hence the name. This data format does not allow us to include any metadata about the time series. For instance, in our data, we have information about whether a particular household is under standard or dynamic pricing. There is no way for us to include such metadata in the wide format. From an operational perspective, the wide format also does not play well with relational databases because we have to keep adding columns to the table when we get new time series. We won't be using this format in this book.

### Enforcing Regular Intervals in Time Series

One of the first things you should check and correct is whether the regularly sampled time series data that you have has equal intervals of time. In practice, even regularly sampled time series have some samples missing in between because of some data collection error or some other peculiar way data is collected. So, while working with the data, we will make sure we enforce regular intervals in the time series.

> **BEST PRACTICE**
>
> While working with datasets with multiple time series, it is best practice to check the end dates of all the time series. If they are not uniform, we can align them with the latest date across all the time series in the dataset.

In our smart meters dataset, some `LCLid` columns end much earlier than the rest. Maybe the household opted out of the program, or they moved out and left the house empty; the reason could be anything. However, we need to handle that while we enforce regular intervals.

We will learn how to convert the dataset into a time series format in the next section. The code for this process can be found in the `02 - Preprocessing London Smart Meter Dataset.ipynb` notebook.

### Converting the London Smart Meters Dataset into a Time Series Format

For each dataset that you come across, the steps you would have to take to convert it into either a compact or expanded form would be different. It depends on how the original data is structured. Here, we will look at how the London Smart Meters dataset can be transformed so that we can transfer those learnings to other datasets.
