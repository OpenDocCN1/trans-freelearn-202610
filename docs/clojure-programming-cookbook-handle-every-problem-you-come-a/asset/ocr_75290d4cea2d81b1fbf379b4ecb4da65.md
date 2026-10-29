# comp

The `comp` composes a function from functions given as arguments.

The expression `(comp f2 f1)` applies f1 to arguments and then applies f2 to the results of f1. The `comp` applies functions from right to left. The following code increments the number and then negates it:

```
(map (comp - inc) [ 1 2 3 4 5])
;;=> (-2 -3 -4 -5 -6)
```

# every-pred and some-fn

The `every-pred` function returns a function to test whether all predicates are true.

In the following code, `every-pred` returns a function that checks whether all tests for all arguments are passed or not:

```
2 < x < 12
x is odd number
```

There are two variations to be evaluated, `true` or `false`:

```
((every-pred odd? #(< % 12) # (> % 2)) 3 5 7)
;;=> true
```

```
((every-pred odd? #(< % 12) # (> % 2)) 1 2 3)
;;=> false
```

Using `filter`, it tests each element of the sequence:

```
(filter (every-pred odd? #(< % 12) # (> % 2)) [1 2 3 4 5])
;;=> (3 5)
```

In the following code, the `some-fn` function returns true if at least one of a given predicates returns true for any arguments.

In the following code, the first expression is evaluated to be true and the second expression to be false:

```
((some-fn even? # (= % 10) # (= % 1)) 11 10 1)
```