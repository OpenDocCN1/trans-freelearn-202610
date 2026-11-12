
| | lm.coef | cat | coef.rank |
|---|---|---|---|
| structure.c..0.406183442737828..0.0003367987... | 0.0003367988 | 18 to 24 YEARS | 06 - 18 to 24 YEARS |
| structure.c..11.3751875969696..0.00579618220... | 0.0057961822 | 25 to 34 YEARS | 23 - 25 to 34 YEARS |
| structure.c..10.9168220838485..0.00553403690... | 0.0055340369 | 35 to 44 YEARS | 21 - 35 to 44 YEARS |
| structure.c..11.5445664482143..0.00582919407... | 0.0058291941 | 45 to 54 YEARS | 24 - 45 to 54 YEARS |
| structure.c..4.70961214602917..0.00240990753... | 0.0024099075 | 55 to 64 YEARS | 14 - 55 to 64 YEARS |
| structure.c..1.25623750951236..0.00063341249... | 0.0006334125 | 65 YEARS AND OVER | 07 - 65 YEARS AND OVER |
| structure.c..4.10262144273963..0.00211905813... | 0.0021190581 | ALL AGES | 12 - ALL AGES |
| structure.c..2.67730000270634..0.00145538791... | 0.0014553879 | FEMALE 18 to 24 YEARS | 09 - FEMALE 18 to 24 YEARS |
| structure.c..9.99097876897398..0.00508800858... | 0.0050880086 | FEMALE 25 to 34 YEARS | 19 - FEMALE 25 to 34 YEARS |
| structure.c..9.5647240414497..0.004850187645... | 0.0048501876 | FEMALE 35 to 44 YEARS | 17 - FEMALE 35 to 44 YEARS |
| structure.c..10.3633655124299..0.00523536960... | 0.0052353696 | FEMALE 45 to 54 YEARS | 20 - FEMALE 45 to 54 YEARS |
| structure.c..4.10277495735534..0.00210834349... | 0.0021083435 | FEMALE 55 to 64 YEARS | 11 - FEMALE 55 to 64 YEARS |
| structure.c..1.36745107793605..0.00068877425... | 0.0006887743 | FEMALE 65 YEARS AND OVER | 08 - FEMALE 65 YEARS AND OVER |
| structure.c..3.81748382391257..0.00197005880... | 0.0019700588 | FEMALE ALL AGES | 10 - FEMALE ALL AGES |
| structure.c..3.26759338613744..0.00157832779... | -0.0015783278 | FEMALE UNDER 18 YEARS | 02 - FEMALE UNDER 18 YEARS |
| structure.c..4.03612772673809..0.00186299125... | -0.0018629913 | MALE 18 to 24 YEARS | 01 - MALE 18 to 24 YEARS |
| structure.c..9.71595028579057..0.00498362081... | 0.0049836208 | MALE 25 to 34 YEARS | 18 - MALE 25 to 34 YEARS |
| structure.c..7.70662482144783..0.00394154306... | 0.0039415431 | MALE 35 to 44 YEARS | 16 - MALE 35 to 44 YEARS |
| structure.c..10.9753879171274..0.00554925524... | 0.0055492552 | MALE 45 to 54 YEARS | 22 - MALE 45 to 54 YEARS |
| structure.c..5.37038054369785..0.00273826914... | 0.0027382691 | MALE 55 to 64 YEARS | 15 - MALE 55 to 64 YEARS |

## Merging scores back into the original dataframe

We will augment the original x2 dataframe with this new information by merging back by category, and then by sorting the dataframe by the rank of the coefficient. This will allow us to use this as a proxy for trend:

```
x2x <- x2 %>% left_join(xx4, by = "cat") %>% arrange(coef.rank, c
```

```
# exclude some columns so as to fit on one page
head(x2x[, c(-2, -3, -4, -8)])
> Source: local data frame [6 x 7]
>
> cat Year.1 Total.People Total Not.Covered.
> (fctr) (int) (dbl) (dbl) (d
> 1 MALE 18 to 24 YEARS 2012 15142.04 11091.86 0.2674
> 2 MALE 18 to 24 YEARS 2011 15159.87 11028.75 0.2725
> 3 MALE 18 to 24 YEARS 2010 14986.02 10646.88 0.2895
> 4 MALE 18 to 24 YEARS 2010 14837.14 10109.82 0.3186
> 5 MALE 18 to 24 YEARS 2008 14508.04 10021.66 0.3092
> 6 MALE 18 to 24 YEARS 2007 14391.92 10230.61 0.2891
> Variables not shown: lm.coef (dbl), coef.rank (fctr)
```

## Plotting the data with the trend lines

Now that we have the trend coefficients, we will use ggplot to first plot enrollment for all of the 24 categories, and then create a second set of plots which adds the trend line based upon the linear coefficients we have just calculated.

