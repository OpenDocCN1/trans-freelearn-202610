Here, for each of the values 1, 2, 3, 4, 5, it has been evaluated whether the value is larger or equal to 3, giving `FALSE` for 1 and 2 and `TRUE` for 3, 4, and 5.

If we want to check whether a given value from one vector is present in another, we can use the `%in%` operator. With `%in%`, we basically ask whether the value(s) of a vector on the left match any of the values of a vector on the right:

```
> 1 %in% 1:10
[1] TRUE
> 11 %in% 1:10
[1] FALSE
```

For these simple examples, we can do without the `%in%` operator (see the following examples). Its utility will become apparent towards the end of this chapter, when we want, for instance, to look for each element of a long vector A and check whether it has a match in a long vector B. Here are the alternatives to the preceding expressions:

```
> any(1:10 == 1)
[1] TRUE
> any(1:10 == 11)
[1] FALSE
```

In these two examples, we encompass the logical operation within the `any` function to check whether the resulting logical vector has at least one TRUE value.

Now, let's move on to character vectors. When working with character values, the `paste` function can be useful to combine separate elements into a single character string. The sep parameter of this function determines which characters will be used to separate the values (a single space is the default). Let's take a look at the following example:

```
> paste("There are", "5", "books.")
[1] "There are 5 books."
```

The `paste` function also works with vectors that have more than one element:

```
> paste("Image", 1:5)
[1] "Image 1" "Image 2" "Image 3" "Image 4" "Image 5"
```

Note that the `paste` function automatically converts numeric values into characters if characters are supplied:

```
> x = 80
> paste("There are", x, "books.")
[1] "There are 80 books."
```