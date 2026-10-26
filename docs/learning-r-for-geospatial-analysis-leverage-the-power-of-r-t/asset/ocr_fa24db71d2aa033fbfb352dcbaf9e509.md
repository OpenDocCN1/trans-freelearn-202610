Using the [ operator, we can obtain a subset of a data.frame object, which will include the intersection of any number of rows and columns. This works the same way as vectors subsetting with one difference, that is, a data.frame is a two-dimensional object while a vector is a one-dimensional object. Therefore, we need to provide two indices rather than just one.

Remember that when subsetting with the [ operator in data.frame, the first index refers to rows and the second index refers to columns. This arrangement is going to appear in other contexts as well (for example, in matrices and rasters).

The two vectors of indices, used in order to create a subset of a data.frame object, can include any combination of the following:

+   - A numeric vector, in which case the numeric vector refers to the indices of rows/columns to retain in the subset
+   - A character vector, in which case the character vector indicates the names of rows/columns to retain
+   - A logical vector, in which case the logical vector indicates whether to retain each row/column of data.frame

We have been extensively using methods 1 and 3 in vectors (see the previous chapter), so extending the methods to the two-dimensional case should be intuitive. In fact, method 2 can also be used with vectors since vectors can have element names the same way that a data.frame object has row names and column names (but we are not going to use that here).

For example, the following expression gives us the element populating the second row and the third column of df:

```
> df[2, 3]
[1] "B"
```

Leaving an empty space instead of the row's or column's index indicates we are interested in all the elements of the respective dimension (all rows or all columns). For example, the following expressions return all elements of the second row and the third column of df:

```
> df[2, ]
  num lower upper
2   2    b     B
> df[,3]
[1] "A" "B" "C"  "D"
```