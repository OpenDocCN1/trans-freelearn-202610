# Working with Tables

Alternatively, we can have the months form new columns, as follows:

```
> dat2 = dcast(dat_melt, station+station_name+variable+year~month)
> head(dat2)
        station      station_name variable year    1    2    3
1 GHCND:SP000003195 MADRID RETIRO SP     tcpc 1984 25.3 37.2 58.0
2 GHCND:SP000003195 MADRID RETIRO SP     tcpc 1985 67.8 45.1  6.0
3 GHCND:SP000003195 MADRID RETIRO SP     tcpc 1986 10.6 57.0 22.2
4 GHCND:SP000003195 MADRID RETIRO SP     tcpc 1987 93.3 42.9  6.7
5 GHCND:SP000003195 MADRID RETIRO SP     tcpc 1988 60.0 20.9  1.1
6 GHCND:SP000003195 MADRID RETIRO SP     tcpc 1989  9.9 19.5 23.8

             4    5    6    7    8    9   10   11   12
1         39.2 82.4 35.6  0.0  7.1  7.1 30.1 161.0 11.3
2         36.8 29.9 24.1  5.4  0.0  0.0  0.0  39.9 83.8
3         57.3 12.0  1.0 37.1 16.4 47.5 93.8  13.6 19.0
4         63.1 58.4  7.8 43.9 14.5 11.9 58.5  64.8 79.1
5         96.6 46.5 49.8  9.5  0.0  0.0 79.2  50.1  0.2
6         52.2 97.7 12.7 13.3  3.2 37.5  5.9 146.4 138.6
```

> Note that this time we omitted some of the variables from the resulting table (elevation, latitude, and so on), and made the months' levels (there are 12 of these) appear in separate columns, with the values of the respective climatic variable for each month in the cells of the given column. This form is ideal to answer questions such as which month is the warmest in each station (using apply, for example).

The casting operations we have performed so far involved the retention of all original data in the resulting table. What happens when we instruct the creation of a table that cannot contain all of our original data? In this case, aggregation takes place and we need to specify the function that will be used for aggregation (otherwise, the default function length will be used). For example, we can calculate the mean climatic conditions in Spain per year, as follows:

```
> dat2 = dcast(dat_melt, year ~ variable, mean, na.rm = TRUE)
> head(dat2)
   year       tcpc      tmxt      mmnt
1  1984 54.34180 19.38194  9.381115
2  1985 45.13103 20.31096  9.793890
3  1986 47.55329 19.75327  9.681250
4  1987 57.09826 20.31684 10.350206
5  1988 47.78863 20.07662  9.934514
6  1989 54.82944 20.97615 10.654617
```