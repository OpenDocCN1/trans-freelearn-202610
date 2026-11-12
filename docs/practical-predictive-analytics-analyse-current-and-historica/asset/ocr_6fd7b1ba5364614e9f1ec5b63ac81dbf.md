```
> 4 WoollyHottieHeart. HEART. WOOLLY
> 1 CreamCupidHeartsCoatHanger HANGER CREAM
> 8 HandWarmerPolkaDot DOT HAND # concatenate the Invoice Number
data2 <-- paste(OnlineRetail$InvoiceNo, OnlineRetail$Desc2, sep =

# eliminate duplicates
data2 <-- unique(data2)
#

write(data2, file = "demo_single")
file.show('demo_single') #not run
```

# Reading the transaction file back in

Use the read.transaction file to read the delimited file back into memory formatted as a transaction file. This will have the same results as coercing the dataframe to a transaction file, which we did earlier.

The difference is that the transactions are formatted one transaction per line, as specified by the format='single' option. We also specify where the TransactionID and item descriptions are via the cols option. It is possible to have multiple descriptors and transaction IDs, and identify them with the cols options. The sep keyword designates the delimiter, which in this case is the ! character. There is also a remove duplicate transactions option, which is a logical value that determines whether or not you want to eliminate duplicates.

The returned object, trans, is an itemMatrix. You can type trans to see the dimension, or run the dim(trans) to see the dimensions. This will tell you how many transactions the itemMatrix is based upon.

As before, to view the items in the trans object, use the inspect() function:

```
library(arules)
library(arulesViz)
setwd("C:/Users/randy/Desktop/ch6")

file.show('demo_single')
trans <-- read.transactions("demo_single", format = "single", sep
2), rm.duplicates = FALSE, quote = "")
trans > transactions in sparse format with
> 19403 transactions (rows) and
> 3462 items (columns) dim(trans) > [1] 19403 3462 inspect(trans
```