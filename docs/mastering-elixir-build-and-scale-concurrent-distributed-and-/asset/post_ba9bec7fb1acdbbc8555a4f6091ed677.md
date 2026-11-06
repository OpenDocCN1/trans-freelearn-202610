
To exemplify this, we'll use the Enum.take/2 function, which allows us to take a given number of items from an enumerable:

```
iex> [1, 2, 3, 4, 5] \
...> |> Stream.map(&(&1 + 10)) \
...> |> Stream.zip(["a", "b", "c", "d", "e"]) \
...> |> Enum.take(1)
[{11, "a"}]
```

As you can see, we're now getting the expected result back. Note that this is not a result of applying our computation to all the list and then just taking the first element. We've essentially only computed results for the first element, as that's all that was necessary. If you wanted to have the full list in the end, you could use the Enum.to_list/1 function.

Streams are a really nimble way to process large, or even infinite, collections. Imagine that you're parsing values from a huge CSV file, and then running some functions on them. If you're running your application on the cloud, as most of us are these days, you probably have a short amount of memory. Using lazy processing, you can avoid having to load the whole file, processing it line by line. If you're processing an infinite collection, such as an RSS feed, lazy processing is also a great solution, as you can process each element of the collection incrementally, as they arrive.

Note that while the Stream module is amazing, it will not replace your usage of the Enum module. It's certainly great for very large collections, or even if you have a big chain of functions being applied to a collection and only want to traverse it once. However, for small or even medium collections, the Stream module will perform worse, as you're adding a lot of overhead, for instance, by having to save the functions you'll apply instead of applying them right away. Always analyze your situation carefully and take this into account when choosing to use the Enum or the Stream module for a given task.

We'll be using functions from the Stream module in the application we'll build in this book. You'll learn more about the Stream module in Chapter 4, Powered by Erlang/OTP.

![](img/c636f4cfb07f25f1db535995e50706ed_55_0.png)

Elixir provides some functions that wrap most of the complex parts of building streams. If you want to build your own lazy stream, check out these functions from the Stream module: cycle, repeatedly, iterate, unfold, and resource. The full documentation for the Stream can be found at https://hexdocs.pm/elixir/Stream.html.

## Control flow

We're now introducing control-flow constructs. In Elixir, they aren't used as often as in traditional imperative languages, because we can fulfill our control-flow needs, using a mix of pattern matching, multi-clause functions, and guard clauses. Whenever you're about to use one of the constructs we're presenting in this section, stop and check whether it's possible to employ a more functional approach. Code without these traditional control-flow constructs is usually easier to understand and test. If you get to a point where you have nested conditionals, it's almost guaranteed you can simplify it by using one of the approaches I mentioned earlier. Either way, you'll occasionally use these constructs, so it's important to know they exist.

### if and unless

These two constructs can be used with the following syntax:

```
if <expression> do
  # expression was truthy
else
  # expression was falsy
end
```

```
unless <expression> do
  # expression was falsy
else
  # expression was truthy
end
```

As with the `def` construct, they can be inlined. For `if`, you'd do this:

```
if <expression>, do: # expression was truthy, else: # expression was falsy
```

For both constructs, the `else` clause is optional. They will return `nil` if the main clause doesn't match and no `else` clause was provided.

### cond

`cond` can be seen as a multi-way `if` statement, where the first *truthy* condition will run its associated code. This may substitute chains of `if` ... `else if` blocks. Let's see this with an example on IEx:

```
iex> x = 5
```

```
5
iex> cond do
...>   x * x == 9 -> "x was 3"
...>   x * x == 16 -> "x was 4"
...>   x * x == 25 -> "x was 5"
...>   true -> "none of the above matched"
...> end
"x was 5"
```

true in a condition will serve as a default condition, which will run when no other clause matches.

### case

case accepts an expression, and one or more patterns, which will match against the return value of the expression. These patterns may include guard clauses. These patterns are matched (from top to bottom), and will run the code associated with the first expression that matches. Here is a simple example:

```
iex> case Enum.random(1..10) do
...>   2 -> "The lucky ball was 2"
...>   7 -> "The lucky ball was 7"
...>   _ -> "The lucky ball was not 2 nor 7"
...> end
"The lucky ball was not 2 nor 7"
```

Note that your output may differ when running this example, as we're matching against Enum.random/1. In here, the default condition is represented by using _ in the pattern, which will match anything. Although a bit more condensed, the case construct is similar to a multi-clause function.

### with

