# Comprehensions

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

We won't be using comprehensions in the application we'll build throughout this book. However, it's important to mention them, as there are cases where using a comprehension instead of functions from the Enum module renders more elegant and expressive code

In our example, all comprehensions are returning a list, which is the default behavior. We can change that by passing the into: option, as you can see in this example:

```
iex> for x <- [1, 2, 3], into: %{}, do: {x, x + 1}
%{1 => 2, 2 => 3, 3 => 4}
```

![](bbox=[0.12, 0.7, 0.2, 0.8])

As you can see, now we're getting a map back. The into: option takes a collection that will receive the results of the comprehension. This collection must implement the Collectable protocol. This protocol can be seen as the opposite of the Enumerable protocol, and is used to create a new structure from the values of an existing collection. This also has usage outside of comprehensions–the Enum.into/2 function uses this protocol to create a new collection based on an enumerable.