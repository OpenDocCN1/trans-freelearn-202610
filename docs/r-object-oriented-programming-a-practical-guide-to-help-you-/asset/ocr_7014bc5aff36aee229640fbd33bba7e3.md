# Sampling

The final topic that we will discuss is sampling. This can also be a complicated subject, and it is often used in bootstrapping and a wide variety of other techniques. Because of its prevalence, we provide it as a separate section.

The sole focus of this section is on the sample command. It may seem odd to grant such attention to a single command, but sampling is a complex topic with more opinions associated with it than there are statisticians. The sample command requires at least one argument, a vector or a number, and it returns a set of values chosen at random. The options for the command allow you to specify how many samples to take, whether or not to use replacement, and a probability distribution if you do not wish to use a uniform mass function.

The sample function’s behavior depends on whether or not you give it a vector or a number. If you pass a number to it (that is, a vector of length 1), it will sample from the set of whole, positive numbers less than or equal to that number:

```
> sample(3)
[1] 3 2 1
> sample(5)
[1] 2 3 1 4 5
> sample(5.6)
[1] 5 1 4 3 2
```

If you pass it a vector whose length is greater than 1, it will sample from the elements in the vector:

```
> x <- c(1,3,5,7)
> sample(x)
[1] 7 1 5 3
```

If you do not specify the number of samples to take, it will use the number of objects passed to it. The size parameter allows you to specify a different number of samples:

```
> x <- c(1,3,5,7)
> sample(x,size=2)
[1] 1 7
> sample(x,size=3)
[1] 1 3 5
> sample(x,size=8)
Error in sample.int(length(x), size, replace, prob) :
    cannot take a sample larger than the population when 'replace = FALSE'
```

In the preceding example, the number of samples is larger than the number of elements available. To avoid an error, you have to specify sampling with replacement:

```
> x <- c(1,3,5,7)
> sample(x,size=8,replace=TRUE)
[1] 3 5 5 5 3 7 5 1
```

In the previous examples, the samples were found using the default algorithm. The default is to use a uniform probability mass function meaning every element has the same