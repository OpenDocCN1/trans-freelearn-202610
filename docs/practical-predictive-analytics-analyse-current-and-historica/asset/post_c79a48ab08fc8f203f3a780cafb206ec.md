
Print the OnlineRetail table along with the merged itemcount:

```
kable(OnlineRetail [1:5, 1:9], col.names=c("Inv#","StockCode","Desc","Quant","InvDate"))
```

Itemcount appears as the last column in this table:

| Inv# | StockCode | Desc | Quant | InvDate |
| :--- | :--- | :--- | :--- | :--- |
| 536365 | 84406B | CREAM CUPID HEARTS COAT HANGER | 8 | 12/1/201 |
| 536365 | 22752 | SET 7 BABUSHKA NESTING BOXES | 2 | 12/1/201 |
| 536365 | 85123A | HANGING HEART T-LIGHT HOLDER | 6 | 12/1/201 |
| 536365 | 84029E | WOOLLY HOTTIE HEART. | 6 | 12/1/201 |
| 536365 | 71053 | METAL LANTERN | 6 | 12/1/201 |

## Compressing descriptions using camelcase

For long descriptions, sometimes it is beneficial to compress them into camelcase to improve readability. This is especially valuable when viewing descriptions that are labels on _x_ or _y_ axes.

Camelcase is a method that some programmers use for writing compound words, where spaces are first removed, and then each word begins with a capital letter. It is also a way of conserving space.

To accomplish this, we can write a function called `.simpleCap`, which performs this function. To illustrate how it works, we will pass it a two element character vector c("A certain good book","A very easy book"), and observe the results.

## Custom function to map to camelcase

This is a simple example use of this function that maps the two character vector c("A certain good book", "A very easy book") to camelcase. This vector is mapped to two new elements:

```
[1] "ACertainGoodBook", and [2] "AVeryEasyBook"
```

```
# change descriptions to camelcase maybe append to itemnumber for
.simpleCap <- function(x) {
  # s <- strsplit(x, ' ')[[1]]
  s <- strsplit(tolower(x), " ")[[1]]

  aa <- paste(toupper(substring(s, 1, 1)), substring(s, 2), sep = "")
  gsub(" ", "", aa, fixed = TRUE)
}

a <- c("A certain good book", "A very easy book")
a4 <- gsub(" ", "", .simpleCap(a), fixed = TRUE)
a4
```

```
> [1] "ACertainGoodBook"
lapply(a, .simpleCap)
> [[1]]
> [1] "ACertainGoodBook"
>
> [[2]]
> [1] "AVeryEasyBook"
```

Let's use the .simpleCap function to create a new version of description from our OnlineRetail dataset, and call it Desc2, which removes the blanks and capitalizes the first letter of each word:

```
OnlineRetail$Desc2 <- lapply(as.character(OnlineRetail$Description), .simpleCap)
kable(OnlineRetail[1:5, c(3, 10)], padding = 0)
```

| Description | Desc2 |
|:---|:---|
| CREAM CUPID HEARTS COAT HANGER | CreamCupidHeartsCoatHanger |
| SET 7 BABUSHKA NESTING BOXES | Set7BabushkaNestingBoxes |
| WHITE HANGING HEART T-LIGHT HOLDER | WhiteHangingHeartT-lightHolder |
| WOOLLY HOTTIE WHITE HEART. | WoollyHottieWhiteHeart. |
| WHITE METAL LANTERN | WhiteMetalLantern |

## Extracting the last word

Often the first and last word of product descriptions contain useful information, and sometimes you can use a single word or phrase in place of the original longer description. This may not always be the case, but it is worth trying. In order to extract the last word from the descriptions, we can use the word function from the stringr package:

```
library(stringr)
OnlineRetail$lastword <- word(OnlineRetail$Description, -1) #sup
OnlineRetail$Description <- trimws(OnlineRetail$Description, "1")
OnlineRetail$firstword <- word(OnlineRetail$Description, 1)
```

```
# use head(OnlineRetail) if you are not using Rmarkdown
kable(OnlineRetail[1:5, c(3, 10:12)], padding = 0)
```

Firstword appears as the last column in the following output:

|Description |Desc2 |lastword|firstword|
|:---|:---|:---|:---|
|CREAM CUPID HEARTS COAT HANGER |CreamCupidHeartsCoatHanger |HANGER |CREAM |
|SET 7 BABUSHKA NESTING BOXES |Set7BabushkaNestingBoxes |BOXES |SET |
|WHITE HANGING HEART T-LIGHT HOLDER|WhiteHangingHeartT-lightHolder|HOLDER |WHITE |
|WOOLLY HOTTIE WHITE HEART. |WoollyHottieWhiteHeart. |HEART. |WOOLLY |
|WHITE METAL LANTERN |WhiteMetalLantern |LANTERN |WHITE |

