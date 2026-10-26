# # Reading climatic data from a CSV file

You are now going to learn how to use dates in R using our very first real-world example. We are going to use the **comma separated values (CSV)** file named `338284.csv`, which was downloaded from the **National Oceanic and Atmospheric Administration (NOAA)** National Climatic Data Center. This file contains daily rainfall and temperature data from a meteorological station at the Albuquerque International Airport, New Mexico, from March 1, 1931 to May 15, 2014.

A CSV file is used to store plain tabular data with no additional features that are common in spreadsheet files such as XLS. This is how the file looks when opened in Excel:

![](bbox=[0.23, 0.337, 0.771, 0.632])

The following three lines of code read the file into R and assign the values in the DATE and TMAX columns to two separate vectors named time (since the data in the DATE column represents time) and tmax (which stands for maximum temperature). This involves operations on tables, which will be explained in the next chapter. They are provided here only for completeness:

```
```r
> dat = read.csv("C:\\Data\\338284.csv", stringsAsFactors = FALSE)
> time = dat$DATE
> tmax = dat$TMAX
```
```

The important point is that we now have two vectors to work with, time and tmax, as an exercise summarizing most of the topics we dealt with in this chapter.