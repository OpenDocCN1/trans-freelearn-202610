# Working with Tables

```text
1  1984-01-01  51.4   5.6  -0.5   1 1984
2  1984-02-01   0.0   9.8   2.8   2 1984
3  1984-03-01  68.7   7.2   0.2   3 1984
4  1984-04-01  13.6  12.2   3.5   4 1984
5  1984-05-01  21.4  11.8   3.0   5 1984
6  1984-06-01   6.5  19.6  10.5   6 1984
```

Using the first expression, we converted the `dat$date` vector to a Date object (and assigned it back to `dat$date`). In the second and third expressions, we extracted the month and year components, as numeric vectors, out of `dat$date` and assigned them to the new columns, `dat$month` and `dat$year`, respectively.

# Writing a data.frame object to a CSV file

A `data.frame` object can be written to a CSV file with the `write.csv` function. The two first (and most important) parameters for this function indicate the name of the `data.frame` object, which we would like to save, and the path to the new file (including the new filename). These parameters have no defaults, so we need to specify them. For example, the following expression writes the `data.frame` object `df` to the `df.csv` file in the `C:\Data` directory:

```
```r
> write.csv(df, "C:\\Data\\df.csv")
```
```

The newly created file when opened in Excel looks like the following screenshot:

![](bbox=[0.252, 0.512, 0.748, 0.78])

Note that row names (the numbers 1 to 4 in column A) and column names have been added; this behavior can be disabled when required.