# # Censoring data

Using a logical vector as an index is useful for limiting data that is examined. For example, to limit a vector to examine only the positive values in the data set, a logical comparison can be used for the index into the vector:

```
```r
> u <- 1:6
> v <- c(-1,1,-2,2,-3,3)
> u
[1] 1 2 3 4 5 6
> v
[1] -1  1 -2  2 -3  3
> u[v > 0]
[1] 2 4 6
> u[v < 0] = -2*u[v < 0]
> u
[1]  -2  2  -6  4 -10  6
```
```

Another useful aspect of a logical index into a vector is the use of the NA data type. The is.na function and a logical NOT operator (!) can be a useful tool when a vector contains data that is not defined:

```
```r
> v <- c(1,2,3,NA,4,NA)
> v
[1]  1  2  3 NA  4 NA
> v[is.na(v)]
[1] NA NA
> v[!is.na(v)]
[1] 1 2 3 4
```
```

Note that many functions have optional arguments to specify how R should react to data that contains a value with the NA type. Unfortunately, the way this is done is not consistent, and you should use the help command with respect to any particular function:

```
```r
> v <- c(1,2,3,NA,4,NA)
> v
[1]  1  2  3 NA  4 NA
> mean(v)
[1] NA
> mean(v,na.rm=TRUE)
[1] 2.5
```
```

In this last example, the na.rm option in the mean function is set to TRUE to specify that R should ignore the entries in the vector that are NA.