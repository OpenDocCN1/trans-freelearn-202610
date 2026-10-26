To write a code that makes the selection, we must choose a certain criterion (either spatial or nonspatial), that separates the features of interest. In this case, for example, we can see that the pol features we wish to select are those closest to Lahav Kibbutz. Therefore, we can utilize the towns point layer (see earlier) to find the distance of each polygon from Lahav Kibbutz, and select the two most proximate ones.

Using the gDistance function (see Chapter 5, Working with Points, Lines, and Polygons), we will first find out the distances between each polygon in pol and each point in towns:

```
> dist_towns = gDistance(towns, pol, byid = TRUE)
> dist_towns
          1        2
112 14524.94060 12697.151
114  5484.66695  7529.195
137  3863.12168  5308.062
203    29.48651  1119.090
240  1910.61525  6372.634
371 11687.63594 11276.683
445 12751.21123 14371.268
460 14860.25487 12300.319
```

The returned matrix, named dist_towns, contains the pairwise distances, with rows corresponding to the pol feature and columns corresponding to the towns feature. Since Lahav Kibbutz corresponds to the first towns feature (column "1"), we can already see that the fourth and fifth pol features (rows "203" and "240") are the most proximate ones, thus corresponding to the Lahav and Kramim forests. We could subset both forests by simply using their IDs—pol[c("203","240"),]. However, as always, we are looking for general code that will select, in this case, the two closest features irrespective of the specific IDs or row indices. For this purpose, we can use the order function, which we have not encountered so far. This function, given a numeric vector, returns the element indices in an increasing order according to element values. For example, applying order to the first column of dist_towns, we can see that the smallest element in this column is in the fourth row, the second smallest is in the fifth row, the third smallest is in the third row, and so on:

```
> dist_order = order(dist_towns[, 1])
> dist_order
[1] 4 5 3 2 6 7 1 8
```