As with most functional programming languages, functions are an important type, hence they justify having their own section, away from other built-in types.

We will start by exploring anonymous functions, followed by an explanation of modules and named functions, and then we'll end this section with a quick tour of module attributes and directives.

# Anonymous functions

Anonymous functions, usually called lambdas, are created with the fn keyword, as we can see in the following example:

```
iex> plus_one = fn (x) -> x + 1 end
#Function<6.99386804/1 in :erl_eval.expr/5>
iex> plus_one.(10)
11
```

Here, we are defining a function that takes one argument, which we've named x, and simply adds one to the provided argument. We then bind this anonymous function to a variable named plus_one, and execute it with 10 as the argument, using the syntax we can see in the preceding snippet. As expected, we get 11 back.

![](bbox=[0.12, 0.55, 0.2, 0.65])

There is no return keyword in Elixir–the return value of a function is the value returned by its last expression.

An anonymous function can also have multiple implementations, depending on the value and/or type of the arguments provided. Let's see this in action with an example:

```
iex> division = fn
...> (_dividend, 0) -> :infinity
...> (dividend, divisor) -> dividend / divisor
...> end
#Function<12.99386804/2 in :erl_eval.expr/5>
iex> division.(10, 2)
5.0
iex> division.(10, 0)
:infinity
```