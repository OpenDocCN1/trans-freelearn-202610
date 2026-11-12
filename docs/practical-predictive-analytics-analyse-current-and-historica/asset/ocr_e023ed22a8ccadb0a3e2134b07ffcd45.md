First, call the function for the `test` dataset:

```
test_normal <- normalize_it(test_means)
```

Next, normalize for the training dataset:

```
df_normal <- normalize_it(df_means)
```

# Displaying the output

Print some of the output for the normalized training data.

```
head(df_normal)
```

Notice from the output below that the normalized values appear to range between -2 and +2 with a mean of 0. Any value close to these extremes falls near the extreme range for the variables.

For example, observation 4 shows a normalized age of less than -2, which corresponds to the original age of 9, while observation 4 also shows a normalized body mass of 1.68, which corresponds to the original body mass of 44.

This is at the upper range for a 9-year-old child, and we have been able to quickly identify some extreme values simply by looking at the relationship between some of the normalized values: