```
(a.pressure > mean_pressure + (1.5*std_pressure))
OR
(a.pressure < mean_pressure - (1.5*std_pressure))
OR
(a.age < b.mean_age - (1.5*std_age))
OR
(a.age > mean_age + (1.5*std_age))
order by agediff desc
limit 1000"
head(anomolies)
```

The output shows only one record with an age 1.5 standard deviations higher than the average. Â You can customize the cutoff standard deviation to increase or decrease the number of rows returned for the purposes of outlier inspection.

| outcome | pressure | mean_pressure | age | mean_age | agediff |
|---|---|---|---|---|---|
| 1 | 0 | 75.93101 | 70.87059 | 51.63939 | 31.19 |
| | | | | | 20.44939 |

# Changing to the SQL API

You can change to the SQL interface by coding %sql on the first line. That allows all subsequent code statements in that particular notebook cell to be interpreted as SQL only.

If you wish to return to another language in a subsequent cell, enter %r, %python, %scala, and so on as the first line of the cell. If you do not enter one of these magicÂ directives, it will be interpreted in the syntax of the default notebook type (in this case, R).

Here is the version of the count sample_bin query that we performed earlier in R, but now purely in the SQL interface. The results are shown as a pivot table. If you wish, you can manipulate the format by selecting the the raw data icon (the first one on the left, below the data), or by selecting the Plot icon (the second one after that), and choosing a specific plot:

```
%sql
```