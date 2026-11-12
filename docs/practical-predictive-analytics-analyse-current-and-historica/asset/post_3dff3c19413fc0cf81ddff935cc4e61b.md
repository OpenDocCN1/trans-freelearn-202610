
```
summary(x$Not.Covered.Pct)
Min. 1st Qu. Median Mean 3rd Qu. Max.
0.009205 0.109400 0.145300 0.154200 0.210400 0.325200
```

As typical with a target variable, we also like to see basic plots showing the distribution of the variable:

```
hist(x$Not.Covered.Pct)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_447_0.png)

## Saving the data

This may be a good time to save the current data to a file, since we may want to read it back later, starting from the analysis stage. This will avoid having to reread the same file again unless, of course, the source data changes:

```
save(x, file = "x.RData")
```

## Determining all of the subset groups

Since we have only looked at parts of the file (via `head()` or `tail()` functions), we do not know how many categories there are and how they differ in terms of health care coverage. So we will start off by looking at some of the groupings.

In previous chapters, we have used `sql()` and the `aggregate()` function to group data. For this example, we will use the `dplyr` package. One advantage of the `dplyr` package is that it can also be used with pipe syntax, which allows the result of one function to be passed to the next function without intermediate assignments:

```
library(dplyr)
>
> Attaching package: 'dplyr'
> The following objects are masked from 'package:stats':
>
>     filter, lag
> The following objects are masked from 'package:base':
>
>     intersect, setdiff, setequal, union
# str(x)
```

The `by.cat` object will show the average number insured, and the average total population for each category. Remember, this data is also grouped by year; however, we just want to get a sense of what the averages are across all of the years for now. Since the arrange function will end up sorting the data by the total population sizes, from largest to smallest, we can see that the numbers line up as expected:

- ALL AGES is the largest category
- This is followed by FEMALE ALL AGES
- Then MALE ALL AGES

```
by.cat <- x %>% select(cat, Total, Total.People) %>%
group_by(cat) %>%
summarise(Avg.Total.Insured = mean(Total), Avg.People = mean(Total.People)) %>%
arrange(desc(Avg.People))
by.cat
```

As a sanity check, if you add the totals for the latter two categories, you can see that they sum to the ALL AGES category.

From the console, we can see from the str(by.cat) function that there are 24 categories:

```
str(by.cat)
> Classes 'tbl_df', 'tbl' and 'data.frame': 24 obs. of  3 variables
> $ cat : Factor w/ 24 levels "18 to 24 YEARS",...:
> $ Avg.Total.Insured: num 251243 130201 121042 66200 34762 ..
> $ Avg.People : num 294700 150330 144371 73752 42433 ..
by.cat
> Source: local data frame [24 x 3]
>
>
> cat Avg.Total.Insured Avg.People
> (fctr) (dbl) (dbl)
> 1 ALL AGES 251242.96 294700.47
> 2 FEMALE ALL AGES 130200.90 150329.73
> 3 MALE ALL AGES 121042.06 144370.74
> 4 UNDER 18 YEARS 66200.46 73752.50
> 5 35 to 44 YEARS 34761.74 42433.12
> 6 45 to 54 YEARS 35911.82 42100.20
> 7 25 to 34 YEARS 29973.91 39942.64
> 8 MALE UNDER 18 YEARS 33832.87 37700.70
> 9 65 YEARS AND OVER 36199.32 36722.61
> 10 FEMALE UNDER 18 YEARS 32367.59 36051.79
> .. ... ... ...
```

## Merging the aggregate data back into the original data

Often, you will want to augment your original data with some of the calculated data as derived previously. In these cases, you can merge the data back into the original data using a common key. Again, we will use the `dplyr` package to take the results just obtained (`by.cat`) and join them back to the original data (`x`), using the common key `cat`.

We will be using a `left_join` just for an example; however, we could have used a right join to obtain the same results, since `by.cat` was completely derived from `x`. After joining the two dataframes, we will end up with a new dataframe named `x2`:

```
# Merge the summary measures back into the original data. Merge
x2 <- by.cat %>% left_join(x, by = "cat")
head(x2)
```

```
> Source: local data frame [6 x 9]
>
>      cat Avg.Total.Insured Avg.People   Year Year.1 Total
>    (fctr)           (dbl)     (dbl)  (fctr)  (int)
> 1  ALL AGES        251243   294700.5    2012   2012     3
> 2  ALL AGES        251243   294700.5    2011   2011     3
> 3  ALL AGES        251243   294700.5   2010   2010     3
> 4  ALL AGES        251243   294700.5    2009   2009     3
> 5  ALL AGES        251243   294700.5    2008   2008     3
> 6  ALL AGES        251243   294700.5    2007   2007     2
> Variables not shown: Total (dbl), Not.Covered (dbl), Not.Cover
```

## Checking the time intervals

Earlier, we mentioned needing to have equally sized time intervals. Additionally, before we perform any time series analysis, we need to check for the number of non-missing time intervals. So, let's check the number of enrollment years for each category.

Using the dplyr package, we can use summarize (n()) to count the number of entries for each category:

```
# -- summarize and sort by the number of years
yr.count <- x2 %>% group_by(cat) %>% summarise(n = n()) %>% arrange
# - we can see that there are 14 years for all of the groups. Th
print(yr.count, 10)
```

| cat | n |
|:---|---:|
| 18 to 24 YEARS | 14 |
| 25 to 34 YEARS | 14 |
| 35 to 44 YEARS | 14 |
| 45 to 54 YEARS | 14 |
| 55 to 64 YEARS | 14 |
| 65 YEARS AND OVER | 14 |
| ALL AGES | 14 |
| FEMALE 18 to 24 YEARS | 14 |
| FEMALE 25 to 34 YEARS | 14 |
| FEMALE 35 to 44 YEARS | 14 |

We can see from the above that every category has 14 years of data represented.

So, we don't have to worry about having a uniform time period for each subset. However, this is often not the case, and if you come across this, you may need to do the following:

- Impute data for years that are missing.
- Try to convert to equally spaced time series. Perhaps transform time period to a higher scale. For example for intermittent daily data, try to convert to weekly, monthly or quarterly.
- Use specialized time series techniques to account for unequally spaced time series.

## Picking out the top groups in terms of average population size

In many instances, we will only want to look at the top categories, especially when there are many demographical categories that have been subsetted. In this example, there are only 24 categories but in other examples, there may be a much larger number of categories.

The dataframe x2 is already sorted by Avg.People. Since we know that there are 14 enrollment records for each category, we can get the top 10 categories based upon the highest base population by selecting the first 14*10 (or 140) rows. We will store this in a new dataframe, x3, and save this to disk.

Since we know each group has 14 years, extracting the top 10 groups is easy to calculate. After assigning x2, print the first 15 records and observe that the category break after the first 14 records:

```
x3 <- x2[1:(14 * 10), ]
head(x3, 15)
```

```
cat Avg.Total.Insured Avg.People Total Year Year.1 Total.People Total
<fctr> <dbl> <dbl> <dbl> <fctr> <int> <dbl> <dbl> <dbl>
1 ALL AGES 251243.0 294700.5 2012 2012 311116.2 263165.5 47950.68
2 ALL AGES 251243.0 294700.5 2011 2011 308827.2 260213.8 48613.46
3 ALL AGES 251243.0 294700.5 2010 2010 306553.2 256602.7 499
4 ALL AGES 251243.0 294700.5 2009 2009 304279.9 255295.1 48984.82
5 ALL AGES 251243.0 294700.5 2008 2008 301482.8 256702.4 44780.40
6 ALL AGES 251243.0 294700.5 2007 2007 299105.7 255017.5 44088.18
7 ALL AGES 251243.0 294700.5 2006 2006 296824.0 251609.6 45214.35
8 ALL AGES 251243.0 294700.5 2005 2005 293834.4 250799.4 43034.92
9 ALL AGES 251243.0 294700.5 2004 2004 291166.2 249413.9 4175
10 ALL AGES 251243.0 294700.5 2003 2003 288280.5 246331.7 41948.7
11 ALL AGES 251243.0 294700.5 2002 2002 285933.4 246157.5 39775.9
12 ALL AGES 251243.0 294700.5 2001 2001 282082.0 244058.6 38023.3
13 ALL AGES 251243.0 294700.5 2000 2000 279517.4 242931.5 365
14 ALL AGES 251243.0 294700.5 1999 1999 276803.8 239102.0 377
15 FEMALE ALL AGES 130200.9 150329.7 2012 2012 158780.9 136315.1
#
save(x3, file = "x3.RData")
```
