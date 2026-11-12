sophisticated techniques such as word clustering to try to identify which types of purchasing clusters occur and then use that to subset the association rules. To illustrate text clustering on our `OnlineRetail` dataset, we will first need to load our training and test dataframes that we previously saved. Also, issue a set.seed command since we will be doing some sampling later on:

```
setwd("C:/Users/randy/Desktop/ch6")
# load the training data
load("OnlineRetail.full.Rda") set.seed(1)
```

We previously demonstrated some text mining examples using a package called `RTextTools`. Another popular text mining package is `tm`. The `tm` package has been around for a long time, and it will be useful to know how this package works. Tm requires that all text data be converted to a corpus first. That can be done using the `VCorpus()` function. We can use vector input, since we already have the data available in an existing data frame, and there is no need to read in additional external data:

```
library(tm)
> Loading required package: NLP attach(OnlineRetail)
nrow(OnlineRetail)
> [1] 268034 corp <- VCorpus(VectorSource(OnlineRetail$Descripti
```

Displaying the `corp` object shows you some information about the metadata:

```
head(corp) > <<VCorpus>>
> Metadata: corpus specific: 0, document level (indexed): 0
> Content: documents: 6
```