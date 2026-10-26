Two other useful conditional operators (`==` and `!=`) are used for testing equality and inequality, respectively. These operators are opposites from one another since a pair of objects can be either equal or non-equal to each other.

```
```r
> 1 == 1
[1] TRUE
> 1 == 2
[1] FALSE
> 1 != 1
[1] FALSE
> 1 != 2
[1] TRUE
```
```

As you can see in the preceding examples, when using the `==` operator, TRUE is returned if the compared objects are equal; otherwise FALSE is returned (refer to expressions 1 and 2). With `!=` it is the other way around (refer to expressions 3 and 4).

The last operator that we are going to cover is the not operator (`!`). This operator reverses the resulting logical value, from TRUE to FALSE or from FALSE to TRUE. This is used in cases when it is more convenient to ask whether a condition is *not* satisfied. Let's take a look at the following example:

```
```r
> 1 == 1
[1] TRUE
> !(1 == 1)
[1] FALSE
> (1 == 1) & (2 == 2)
[1] TRUE
> (1 == 1) & !(2 == 2)
[1] FALSE
```
```

# ## Using functions

In mathematics, a function is a relation between a set of inputs and a set of outputs with the property that each input is related to exactly one output. For example, the function *y* = 2*x* relates every input *x* with the output *y*, which is equal to *x* multiplied by 2. The function concept in R (and in programming in general) is very similar:

+   - The function is composed of a code section that knows how to perform a certain operation.
+   - Employing the function is done by *calling* the function.
+   - The function receives one object, or several objects, as input (for example, the number 9).