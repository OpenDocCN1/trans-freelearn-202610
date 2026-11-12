# Compressing descriptions using camelcase

For long descriptions, sometimes it is beneficial to compress them into camelcase to improve readability. This is especially valuable when viewing descriptions that are labels on _x_ or _y_ axes.

Camelcase is a method that some programmers use for writing compound words, where spaces are first removed, and then each word begins with a capital letter. It is also a way of conserving space.

To accomplish this, we can write a function called `.simpleCap`, which performs this function. To illustrate how it works, we will pass it a two element character vector c("A certain good book","A very easy book"), and observe the results.

# Custom function to map to camelcase

This is a simple example use of this function that maps the two character vector c("A certain good book", "A very easy book") to camelcase. This vector is mapped to two new elements:

```
[1] "ACertainGoodBook", and [2] "AVeryEasyBook"
```

```
# change descriptions to camelcase maybe append to itemnumber for
.simpleCap <- function(x) {
  # s <- strsplit(x, ' ')[[1]]
  s <- strsplit(tolower(x), " ")[[1]]

  aa <- paste(toupper(substring(s, 1, 1)), substring(s, 2), se
gsub(" ", "", aa, fixed = TRUE)
}

a <- c("A certain good book", "A very easy book")
a4 <- gsub(" ", "", .simpleCap(a), fixed = TRUE)
a4
```