# # Converting to a document term matrix

Once we have a corpus, we can proceed to convert it to a document term matrix. When building DTM, care must be given to limiting the amount of data and resulting terms that are processed. If not parameterized correctly, it can take a very long time to run. Parameterization is accomplished via the options. We will remove any stopwords, punctuation, and numbers. Additionally, we will only include a minimum word length of four:

```
library(tm)
dtm <- DocumentTermMatrix(corp, control = list(removePunctuation
999), stopwords = TRUE, removeNumbers = TRUE, stemming = FALSE,
Inf)))
```

We can begin to look at the data by using the `inspect()` function.

This is different from the `inspect()` function in anÃ ´ rules package, and if you have the `arules` package loaded, you will want to preface this inspect with `tm::inspect`:

```
inspect(dtm[1:10, 1:10]) > <<DocumentTermMatrix (documents: 10, t
> Non-/sparse entries: 0/100
> Sparsity : 100%
> Maximal term length: 8
> Weighting : term frequency (tf)
>
> Terms
> Docs abstract acapulco account acrylic address adult advent af
> 1 0 0 0 0 0 0 0 0 0 0
> 2 0 0 0 0 0 0 0 0 0 0
> 3 0 0 0 0 0 0 0 0 0 0
> 4 0 0 0 0 0 0 0 0 0 0
> 5 0 0 0 0 0 0 0 0 0 0
> 6 0 0 0 0 0 0 0 0 0 0
> 7 0 0 0 0 0 0 0 0 0 0
> 8 0 0 0 0 0 0 0 0 0 0
> 9 0 0 0 0 0 0 0 0 0 0
> 10 0 0 0 0 0 0 0 0 0 0
> Terms
> Docs ahoy
```