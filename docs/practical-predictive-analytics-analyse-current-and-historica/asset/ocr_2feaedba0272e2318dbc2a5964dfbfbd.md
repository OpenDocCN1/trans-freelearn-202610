The `apriori()` function has several filtering parameters that are used to control the number of rules that are produced. In our example, we will specify the minimum support and confidence threshold that a rule needs to pass in order to be considered.

The number that you pass to apriori depends upon how you want to look at the rules. It can be an initial screening, or it can be a deeper dive, after you have performed several passes. But generally, if we want many rules we can decrease the `support` and `confidence` parameters. If we want to focus on items that appear frequently, we raise the support threshold. If we want to concentrate on higher quality and more accurate rules, we would raise the confidence level.

These numbers are not absolutes. They are numbers that you can tweak in order to limit the number of rules that you will look at, versus their quality.

The `minlen = 2` parameter is often specified in order to guarantee that there are itemsets included in the left-hand side of the rule:

```
rulesx <- apriori(trans4, parameter = list(minlen = 2, support =
> Apriori
> Parameter specification:
> confidence minval smax arem aval originalSupport support min
> 0.01 0.1 1 none FALSE TRUE 0.02
> target ext
> rules FALSE
> Algorithmic control:
> filter tree heap memopt load sort verbose
> 0.1 TRUE TRUE FALSE TRUE 2 TRUE
> Absolute minimum support count: 272
> set item appearances ...[0 item(s)] done [0.00s].
> set transactions ...[13 item(s), 13617 transaction(s)] done [0
> sorting and recoding items ... [13 item(s)] done [0.00s].
> creating transaction tree ... done [0.00s].
> checking subsets of size 1 2 3 4 5 done [0.00s].
> writing ... [829 rule(s)] done [0.00s].
> creating S4 object ... done [0.00s].
```

The output from the apriori algorithm will tell us how many rules were