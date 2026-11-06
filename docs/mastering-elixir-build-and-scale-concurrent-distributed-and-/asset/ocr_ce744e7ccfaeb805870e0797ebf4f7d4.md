# Tuples

*Tuples* are used to group a fixed number of elements together. They can hold any value—even other tuples. They are stored contiguously in memory, which provides constant access time to elements inside a tuple. You create a tuple surrounding the elements with curly brackes ({ and }), and separate the elements with commas:

```
```
iex> {:ok, 3.14}
{:ok, 3.14}
```
```

A common usage of tuples in Elixir is to pattern-match on the result of a function to ensure its success (usually with an :ok atom) or deal with an error. We will be looking to pattern matching and functions later in this chapter.

To access an element inside a tuple, we use the elem function (from the Kernel module), providing the tuple and a zero-based index:

```
```
iex> result = {:ok, 3.14}
{:ok, 3.14}
iex> elem(result, 1)
3.14
```
```

![](bbox=[0.12, 0.49, 0.19, 0.58])

Functions from the Kernel module are auto-imported. Thus, we don't need to prefix them with the module name.

To change the elements on a tuple, you can use the put_elem function. The arguments are similar to the elem function, but you also provide the new value for that position of the tuple:

```
```
iex> put_elem(result, 1, 1.61)
{:ok, 1.61}
iex> result
{:ok, 3.14}
```
```

Notice how the result variable hasn't changed. As we discussed in the beginning of this chapter, data in Elixir is immutable. As such, although we've updated the tuple with a new value, the original tuple hasn't changed—Elixir updated the value on a copy of the original tuple. This way our code is side-effect free, and any other function holding a reference to the result variable won't have any surprises.

The general recommendation in Elixir is that tuples should hold up to four elements—anything more than that and you probably should be using another type.