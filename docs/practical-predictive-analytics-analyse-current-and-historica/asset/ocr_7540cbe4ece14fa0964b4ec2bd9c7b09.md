```
> 3 {BOTTLE,HOLDER,MUG} 536373
> 4 {BOTTLE,HOLDER}       536375
> 5 {HOLDER}              536376
```

Another way of displaying transactions, other than using inspect, is to first coerce it to a matrix and then display the items as Boolean values. If you have many items, you will need to subset the column vectors so that they fit on the screen:

```
as(trans4, "matrix")[1:5, 1:5]
>        BAG BOTTLE BOWL BOX BUNTING
> 536365 FALSE FALSE FALSE FALSE FALSE
> 536370 FALSE FALSE FALSE TRUE  FALSE
> 536373 FALSE TRUE  FALSE FALSE FALSE
> 536375 FALSE TRUE  FALSE FALSE FALSE
> 536376 FALSE FALSE FALSE FALSE FALSE
as(trans4, "matrix")[1:5, 6:ncol(trans4)]
>          CANDLES CASES CHRISTMAS COVER HEART HOLDER  MUG SIGN
> 536365 FALSE FALSE       FALSE FALSE FALSE TRUE  FALSE FALSE
> 536370 FALSE FALSE       FALSE FALSE FALSE FALSE FALSE FALSE
> 536373 FALSE FALSE       FALSE FALSE FALSE TRUE  TRUE  FALSE
> 536375 FALSE FALSE       FALSE FALSE FALSE TRUE  FALSE FALSE
> 536376 FALSE FALSE       FALSE FALSE FALSE TRUE  FALSE FALSE
```

# Obtaining the topN purchased items

Even before we run any association rules we can obtain counts of the topN items, as shown in the first plot.

If we are looking for items with a specified support level, we can include that as a parameter to the function as well.

The second plot is another way of plotting the items, which specifies support as a filter, and it indicates higher support for bags, boxes, cases, and holders:

```
par(mfrow = c(1, 2), bg = "white", col = c("blue"))
itemFrequencyPlot(trans4, topN = 10, type = "absolute", cex.name
itemFrequencyPlot(trans4, support = 0.2, cex.names = 0.75)
```