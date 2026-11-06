
## Looping through recursion

We'll show you how to create a recursive functions through two simple examples: doubling each element on a list, and multiplying consecutive elements of a list. As mentioned earlier, although you probably won't be using this in your day-to-day coding, it's still very important to understand how this works. This way, if the abstractions Elixir provides aren't enough for your use case, you can just create your own recursive functions to accomplish what you need.

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
  def multiply([]), do: 1

  def multiply([head | tail]) do
    head * multiply(tail)
  end
end
```

Here's its use on an IEx session:

```
iex> Recursion.multiply([1, 2, 3])
6
```

The strategy is similar to the one shown in the previous example, except, instead of adding an element to a list at each step, we're now using our head as an accumulator. Also, it's important to note that, since we're doing a multiplication, our stopping condition must return 1 (the neutral element of this operation). The definition of the stopping condition varies between different problems, and is, arguably, one of the most important steps of defining a function recursively.

A common concern when dealing with recursive functions is its memory usage, as we have multiple function calls that will get into the stack. The Erlang runtime employs tail-call optimization whenever it can, which means that a recursive call won't generate a new stack push. For the runtime to do this optimization, you have to ensure that the last thing our function does is call another function (including itself)–or, in other words, make a tail call. Here's our multiply function updated to make tail calls:

```
$ cat examples/multiply_with_tail_recursion.ex
def multiply(list, accum \\ 1)
def multiply([], accum), do: accum
def multiply([head | tail], accum) do
  multiply(tail, head * accum)
end
```

The usual strategy is to pass an accumulator around, which enables us to use the tail-call optimization. Note that there's a trade-off here: On one hand, this optimization is important when dealing with large collections (since function calls don't consume additional memory); on the other hand, code that doesn't use this optimization is usually easier to read and comprehend, as it's usually more concise. When doing recursion, consider the advantages and disadvantages of each solution.

## Eager processing with the Enum module

Having seen how recursion works in Elixir, we'll now show some examples of the abstractions that are built on top of it. We'll explore the Enum module, which contains a set of functions to work on collections. We've already seen some examples of collections in the Elixir's data types section, such as lists or maps. More generally, we can use the Enum module on collections that implement the Enumerable protocol.

![](img/c636f4cfb07f25f1db535995e50706ed_52_0.png)

We haven't yet covered protocols. We will do so in the Protocols section.

Taking the two examples from our Recursion section, let's see how they become incredibly simple to implement using the Enum module:

```
iex> Enum.map([2, 4, 6], &(&1 * 2))
[4, 8, 12]
iex> Enum.reduce([1, 2, 3], 1, &(&1 * &2))
6
```

The map function receives a collection and a lambda, and returns a new list where the lambda is applied to each element of the collection.

The reduce function receives a collection, an accumulator, and a lambda. The lambda receives the current element of the collection and the accumulator, and the result of this lambda is the accumulator for the following iteration. At the end of the iteration, reduce returns the final accumulator value.

We're using the capture operator to define a lambda. As we've previously hinted, you can also use it to capture named functions. In the following example, we're using the Integer.is_even/1 function to check which numbers are even in a collection:

![](img/c636f4cfb07f25f1db535995e50706ed_52_1.png)

```
iex> require Integer
Integer
iex> Enum.map([1, 2, 3], &Integer.is_even/1)
[false, true, false]
```

You'll see the Enum module being used in the application that we'll build throughout the book. For further usage of the Enum module, check its documentation at https://hexdocs.pm/elixir/Enum.html.

## Comprehensions

Elixir provides another construct to iterate collections: comprehensions. As with the functions from the Enum module, comprehensions work on anything that implements the Enumerable protocol. Let's see a simple example:

```
iex> for x <- [2, 4, 6], do: x * 2
[4, 8, 12]
```

While, in this simple example, it is similar to Enum.map/2, comprehensions bring some other interesting features. You can, for instance, iterate over multiple collections and also apply filters. Let's see these two being applied in the following example:

```
iex> for x <- [1, 2, 3], y <- [4, 5, 6], Integer.is_odd(x), do: x * y
[4, 5, 6, 12, 15, 18]
```

Here we're doing a nested iteration–for each element of the first enumerable (which is represented by x), we will iterate through all elements of the second enumerable (represented by y). Also, we're applying a filter, and the body of our comprehension only gets executed when x is odd.

We won't be using comprehensions in the application we'll build throughout this book. However, it's important to mention them, as there are cases where using a comprehension instead of functions from the Enum module renders more elegant and expressive code.

In our example, all comprehensions are returning a list, which is the default behavior. We can change that by passing the into: option, as you can see in this example:

```
iex> for x <- [1, 2, 3], into: %{}, do: {x, x + 1}
%{1 => 2, 2 => 3, 3 => 4}
```

![](img/c636f4cfb07f25f1db535995e50706ed_53_0.png)

As you can see, now we're getting a map back. The into: option takes a collection that will receive the results of the comprehension. This collection must implement the Collectable protocol. This protocol can be seen as the opposite of the Enumerable protocol, and is used to create a new structure from the values of an existing collection. This also has usage outside of comprehensions–the Enum.into/2 function uses this protocol to create a new collection based on an enumerable.

## Lazy processing with the stream module

We will now talk about a different way of processing collections, which, as functional programming, may require a shift in your mindset. Before talking about lazy processing, let's enumerate some of the shortcomings of working with the Enum module. The Enum module is referred to as being eager. This means that when processing a collection, this module will load the entire collection into memory. Furthermore, if you have a chain of functions you want to apply to a collection, the Enum module will iterate through your collection as many times as the functions are applying to it. Let's examine this further with an example:

```
iex> [1, 2, 3, 4, 5] \
...> |> Enum.map(&(&1 + 10)) \
...> |> Enum.zip(["a", "b", "c", "d", "e"])
[{11, "a"}, {12, "b"}, {13, "c"}, {14, "d"}, {15, "e"}]
```

![](img/c636f4cfb07f25f1db535995e50706ed_54_0.png)

The \ on the end of the first two lines is to stop our Elixir console from evaluating this line right away, and wait for a new line instead. This way, we can write these operations with the pipe operator on multiple lines, which makes them more readable.

We take our initial collection and iterate it to add 10 to each element inside it. This generates a new list, which is passed to our next function. This function will zip the two lists together, which will produce a new list, which is returned to us. In this simple example, we need to traverse our list twice to build the desired result.

This is where the Stream module, and lazy processing, becomes advantageous. When working with lazy enumerables, the entire collection never gets loaded into memory, and contrary to what we're accustomed to, the computations aren't made right away. The results are produced as they are needed. Let's see this same example with the Stream module:

```
iex> [1, 2, 3, 4, 5] \
...> |> Stream.map(&(&1 + 1)) \
...> |> Stream.zip(["a", "b", "c", "d", "e"])
#Function<66.40091930/2 in Stream.zip/1>
```

As you can see, we're not getting our final list back. When we feed our list to Stream.map, the list is not iterated. Instead, the functions that will be applied on it are saved into a structure (along with the collection we're working on). We can then pass this structure into the next function, which will further save a new function to be applied to our list. This is really cool! But how do we make it return the result we're expecting? Just treat it as a regular (eager) enumerable, by applying a function from the Enum module, and it will start to produce results.
