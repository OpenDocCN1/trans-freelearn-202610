```
all_imputed_df <- complete(imp, "long", include=TRUE)
```

Run the `table()` function on the new dataframe to calculate a count of the missing values for all of the imputations, plus the original values. The original dataframe containing NAs is designated by `imp=0`.

There are five other imputations of age designated as imp 1-6:

```
table(all_imputed_df$imp,is.na(all_imputed_df$Age))
```

```
> table(all_imputed_df$imp,is.na(all_imputed_df$Age))

FALSE TRUE
0 1920 80
1 2000 0
2 2000 0
3 2000 0
4 2000 0
5 2000 0
```

To see the imputation in action, we will filter the new dataframe for one of the original IDs that contained missing values (#216).

Then we can look to see how the age value changes for each imputation of age. There can be some variation for the imputed values. For this example, it can range from a starting value of 25 up until 34:

```
all_imputed_df %>% filter(.id == 216)
>all_imputed_df %>% filter(.id == 216)

.imp .id Age Gender Education
1 0 216 NA M Some College, 1 or More Years, No Degree
2 1 216 25 M Some College, 1 or More Years, No Degree
3 2 216 29 M Some College, 1 or More Years, No Degree
4 3 216 22 M Some College, 1 or More Years, No Degree
5 4 216 34 M Some College, 1 or More Years, No Degree
6 5 216 34 M Some College, 1 or More Years, No Degree
```

Of course, that is for a single observation, if you look at the mean values for imp=1-6 that are all similar:

```
all_imputed_df %>% group_by(.imp) %>% summarize(MeanAge=mean(Age)
> all_imputed_df %>% group_by(.imp) %>% summarize (MeanAge=mean(Ag
# A tibble: 6 Å 2
.imp MeanAge
```