# Working with Tables

Importantly, the condition should be an expression that returns a single logical value. The code section following this condition will then be executed if the value is TRUE or ignored if the value is FALSE. For example, if x is not larger than 2, nothing will happen since the `print("x is large!")` expression will not be executed:

```
```r
> x = 0
> if(x > 2) {print("x is large!")}
```
```

Nothing is printed on screen.

> The same way as with function definitions (see the previous chapter) and for loops (see the next section), code with only one expression does not have to be encompassed in parentheses `{`.

Optionally, we can use the `else` operator to add another code section. The code section after the else operator will be executed when the condition in if is FALSE as follows:

```
```r
> x = 3
> if(x > 2) {print("x is large!")} else {print("x is small!")}
[1] "x is large!"
> x = 1
> if(x > 2) {print("x is large!")} else {print("x is small!")}
[1] "x is small!"
```
```

There is another conditional operator, specialized in working on vectors element by element, called `ifelse`. With `ifelse`, we need to supply three arguments: a logical vector, a value for TRUE (the yes parameter), and a value for FALSE (the no parameter). What we receive is a new vector with the same length as the input logical vector, where the TRUE and FALSE values have been replaced with the alternative values we supplied.

Regarding the replacement values for TRUE and FALSE, the most useful modes of operation are either to have them as vectors of length 1 (and then they are recycled to fill the entire length of the logical vector) or to have them as vectors of the same length as the logical vector (and then the elements of the logical vector are replaced with the respective elements either from the yes or no vector).

For example, the first mode of operation is useful when we want to classify the values of a given vector into two categories, according to a condition:

```
```r
> dat$mmxt[1:7]
[1]  5.6  9.8  7.2 12.2 11.8 19.6 24.1
> ifelse(dat$mmxt[1:7] < 10, "cold", "warm")
[1] "cold" "cold" "cold" "warm" "warm" "warm" "warm"
```
```