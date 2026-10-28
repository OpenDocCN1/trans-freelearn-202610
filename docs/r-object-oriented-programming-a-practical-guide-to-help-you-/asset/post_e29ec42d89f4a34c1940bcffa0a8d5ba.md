
## Data frames

A data frame is similar to a list, and many of the operations are similar. The primary difference is that all of the components of a data frame must have the same number of elements. This is one of the most common ways to store information, and many of the functions available to read data from a file return a data frame by default. For example, suppose we ask five people two questions. The first question is, “Do you have a pet cat?” The second question is, “How many rooms in your house need new carpet?”

```
```r
> d <- data.frame(Q1=as.factor(c("y","n","y","y","n")),
+                 Q2=c(2,0,1,2,0))
> d
  Q1 Q2
1  y  2
2  n  0
3  y  1
4  y  2
5  n  0

> d$Q1
[1] y n y y n
Levels: n y

> summary(d)
   Q1             Q2
 n:2   Min.   :0
 y:3   1st Qu.:0
       Median :1
       Mean   :1
       3rd Qu.:2
       Max.   :2
```
```

The names and attributes commands have the same behaviors with data frames as lists. In the preceding example, we take the data frame defined in the previous example and rename the fields to something more descriptive:

```
```r
> d <- data.frame(Q1=as.factor(c("y","n","y","y","n")),
+                 Q2=c(2,0,1,2,0))
> names(d) <- c("HaveCat","NumberRooms")
> d
  HaveCat NumberRooms
1       y           2
2       n           0
3       y           1
4       y           2
5       n           0
```
```

number. The cut command takes the numeric data and a vector of break points that indicate the cutoff points between each interval, as follows:

```
```r
> a <- c(-0.8,-0.7,0.9,-1.4,-0.3,1.2)
> b <- cut(a,breaks=c(-1.5,-1,-0.5,0,0.5,1.0,1.5))
> b
[1] (-1,-0.5] (-1,-0.5] (0.5,1]   (-1.5,-1] (-0.5,0]  (1,1.5]
Levels: (-1.5,-1] (-1,-0.5] (-0.5,0] (0,0.5] (0.5,1] (1,1.5]
> summary(b)
(-1.5,-1] (-1,-0.5] (-0.5,0]   (0,0.5]   (0.5,1]   (1,1.5]
         1          2          1          0          1          1
> table(b)
b
(-1.5,-1] (-1,-0.5] (-0.5,0]   (0,0.5]   (0.5,1]   (1,1.5]
         1          2          1          0          1          1
```
```

## Matrices and arrays

Tables are a special case of an array. An array or a matrix can be constructed directly using either the `array` or `matrix` commands. The `array` command takes a vector and dimensions, and it constructs an array using column major order. If you wish to provide the data in row major order, then the command to transpose the result is `t`.

```
```r
> a <- c(1,2,3,4,5,6)
> A <- array(a,c(2,3))
> A
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6
> t(A)
     [,1] [,2]
[1,]    1    2
[2,]    3    4
[3,]    5    6
```
```

You are not limited to two-dimensional arrays. The dim option can include any number of dimensions. In the following example, a three-dimensional array is created by using three numbers for the number of dimensions:

```
```r
> A <- array(1:24,c(2,3,4),dimnames=c("row","col","dep"))
> A
, , 1

     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

, , 2

     [,1] [,2] [,3]
[1,]    7    9   11
[2,]    8   10   12

, , 3

     [,1] [,2] [,3]
[1,]   13   15   17
[2,]   14   16   18

, , 4

     [,1] [,2] [,3]
[1,]   19   21   23
[2,]   20   22   24

> A[2,3,4]
[1] 24
```
```

A matrix is a two-dimensional array and is a special case that can be created using the `matrix` command. Rather than using the dimensions, the `matrix` command requires that you specify the number of rows or columns. The command has an additional option to

specify whether or not the data is in row major or column major order:

