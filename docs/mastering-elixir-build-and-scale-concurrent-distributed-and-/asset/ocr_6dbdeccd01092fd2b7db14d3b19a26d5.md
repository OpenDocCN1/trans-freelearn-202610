# Looping through recursion

We'll show you how to create a recursive functions through two simple examples: doubling each element on a list, and multiplying consecutive elements of a list. As mentioned earlier, although you probably won't be using this in your day-to-day coding, it's still very important to understand how this work. This way, if the abstractions Elixir provides aren't enough for your use case, you can just create your own recursive functions to accomplish what you need.

Before jumping into the examples, let's explain generally how recursive functions work. In Elixir, they're usually implemented as a multi-clause function, using pattern matching to control its flow of execution. The first clause sets the condition that will stop the recursion, and is followed by other broader clauses that apply the recursion.

In the first example, we want to take a list of integers as input, and return a new list where each element is multiplied by two. Let's see the code for such a function:

```
$ cat examples/double.ex
defmodule Recursion do
  def double([]), do: []

  def double([head | tail]) do
    [head * 2 | double(tail)]
  end
end
```

Here are its results:

```
iex> Recursion.double([2, 4, 6])
[4, 8, 12]
```

Besides using multi-clause functions, we're also using pattern matching in two ways: to know when we've reached the end (and the empty list) and treat it accordingly; and to extract the head and the tail of a list, similar to what we've shown in the pattern matching section. The recursion happens when we call double(tail). As we're only passing the tail to the recursive call, we're essentially iterating through the list. When we reach an empty list, the first clause matches, we return an empty list, and all of the intermediate calls will unfold and create our new list.

What if, instead of returning a new list, we want to return a single value? We'll exemplify this by multiplying consecutive elements of a list. Here's the code to do it:

```
$ cat examples/multiply.ex
defmodule Recursion do
  def multiply([],) do: 1
```