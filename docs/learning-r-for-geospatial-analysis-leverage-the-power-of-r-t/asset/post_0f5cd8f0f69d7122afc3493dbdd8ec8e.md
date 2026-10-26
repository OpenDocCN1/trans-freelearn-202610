
There is another assignment operator in R, namely <-:

```r
> v <- 1:10
```

Throughout this book, the = operator is used since it is easier to type.

Also, note the difference between the assignment operator = and the equality conditional operator == (see the previous chapter). The = operator is used for assignment:

```r
> one = 1
> two = 2
> one = two
> one
[1] 2
> two
[1] 2
```

The == operator is used to compare:

```r
> one = 1
> two = 2
> one == two
[1] FALSE
```

![](img/ebfd6294770b225fdeceda9a7b8b560e_45_0.png)

When assigning an object with a name that already exists in memory, the older object is deleted and replaced by the new one:

```r
> x = 55
> x
[1] 55
> x = "Hello"
> x
[1] "Hello"
```

The ls function returns a character vector with the names of all the user-defined objects (in a given environment, with the default one being the global R environment). For example, so far we have assigned four objects in memory:

```r
> ls()
[1] "one" "two" "v" "x"
```

## Removing objects from memory

We can remove objects from memory by using the rm function. Let's take a look at the following examples:

```r
> rm("v")
> ls()
[1] "one" "two" "x"
```

It is sometimes useful to remove all objects from memory. For example, if we want to run a given code section without worrying that the previously defined objects will interfere, this can be done by passing the whole list of objects currently in memory to the rm function as follows:

```r
> rm(list = ls())
> ls()
character(0)
```

The `character(0)` output indicates an empty character vector. Removing all objects can be achieved by navigating to **Misc | Remove all objects** (RGui) or **Session | Clear workspace...** (RStudio). The reason for writing the `list=ls()` part will become evident after reading the *Using functions with several parameters* section in this chapter.

![](img/ebfd6294770b225fdeceda9a7b8b560e_46_0.png)

## Summarizing vector properties

Many functions in R are intended to work with vectors. The current section reviews some commonly used functions that are used to find out vectors' properties.

For example, we may be interested in the mean, minimal, and maximal values of a given vector. To get these, we can use the mean, min, and max functions, respectively:

```r
> v = 1:10
> mean(v)
[1] 5.5
> min(v)
[1] 1
> max(v)
[1] 10
```

We can also get both the minimal and maximal values at once with the `range` function:

```r
> range(v)
[1] 1 10
```

The length function returns the number of elements a given vector has:

```r
> v = c(4,2,3,9,1)
> length(v)
[1] 5
```

With logical vectors, we sometimes would like to know whether they contain at least one TRUE value or whether all of their values are TRUE. This can be achieved with the any and all functions, respectively:

```r
> l = c(TRUE, FALSE, FALSE, TRUE)
> any(l)
[1] TRUE
> all(l)
[1] FALSE
```

If we would like to know how many TRUE values a vector contains, we can utilize the default transformation from logical to numeric vectors when arithmetic functions are used on the former:

```r
> l = c(TRUE, FALSE, FALSE, TRUE)
> sum(l)
[1] 2
```

In this example, each TRUE value was first converted to 1 and each FALSE value to 0. Therefore, the vector c(TRUE,FALSE,FALSE,TRUE) became the vector c(1,0,0,1) and the sum of this vector's elements is 2.

The which function returns the positions of all the TRUE elements within a logical vector:

```r
> which(l)
[1] 1 4
```

Here, a vector of length 2 was returned since there are two TRUE values in the vector l. The two values of this vector are 1 and 4 since the first TRUE value occupies the first position in the vector l, while the second TRUE value occupies the fourth position.

The related functions `which.min` and `which.max` return the position of the minimal or maximal element in a numeric vector:

```r
> which.min(v)
[1] 5

> which.max(v)
[1] 4
```

We are going to see another example with `which.min` later in this book.

The last useful function we will mention is the unique function, which returns the unique elements of a vector; that is, it returns a set of elements the vector consists of without repetitions. Let's take a look at the following examples:

```r
> v = c(5,6,2,2,3,0,-1,2,5,6)
> unique(v)
[1] 5 6 2 3 0 -1
```

## Element-by-element operations on vectors