Code notes: facet_wrap will order the plots by the value of variable z, which was assigned to the coefficient rank. Thus, we can get to see the categories with declining enrollment first, ending with the categories having the highest trend in enrollment from the period 1999-2012.

### Note

I like to assign the variables that I will be changing to standard variable names, such as x, y, and z, so that I can remember their usage (for example, variable x is always the x variable, and y always the x variable). But you can supply the variable names directly in the call to ggplot, or set up your own function to do the same thing:

```
library(ggplot2)
.df <- data.frame(x = x2x$Year.1, y = x2x$Not.Covered.Pct, z = x2
#use ggplot to layer the different components of the visualization
.plot <- ggplot(data = .df, aes(x = x, y = y, colour = 1, shape =
+ geom_point()
+ scale_shape_manual(values = seq(0, 24))
+ scale_y_continuous(expand = c(0.1,0))
+ scale_x_continuous(expand = c(0.1, 1))
+ facet_wrap(~z) + xlab("Year.1")
+ ylab("Not.Covered.Pct")
+ labs(colour = "cat", shape = "cat")
+ theme(panel.margin = unit(0.3,"points"), legend.position = "non
+ theme(strip.text.x = element_text(size = 6))
print(.plot)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_469_0.png)

As you can see, for the ALL AGES category, the non-covered percentage is seen to be increasing over the time period, until 2010 (where the affordable care act was enacted), and then the percentage begins to decrease. However, for the same time period, the under 18 age group shows a decrease in the proportion of non-insured relative to the population size.

We can take a closer look at the top and bottom four categories, along with the ALL AGES category, to examine this more closely. This time, we will add our own trend line using the geom_smooth parameter, which will add a linear regression trend line:

```
# declining enrollment
.df2 <- rbind(head(.df, (4*14)) , tail(.df, (4*14)) , .df[.df$z == "1
.plot2 <- ggplot(data = .df2, aes(x = x, y = y, colour = 1, shape
+ geom_point()
+ scale_shape_manual(values = seq(0, nrow(.df2)))
+ scale_y_continuous(expand = c(0.1, 0))
+ scale_x_continuous(expand = c(0.1, 1))
+ facet_wrap(~z)
+ xlab("Year.1")
+ ylab("Not.Covered.Pct")
+ labs(colour = "cat", shape = "cat")
+ theme(panel.margin = unit(0.3,"points"),legend.position = "none
+ geom_smooth(method = "lm", se = FALSE,colour = "red")
.plot2 <- ggplot(data = .df2, aes(x = x, y = y, colour = 1, shape
+ geom_point()
+ scale_shape_manual(values = seq(0, nrow(.df2)))
+ scale_y_continuous(expand = c(0.1,0))
+ scale_x_continuous(expand = c(0.1, 1))
+ facet_wrap(~z)
+ xlab("Year.1")
+ ylab("Not.Covered.Pct")
+ labs(colour = "cat", shape = "cat")
+ theme(panel.margin = unit(0.3,"points"),legend.position = "none
+ geom_smooth(method = "lm", se = FALSE,colour = "red")
print(.plot2)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_470_0.png)

## Plotting all the categories on one graph

Sometimes, it's nice to plot all of the lines on one graph rather having them as separate plots. To achieve this, we will alter the syntax a bit, so that the categories show up as stacked lines. Again, we can see the percentage of uninsured aligns across ages, with the under 18 group having the lowest uninsured rate, and the 25-54 group having the highest:

```
library(ggplot2)
### plot all on one graph

.df <- x3[order(x3$Year.1), ]
.plot <- ggplot(data = .df, aes(x = Year.1, y = Not.Covered.Pct,
+ geom_point()
+ geom_line(size = 1)
+ scale_shape_manual(values = seq(0,15))
+ ylab("Not.Covered.Pct")
+ labs(colour = "cat", shape = "cat")
+ theme_bw(base_size = 14,base_family = "serif")
print(.plot)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_472_0.png)

## Adding labels

It can sometimes be a bit difficult to discern the differences between the categories based on the legend, especially as the number of categories increases, so we can use the directlabels library to mark each line with the category name. Now we can see, for example, that ALL MALES has a higher uninsured rate than ALL FEMALES over all time periods:

```
#install.packages("directlabels")
library(directlabels)
direct.label(.plot, list(last.points, hjust = 0.75, cex = 0.75, v
```

Not.Covered.Pct

Year.1

- 25 TO 34 YEARS
- 35 TO 44 YEARS
- 45 to 54 YEARS
- MALE ALL AGES
- ALL AGES
- FEMALE ALL AGES
- FEMALE UNDER 19 YEARS
- 65 YEARS AND OVER

![](img/672c0cf8ba3c000a6529b117ed96945e_473_0.png)
