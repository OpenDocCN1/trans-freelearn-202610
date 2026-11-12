
![](img/672c0cf8ba3c000a6529b117ed96945e_347_0.png)

We can also specify a variable importance plot by using the plot statement with `type='s'`:

```
plot(glmulti.coxph.out,type = "s")
```

This plot gives a nice simple horizontal barchart ordered by the most important features:

## Model-averaged importance of terms

![](img/672c0cf8ba3c000a6529b117ed96945e_348_0.png)

## Summary

In this chapter, we learned about what survival analysis is, and how two main techniques, Kaplan-Meier and Cox Regression, can be used to explain and predict customer churn.

We also learned how we can generate our own data to test assumptions and test the robustness of the models.

Finally, we included some coding techniques to help us reproduce and save our generated code and images.

In the next chapter, we will not be concerned with a customer leaving, but will cover how to keep customers happy by predicting what they will purchase next using a technique known as **Market Basket Analysis**.

## Chapter 7. Using Market Basket Analysis as a Recommender Engine

> "It's not wise to violate the rules until you know how to observe them."

- T.S. Eliot

In this chapter, we will cover the following topics:

- Market basket analysis using the arules package
- Data transformation and cleaning techniques using semi-structured market basket transaction data
- Learn how to transform transaction objects into dataframes
- Use cluster analysis for prediction using the flexclus package
- Utilize some text mining using RTextTools and tm packages

### What is market basket analysis?

If you have survived the last chapter, you will now be introduced to the world of **market basket analysis (MBA)**. Market basket analysis (also sometimes called **affinity analysis**), is a predictive analytics technique that is used heavily in the retail industry in order to identify baskets of items that are purchased together. The typical use case for this is the supermarket shopping cart in which a shopper would typically purchase an assortment of items such as milk, bread, cheese, and so on, and the algorithm will predict how purchasing certain items together will affect the purchase of other items. It is one of those methods that retailers use to know to start sending you coupons and emails for things that you didn't know you needed!

One often quoted example of MBA is the relationship between diapers and beer:

> "One supermarket chain discovered in its analysis that customers that bought diapers often bought beer as well, have put the diapers close to beer coolers, and their sales increased dramatically" - http://en.wikipedia.org/wiki/Market_basket

However, it is not only restricted to the retail industry. MBA can be used in the insurance industry to look at the various products that an insured person currently has, such as a car, home, and so on, and suggest other possible products such as life, disability, or investment products.

MBA is generally considered an unsupervised learning algorithm, in that target variables are usually not specified. However, as you will see later it is possible to refine the association rules, so that specific items can be specified as target variables.

MBA is also considered a type of recommender engine in which purchases of a set of items imply the purchase of others. Certainly, MBA and other recommender engines can share the same types of input data. However, MBA was developed before the advent of collaborative filtering techniques, as pioneered by Amazon, and is more suggestive of the integration of collected web data, while MBA is more associated with the RFID bar coding technologies found in scanners. However, in both cases suggestions of future purchases based on past purchases is the goal.

### Examining the groceries transaction file

Critical to the understanding of MBA are the concepts of support, confidence, and lift. These are the measures that evaluated the _goodness of fit_ for a set of association rules. You will also learn some specific definitions that are used in MBA, such as consequence, antecedent, and itemsets.

To introduce these concepts, we will first illustrate these terms through a very simplistic example. We will use only the first 10 transactions contained in the Groceries transaction file, which is contained in the `arules` package:

```
library(arules)
```

After the `arules` library is loaded, you can see a short description of the Groceries dataset by entering `?Groceries` at the command line. The following description appears in the `help` window:

> "The Groceries data set contains 1 month (30 days) of real-world point-of-sale transaction data from a typical local grocery outlet. The data set contains 9835 transactions and the items are aggregated to 169 categories".

For more information about how this dataset was collected, refer to the original publication (Michael Hahsler, 2006).

Once the `arules` package is loaded, load the `Groceries` dataset into memory:

```
data(Groceries)
```

### Format of the groceries transaction Files

The `Groceries` is a transaction class object, not a dataframe. This R object represents transaction data used for mining item sets or rules. Logically, it is organized as a list of grocery receipts along with the items that were purchased together. Every individual line is referred to as a transaction and every column of the transaction represents a specific item purchased.

For example, here are three transactions consisting of a varying number of purchases:

Transaction 1 Milk Cereal

Transaction 2 Beef

Transaction 3 ButterSugar Cream

However, a transaction object is not physically organized in strict database table format. It is in a special R object format known as transactions.

You can also run a `summary(Groceries)`, which will give you some high level information about the structure of the transactions file, as well as the most frequent individual items found in the market basket:

```
> summary(Groceries)
transactions as itemMatrix in sparse format with
9835 rows (elements/itemsets/transactions) and
169 columns (items) and a density of 0.026

most frequent items:
whole milk other vegetables rolls/buns soda yogurt (Other)
2513 1903 1809 1715 | 1372 34055
```

You will see later on how we can convert dataframes to transaction objects, and vice versa. For more information about this object, you can enter ?transactionInfo at the console line.

To see an example of a simple market basket for the `Groceries` file, run the following code:

Examine the output produced from the `inspect()` function, which prints transactions 10-19 from the market basket. Note that each transaction can consist of a single transaction ID, along with a list of purchased items. It can be a single purchased item (Transaction #4, Beef purchased by itself), or consist of multiple purchases (Transaction #1, Milk and Cereal):

```
inspect(Groceries[10:19])
```

This is the following output:

```
items
[1] {whole milk,cereals}
[2] {tropical fruit,other vegetables,white bread,bottled water,ch
[3] {citrus fruit,tropical fruit,whole milk,butter,curd,yogurt,flour,bottled water,dishes}
[4] {beef}
[5] {frankfurter,rolls/buns,soda}
[6] {chicken,tropical fruit}
[7] {butter,sugar,fruit/vegetable juice,newspapers}
[8] {fruit/vegetable juice}
[9] {packaged fruit/vegetables}
[10]{chocolate}
```

### The sample market basket

Each transaction numbered 1-10 listed previously represents a basket of items purchased by a shopper. These are typically all items that are associated with a particular transaction or invoice. Each basket is enclosed within braces {}, and is referred to as an itemset. An itemset is a group of items that occur together.

Market basket algorithms construct rules in the form of:

```
Itemset{x1,x2,x3 ...} --> Itemset{y1,y2,y3...}.
```

This notation states that buyers who have purchased items on the left-hand side of the formula (lhs) have a propensity to purchase items on the right-hand side (rhs). The association is stated using the => symbol, which can be interpreted as implies.

### Note

The lhs of the notation is also known as the antecedent, and the rhs is known as the consequence. If nothing appears on either the left-hand side or right-hand side there is no specific association rule for those items; however, it also means that those items have appeared in the basket.
