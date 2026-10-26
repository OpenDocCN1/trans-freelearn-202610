Note that with `latitude=latitude[1]` we say, in plain language: take the first latitude value you see and assign it to the aggregated table, per station. Since the location of a given station should be constant over time, we can take any of the latitude values. However, we do not know exactly how many rows of data each station has (actually, in this particular case, we do know it is at least 12 since otherwise the data for that station could not have formed a complete 12 months series and would have been removed altogether); therefore, selecting the first one is a reasonable option.

We will save this `data.frame` object to a file since we will use it in subsequent chapters:

```
write.csv(spain_stations, "C:\\Data\\spain_stations.csv",\n          row.names = FALSE)
```

The additional parameter `row.names` indicates whether we would like row names to be saved as an additional column in the CSV file (in this case, we do not).

Next, we will aggregate the climatic data itself, per station/variable/year combination. Here, our purpose is to find the sum of each month (in case of rainfall) or the average of each month (in case of temperature). We will use `ifelse` to assign a sum of the 12 values when the variable is `tpcp` or the average otherwise (when the variable is either `mxtx` or `mntt`). Let's take a look at the following example:

```
spain_annual = ddply(dat3,\n  .(station, variable, year),\n  + summarise,\n  + value = ifelse(variable[1] == "tpcp",\n  + sum(value, na.rm = TRUE),\n  + mean(value, na.rm = TRUE)))\n> head(spain_annual)\n       station variable year value\n1 GHCND:SP000003195     tpcp 1984 494.3\n2 GHCND:SP000003195     tpcp 1985 338.8\n3 GHCND:SP000003195     tpcp 1986 387.5\n4 GHCND:SP000003195     tpcp 1987 544.9\n5 GHCND:SP000003195     tpcp 1988 413.9\n6 GHCND:SP000003195     tpcp 1989 560.7
```

Note that, once again, we consider only the first element in the variable column (`variable[1]`) to make the decision on whether to use the sum or mean function since all values of the column variable are, by definition, identical within a given station/year/variable combination.