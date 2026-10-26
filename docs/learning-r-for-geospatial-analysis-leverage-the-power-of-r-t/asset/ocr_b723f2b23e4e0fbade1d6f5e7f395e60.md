We will next check how many dates (and which ones) are missing. First, we will verify that, indeed, not all dates appear in the time vector using the `%in%` operator (asking for each element in `all_dates` whether it appears in the time vector) and the `all` function (asking whether all of the values in the resulting logical vector are TRUE).

```
> all(all_dates %in% time)
[1] FALSE
```

The answer is no; at least one of the dates in the range of March 1, 1931 to May 15, 2014 is indeed missing from the time vector. The next question would be which one is missing, or which ones are missing? We can get the indices of the dates that appear in `all_dates` but not in `time` with the `which` function:

```
> which(!(all_dates %in% time))
[1] 5499
```

The missing date is the 5499th element of the `all_dates` vector. Its value is as follows:

```
> all_dates[which(!(all_dates %in% time))]
[1] "1946-03-20"
```

Manually examining the CSV file in a spreadsheet software will confirm that indeed the date March 20, 1946 was skipped for some reason.

Another interesting question we can ask is on what day the highest temperature (which was 41.7 degree Celsius, as we saw earlier) has been observed:

```
> max(tmax, na.rm = TRUE)
[1] 41.7
time[which.max(tmax)]
[1] "1994-06-26"
```

The highest maximum daily temperature was observed on June 6, 1994.

# Creating subsets based on dates

If we are interested in a particular subset of the time series, say the period from December 31, 2005 to January 1, 2014, we could create a subset of the dates in that period based on the time vector and a respective subset of data values based on the tmax vector. We can do this in two steps. First, we will create a logical vector, w, pointing at those dates we would like to keep:

```
> w = time > as.Date("2005-12-31") & time < as.Date("2014-1-1")
```