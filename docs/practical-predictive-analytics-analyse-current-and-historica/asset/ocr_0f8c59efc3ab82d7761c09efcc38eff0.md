# Run kmeansÂ function using the kcca package to generate five cluster assignments:

```
# repeat kmeans using the kcca function. Clusters=5

clust1 = kcca(dtMatrix, k = 5, kccaFamily("kmeans"))
clust1 > kcca object of family 'kmeans'
>
> call:
> kcca(x = dtMatrix, k = 5, family = kccaFamily("kmeans"))
> cluster sizes:
>
> 1 2 3 4 5
> 360 120 152 387 8981
```

# Print the number categorized for each cluster:

```
table(clust1@cluster) >
> 1 2 3 4 5
> 360 120 152 387 8981
```

# Merge the clusters with the training data, and show some sample records displaying the cluster assigned to each:

```
kw_with_cluster2 <- as.data.frame(cbind(OnlineRetail, Cluster =
head(kw_with_cluster2) > InvoiceNo StockCode Description Quantit
> 5 536365 71053 METAL LANTERN 6
> 6 536365 21730 GLASS STAR FROSTED T-LIGHT HOLDER 6
> 2 536365 22752 SET 7 BABUSHKA NESTING BOXES 2
> 4 536365 84029E WOOLLY HOTTIE HEART. 6
> 1 536365 84406B CREAM CUPID HEARTS COAT HANGER 8
> 8 536366 22632 HAND WARMER POLKA DOT 6
> InvoiceDate UnitPrice CustomerID Country itemcount
> 5 12/1/2010 8:26 3.39 17850 United Kingdom 7
> 6 12/1/2010 8:26 4.25 17850 United Kingdom 7
> 2 12/1/2010 8:26 7.65 17850 United Kingdom 7
> 4 12/1/2010 8:26 3.39 17850 United Kingdom 7
> 1 12/1/2010 8:26 2.75 17850 United Kingdom 7
> 8 12/1/2010 8:28 1.85 17850 United Kingdom 2
> Desc2 lastword firstword Cluster
> 5 MetalLantern LANTERN METAL 5
> 6 GlassStarFrostedT-lightHolder HOLDER GLASS 5
> 2 Set7BabushkaNestingBoxes BOXES SET 5
> 4 WoollyHottieHeart. HEART. WOOLLY 5
```