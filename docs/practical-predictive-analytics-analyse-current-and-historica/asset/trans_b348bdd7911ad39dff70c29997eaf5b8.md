

```
summary(x$Not.Covered.Pct)
Min. 1st Qu. Median Mean 3rd Qu. Max.
0.009205 0.109400 0.145300 0.154200 0.210400 0.325200
```

与目标变量通常的情况一样，我们也希望查看显示变量分布的基本图表：

```
hist(x$Not.Covered.Pct)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_447_0.png)

## 保存数据

现在可能是将当前数据保存到文件的好时机，因为我们可能希望稍后从分析阶段重新读取它。这样可以避免再次读取同一文件，除非，当然，源数据发生了变化：

```
save(x, file = "x.RData")
```

## 确定所有子集组

由于我们仅查看了文件的部分内容（通过 `head()` 或 `tail()` 函数），因此不知道有多少个类别，以及它们在医疗保障方面有何差异。因此，我们将首先查看一些分组。

在以前的章节中，我们使用过 `sql()` 和 `aggregate()` 函数来分组数据。对于本示例，我们将使用 `dplyr` 包。`dplyr` 包的一个优势是它也可以与管道语法一起使用，这允许将一个函数的结果传递给下一个函数，而无需中间赋值：

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

`by.cat` 对象将显示每个类别的平均参保人数和平均总人口。请记住，此数据也按年份分组；不过，我们目前只想了解所有年份的平均值。由于 arrange 函数将按总人口规模从大到小对数据进行排序，我们可以看到数字按预期排列：

- ALL AGES 是最大的类别
- 接下来是 FEMALE ALL AGES
- 然后是 MALE ALL AGES

```
by.cat <- x %>% select(cat, Total, Total.People) %>%
group_by(cat) %>%
summarise(Avg.Total.Insured = mean(Total), Avg.People = mean(Total.People)) %>%
arrange(desc(Avg.People))
by.cat
```

作为合理性检查，如果你将后两个类别的总计相加，可以看到它们加起来等于 ALL AGES 类别。

从控制台，我们可以通过 `str(by.cat)` 函数看到有 24 个类别：

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

## 将汇总数据合并回原始数据

通常，你会希望使用之前计算得到的一些数据来增强原始数据。在这种情况下，可以使用公共键将数据合并回原始数据。同样，我们将使用 `dplyr` 包，将刚刚获得的结果（`by.cat`）连接回原始数据（`x`），使用公共键 `cat`。

我们将使用 `left_join` 仅作为示例；不过，我们也可以使用右连接来获得相同的结果，因为 `by.cat` 完全由 `x` 派生而来。连接两个数据框后，我们将得到一个名为 `x2` 的新数据框：

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

## 检查时间间隔

前面，我们提到需要有等大小的时间间隔。此外，在执行任何时间序列分析之前，我们需要检查非缺失时间间隔的数量。那么，让我们检查每个类别的注册年份数量。

使用 dplyr 包，我们可以使用 summarize (n()) 来计算每个类别的条目数量：

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

从上面可以看到，每个类别都有 14 年的数据。

因此，我们不必担心每个子集都有统一的时间段。然而，情况往往并非如此，如果你遇到这种情况，可能需要执行以下操作：

- 为缺失年份填补数据。
- 尝试转换为等间距时间序列。也许可以将时间段转换为更高的尺度。例如，对于间歇性的每日数据，尝试转换为每周、每月或每季度。
- 使用专门的时间序列技术来处理不等间距的时间序列。

## 按平均人口规模选出顶部组

在许多情况下，我们只想查看顶部类别，尤其是当有许多已细分的人口统计类别时。在本示例中，只有 24 个类别，但在其他示例中，类别数量可能要多得多。

数据框 x2 已按 Avg.People 排序。由于我们知道每个类别有 14 条注册记录，我们可以通过选择前 14*10（即 140）行来获取基于最高基础人口的前 10 个类别。我们将把这些数据存储在一个名为 x3 的新数据框中，并将其保存到磁盘。

由于我们知道每个组有 14 年，因此提取前 10 个组很容易计算。在赋值 x2 后，打印前 15 条记录，并观察类别在前 14 条记录后中断：

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