In order to see if this lastword mapping makes any sense, we will sort the results so that we can see the most frequently occuring ending words. We could ultimately use this information to create category subproducts, such as cases, bags, signs, and so on:

```
kable(head(as.data.frame(sort(table(OnlineRetail$lastword[]), decreasing = TRUE)))
```

The sorted results appear. Note that this is formatted as a table, although the default output from the kable() function will appear as plain text in the console.

The lastword description looks useable (other than the large count of NAs), but we will still retain the columns as part of the analytics dataset:

|Var1 | Freq|
|:------------|----------|
| DESIGN | 25557 |
| | 24321 |
| HOLDER | 13528 |
| RETROSPOT | 13013 |
| BOX | 12939 |
| SIGN | 12210 |
| CASES | 10888 |
| BAG | 9723 |
| SET | 9056 |
| CHRISTMAS | 7868 |

## Creating the test and training datasets

Now that we are finished with our transformations, we will create the training and test data frames. We will perform a 50/50 split between training and test:

```
# Take a sample of full vector
nrow(OnlineRetail)
> [1] 536068
pctx <- round(0.5 * nrow(OnlineRetail))
set.seed(1)

# randomize rows
df <- OnlineRetail[sample(nrow(OnlineRetail)), ]
rows <- nrow(df)
OnlineRetail <- df[1:pctx, ] #training set
OnlineRetail.test <- df[(pctx + 1):rows, ] #test set
rm(df)

# Display the number of rows in the training and test datasets.
nrow(OnlineRetail)
> [1] 268034
nrow(OnlineRetail.test)
> [1] 268034
```

## Saving the results

It is a good idea to periodically save your data frames, so that you can pick up your analysis from various checkpoints.

In this example, I will first sort them both by `InvoiceNo`, and then save the test and train data sets to disk, where I can always load them back into memory as needed:

```
setwd("C:/PracticalPredictiveAnalytics/Data")
OnlineRetail <- OnlineRetail[order(OnlineRetail$InvoiceNo), ]
```

```
OnlineRetail.test <- OnlineRetail.test[order(OnlineRetail.test$InvoiceNo), ]
save(OnlineRetail,file='OnlineRetail.full.Rda')
save(OnlineRetail.test,file='OnlineRetail.test.Rda')
load('OnlineRetail.full.Rda') load('OnlineRetail.test.Rda')
nrow(OnlineRetail)
> [1] 268034
nrow(OnlineRetail.test)
> [1] 268034
nrow(OnlineRetail)
> [1] 268034
```

At this point, we have prepared our analytics data sets and are ready to move on to the actual analysis.

If you wish, you can save the entire workspace to disk as follows:

```
save.image(file = "ch6 part 1.Rdata")
```

## Loading the analytics file

If you are still in a session in which OnlineRetail is still in memory, you are OK! However, if you are picking up where we left off, you will need to load the data that we saved in the last session. Start by setting the working directory and then loading the OnlineRetail training dataset:

```
rm(list = ls())
setwd("C:/PracticalPredictiveAnalytics/Data")
load("OnlineRetail.full.Rda")
# works for small data
OnlineRetail <- OnlineRetail[1:10000,]
cat (nrow(OnlineRetail), "rows loaded\n")
> 10000 rows loaded
```

The cat function in the previous step should reflect the number of rows in the training data set, which is 268034.

## Determining the consequent rules

We have seen in the data prep stage that there are a large number of itemsets generated for each invoice. To begin to demonstrate the algorithm, we will extract one representative word from each product description, and use that word as the consequent (or rhs) to build some association rules. We have already saved the first and last words from each product description. We would examine those words more closely and see if we can filter them to result in a manageable set of transactions.

Let's first preview the frequency of the first and last word of the descriptions in descending order. That should give us a clue as to what the popular products are:

```
library(arules)
> Loading required package: Matrix
>
> Attaching package: 'arules'
>
> The following objects are masked from 'package:base':
>
>     abbreviate, write
library(arulesViz)
> Loading required package: grid
```

Print the popular first words of the description. We will do that by sorting the frequency of first words in descending order:

```
kable(head(as.data.frame(sort(table(OnlineRetail$firstword[]), decreasing = TRUE)))
```

| | |
|---|---|
| SET | 17381 |
| BAG | 8720 |
| LUNCH | 7692 |
