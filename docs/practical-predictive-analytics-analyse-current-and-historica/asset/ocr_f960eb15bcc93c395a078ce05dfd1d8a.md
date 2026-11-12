Following an initial inspection of the data, it is a good idea to look at various summary statistics of the target variable broken down by some of the categories (or factors). We could do this using SQL; however, for this example we will use a useful package called dplyr, which has syntax that is SQL-like, and it should be easy for anyone familiar with SQL and/or Linux to pick up.

One of our goals is to break down the `Total.Costs` by some of the factors to see if we can see any differences in costs among the levels. Let's start with something easy, by breaking out these `Total.Costs` by the day of the week. We will do this by piping the `df` dataframe to the `dplyr` group by command, which will then send it to a `summarize()` function that will compute the counts, means, and sums of `Total.Costs`. Finally, the `%>%` operator will send (or pipe) the output to the `View` function:

```
library(dplyr)
df %>% group_by(Admit.Day.of.Week) %>% summarize(total.count=n()),
```

| | Admit.Day.of.Week | total.count | sum(Total.Costs) | mean(Total.Costs) |
|---|---|---|---|---|
| 1 | FRI | 5805 | 258461087 | 44523.87 |
| 2 | MON | 6440 | 278940375 | 43313.72 |
| 3 | SAT | 4856 | 197503874 | 40672.13 |
| 4 | SUN | 4668 | 195987045 | 41985.23 |
| 5 | THU | 5987 | 262899265 | 43911.69 |
| 6 | TUE | 6215 | 263160240 | 42342.76 |
| 7 | WED | 6081 | 272822899 | 44864.81 |

The `Total.Costs` variable seems to be roughly equal for the different days of the week. If you are now running your project with a particular methodology, this might also be a good time to keep track of the results somewhere in one of your results folders, and note that you need to perform a statistical test, such as a t-test or an ANOVA, to test for significant differences among the days of the week.

# Standardizing data

There are a couple more transformations that we can perform on our hospital