As opposed to functions that treat the vector as a single entity (as seen in the previous section), some functions work on each element of the vector as if it was a separate entity and return a vector of the results (which, therefore, has the same number of elements as the input vector). In fact, all arithmetic and logical operators work this way, as shown in the following examples (we did not have a chance to witness this since we always used vectors of length 1):

```r
> 1:10 * 2
[1]  2  4  6  8 10 12 14 16 18 20
> 1:10 - 10
[1] -9 -8 -7 -6 -5 -4 -3 -2 -1 0
> sqrt(c(4,16,64))
[1] 2 4 8
```

In the first expression, we multiplied the vector (1, 2, ..., 10) by 2, which resulted in a vector of 10 elements where the first element is equal to 1*2, the second is equal to 2*2, the third is equal to 3*2, and so on, up to 10*2.

Logical operators function in the same way, shown as follows:

```r
> x = 1:5
> x
[1] 1 2 3 4 5
> x >= 3
[1] FALSE FALSE TRUE TRUE TRUE
```

Here, for each of the values 1, 2, 3, 4, 5, it has been evaluated whether the value is larger or equal to 3, giving `FALSE` for 1 and 2 and `TRUE` for 3, 4, and 5.

If we want to check whether a given value from one vector is present in another, we can use the `%in%` operator. With `%in%`, we basically ask whether the value(s) of a vector on the left match any of the values of a vector on the right:

```r
> 1 %in% 1:10
[1] TRUE
> 11 %in% 1:10
[1] FALSE
```

For these simple examples, we can do without the `%in%` operator (see the following examples). Its utility will become apparent towards the end of this chapter, when we want, for instance, to look for each element of a long vector A and check whether it has a match in a long vector B. Here are the alternatives to the preceding expressions:

```r
> any(1:10 == 1)
[1] TRUE
> any(1:10 == 11)
[1] FALSE
```

In these two examples, we encompass the logical operation within the `any` function to check whether the resulting logical vector has at least one TRUE value.

Now, let's move on to character vectors. When working with character values, the `paste` function can be useful to combine separate elements into a single character string. The sep parameter of this function determines which characters will be used to separate the values (a single space is the default). Let's take a look at the following example:

```r
> paste("There are", "5", "books.")
[1] "There are 5 books."
```

The `paste` function also works with vectors that have more than one element:

```r
> paste("Image", 1:5)
[1] "Image 1" "Image 2" "Image 3" "Image 4" "Image 5"
```

Note that the `paste` function automatically converts numeric values into characters if characters are supplied:

```r
> x = 80
> paste("There are", x, "books.")
[1] "There are 80 books."
```

The paste0 function does the same thing as paste, with the default value for the sep parameter being nothing:

```r
> paste(1, 2, 3, sep = "")
[1] "123"

> paste0(1, 2, 3)
[1] "123"
```

## The recycling principle

In the previous chapter, we only used operators on two vectors of length 1. In this chapter, so far, we have used operations involving one vector of length 1 and another of length >1. What happens when we perform an operation involving two vectors of length >1?

If we have two vectors of exactly the same length, the operation is performed on each consecutive pair of elements taken from the two vectors, as follows:

```r
> c(1,2,3) * c(10,20,30)
[1] 10 40 90
```

In this example, 1 is multiplied by 10, 2 is multiplied by 20, and 3 is multiplied by 30, and the three results are combined into a single vector of length 3.

In case when the lengths of the two vectors are unequal, the shorter vector is recycled before the operation is performed. In other words, values at the beginning of the shorter vector are attached to its end, sequentially and as many times as necessary, until the lengths of both vectors match. The simplest case, which we witnessed in the previous section, is the one that involves one vector of length 1 and another vector of length greater than 1. We can describe what happens in such a case as the recycling of the vector that has one element until it matches the length of the longer vector. For example, when executing the first of these two expressions, it is as if we are performing the second:

```r
> 1:4 * 3
[1] 3 6 9 12

> 1:4 * c(3,3,3,3)
[1] 3 6 9 12
```

The same way, in the following example, the vector c(3,5) is recycled until it is of length 4, to c(3,5,3,5). The result is c(1,2,3,4) multiplied by c(3,5,3,5):

```r
> c(1,2,3,4) * c(3,5)
[1] 3 10 9 20
```