This control-flow construct, introduced in Elixir 1.2, accepts one or more expressions, a do block, and optionally an else block. It allows you to use pattern matching on the return value of each expression, running the do block if every pattern matches. If one of the patterns doesn't match, two things may happen: If provided, the else block will be executed; otherwise, it will return the value that didn't match the expression. In practice, with allows you to replace a chain of nested instances of case or a group of multi-clause functions.

To demonstrate the usefulness of with, let's see an example:

```
iex> options = [x: [y: [z: "the value we're after!"]]]
[x: [y: [z: "the value we're after!"]]]
iex> case Keyword.fetch(options, :x) do
...>   {:ok, value} -> case Keyword.fetch(value, :y) do
...>     {:ok, inner_value} -> case Keyword.fetch(inner_value, :z) do
...>       {:ok, inner_inner_value} -> inner_inner_value
...>       _ -> "non-existing key"
...>     end
...>     _ -> "non-existing key"
...>   end
...>   _ -> "non-existing key"
...> end
"the value we're after!"
```

We're using the Keyword.fetch/2 function to get the value of a key from a keyword list. This function returns {:ok, value} when the key exists, and :error otherwise. We want to retrieve the value that's nested on three keyword lists. However, let's say that if we try to fetch a key that doesn't exist on the keyword list, we have to return "non-existing key". Let's achieve the same behavior using with, operating on the same options list as the preceding example:

```
iex> with {:ok, value} <- Keyword.fetch(options, :x),
...>   {:ok, inner_value} <- Keyword.fetch(value, :y),
...>   {:ok, inner_inner_value} <- Keyword.fetch(inner_value, :z),
...> do: inner_inner_value
"the value we're after!"
```

Note that, since our expression is really small, we're using the shorthand do: syntax (but we can also use a regular do ... end block). As you can see, we're getting the same result back. Let's try to fetch a key that doesn't exist:

```
iex> with {:ok, value} <- Keyword.fetch(options, :missing_key),
...>   {:ok, inner_value} <- Keyword.fetch(value, :y),
...>   {:ok, inner_inner_value} <- Keyword.fetch(inner_value, :z),
...> do: inner_inner_value
:error
```

Since we didn't provide an else block, we're getting back the value that didn't match, which is the return value of Keyword.fetch/2 when a key doesn't exist in the keyword list provided. Let's do the same, but by providing an else block:

```
iex> with {:ok, value} <- Keyword.fetch(options, :missing_key),
...> {:ok, inner_value} <- Keyword.fetch(value, :y),
...> {:ok, inner_inner_value} <- Keyword.fetch(inner_value, :z) do
```

```
...> inner_inner_value
...> else
...> :error -> "non-existing key"
...> _ -> "some other error"
...> end
"non-existing key"
```

Since we're now providing an `else` block, we can now handle error cases accordingly. As you can see, `else` takes a list of patterns to match on. As you do with `case`, you can use `_` as a default clause, which would run when the patterns above (if any) didn't match.

As you can see, `with` is a very helpful construct, which allows us to create very expressive code that is concise and easy to read. Moreover, you can control how to handle each error separately, using pattern matching inside the `else` block.

![](img/c636f4cfb07f25f1db535995e50706ed_59_0.png)

The first expression provided to `with` has to be on the same line of `with` itself, you'll get a `SyntaxError` otherwise. If you do want to have `with` on its own line, wrap the expressions provided to it in parentheses.

## Exceptions

Much like `if` and `else`, exceptions in Elixir aren't used as much as in other popular imperative languages. Exceptions aren't used for control flow, and are left for when truly exceptional things occur. When they do, your process is usually running under a supervision tree, and upon crashing, the supervisor of your process will be notified and (possibly, depending on the strategy) restart it. Then, upon being restarted, you're back to a known and stable state, and the effects of the exceptional event are no longer present. In the Elixir and Erlang communities, this is usually referred to as the "Let it crash!" philosophy. We'll be examining this in greater detail in `Chapter 3, Processes – The Bedrock for Concurrency and Fault Tolerance`, when we talk about processes, supervisors, and supervision trees.

For now, I'll list the traditional error-handling constructs. You can raise an error with the `raise` construct, which takes one or two arguments. If you provide only one argument, it will raise a `RuntimeError`, with the argument as the message. If you provide two arguments, the first argument is the type of error, while the second is a keyword list of attributes for that error (all errors must at least accept the `message:` attribute). Let's see this in action:

```
iex> raise "Something very strange occurred"
** (RuntimeError) Something very strange occurred
```