```
```r
> B <- matrix(1:12,nrow=3)
> B
     [,1] [,2] [,3] [,4]
[1,]    1    4    7   10
[2,]    2    5    8   11
[3,]    3    6    9   12

> B <- matrix(1:12,nrow=3,byrow=TRUE)
> B
     [,1] [,2] [,3] [,4]
[1,]    1    2    3    4
[2,]    5    6    7    8
[3,]    9   10   11   12
```
```

Both matrices and arrays can be manipulated to determine or change their dimensions. The dim command can be used to get or set this information:

```
```r
> C <- matrix(1:12,ncol=3)
> C
     [,1] [,2] [,3]
[1,]    1    5    9
[2,]    2    6   10
[3,]    3    7   11
[4,]    4    8   12
> dim(C)
[1] 4 3
> dim(C) <- c(3,4)
> C
     [,1] [,2] [,3] [,4]
[1,]    1    4    7   10
[2,]    2    5    8   11
[3,]    3    6    9   12
```
```

## Censoring data

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

## Appending rows and columns

The `cbind` and `rbind` commands can be used to append data to existing objects. These commands can be used on vectors, matrices, arrays, and they are extended to also act on data frames. The following examples use data frames, as that is a common operation. You should be careful and try the commands on arrays to make sure that the operation behaves in the way you expect.

The `cbind` command is used to combine the columns of the data given as arguments:

```
```r
> d <- data.frame(one=c(1,2,3),two=as.factor(c("one","two","three")))
> e <- c("ein","zwei","drei")
> newDataFrame <- cbind(d,third=e)
> newDataFrame
  one   two third
1   1   one   ein
2   2   two  zwei
3   3 three  drei
> newDataFrame$third
[1] ein zwei drei
Levels: drei ein zwei
```
```

If the arguments to the `cbind` command are two data frames (or two arrays), then the command combines all of the columns from all of the data frames (arrays):

```
```r
> d <- data.frame(one=c(1,2,3),two=as.factor(c("one","two","three")))
> e <- data.frame(three=c(4,5,6),four=as.factor(c("vier","funf","sechs")))
> newDataFrame <- cbind(d,e)
> newDataFrame
  one   two three  four
1   1   one     4  vier
2   2   two     5  funf
3   3 three     6 sechs
```
```

The `rbind` command concatenates the rows of the objects passed to it. The command uses the names of the columns to determine how to append the data. The number and names of the columns must be identical:

```
```r
> d <- data.frame(one=c(1,2,3),two=as.factor(c("one","two","three")))
> e <- data.frame(one=c(4,5,6),two=as.factor(c("vier","funf","sechs")))
> newDataFrame <- rbind(d,e)
> newDataFrame
  one   two
1   1   one
2   2   two
3   3 three
4   4  vier
5   5  funf
6   6 sechs
```
```

```
2.00 0.75
> typeof(meanResult)
[1] "double"
```

## tapply

The tapply command is used to apply a function to different parts of data within an array. The function takes at least three arguments. The first is the data to apply an operation, the second is the set of factors that defines how the data is organized with respect to the different levels, and the third is the operation to perform. In the following example, a vector is defined that has the diameter of trees. A second vector is defined, which specifies what kind of tree was measured for each observation. The goal is to find the standard deviation for each type of tree:

```
> diameters <- c(28.8, 27.3, 45.8, 34.8, 25.3)
> tree <- as.factor(c("pine","pine","oak","pine","oak"))
> tapply(diameters,tree,sd)
      oak      pine
14.495689  3.968627
```

## mapply

The last command to examine is the mapply command. The mapply command takes a function to apply and a list of arrays. The function takes the first elements of each array and applies the function to that list. It then takes the second elements of each array and applies the function. This is repeated until it goes through every element. Note that if one of the arrays has fewer elements than the others, the mapply command will reset and start at the beginning of that array to fill in the missing values:

```
> a <- c(1,2,3)
> b <- c(1,2,3)
> mapply(sum,a,b)
[1] 2 4 6
>
```
