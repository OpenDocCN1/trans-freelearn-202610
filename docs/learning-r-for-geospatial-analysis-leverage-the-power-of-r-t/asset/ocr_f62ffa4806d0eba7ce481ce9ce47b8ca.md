As a preliminary step to construct the loop, we will define a character vector with season names (seasons) and an empty RasterStack object (season_means). The seasons vector will define loop iterations, while season_means will be used to hold the results:

```
> seasons = c("winter", "spring", "summer", "fall")
> season_means = stack()
```

Now, we are ready to execute the for loop:

```
> for(i in seasons) {
+ season_means = stack(season_means,
+ overlay(r[[which(dates$season == i)]],
+ fun = function(x) mean(x, na.rm = TRUE)))
+ }
```

The loop code section is executed four times (since the number of elements in seasons is four), each time assigning the next season name into i. The code adds the mean of the relevant layers in r (selected using the dates$season vector that we previously created; refer to Chapter 3, Working with Tables) to season_means. Therefore, in the first loop iteration, the mean of the "winter" layers in r is added to the empty season_means; in the second iteration, the mean of "spring" is added to season_means (which already has one layer), and so on until all four seasons are covered.

Now that the result season_means is ready, we can add season names to the respective layers for convenience:

```
> names(season_means) = seasons
```

We will view the result using the levelplot function:

```
> levelplot(season_means, par.settings=RdBuTheme, contour=TRUE)
```