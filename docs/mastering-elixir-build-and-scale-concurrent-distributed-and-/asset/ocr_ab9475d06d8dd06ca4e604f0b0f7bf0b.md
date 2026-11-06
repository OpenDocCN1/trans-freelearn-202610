```
true
iex> false == :false
true
iex> nil == :nil
true
```

You have the common Boolean operators, `or`, `and`, and `not`:

```
iex> true or false
true
iex> true and false
false
iex> not false
true
```

However, these operators are type-strict in their first argument: they only accept `true` or `false`. If you pass anything else as an argument, you'll get `BadBooleanError`.

This is where the concept of *truthiness* and *falseness* enters. Similar to what happens in Ruby or C, `false` and `nil` are treated as *falsey* values, and everything else is considered to be *truthy*. The operators that work with *falsey* and *truthy* values are `&&` (and), `||` (or), and `!` (not):

```
iex> "a value" || false
"a value"
iex> "a value" && false
false
iex> nil && "a value"
nil
iex> !"a value"
false
```

Notice how these operators short circuit depending on the arguments. With `||`, it returns the first value that's *truthy*, whereas with `&&`, it returns the first *falsey* value (in both cases, in the event those conditions never happen, they return the last value).

You also have the other normal comparison operators, such as greater than (`>`) and inequality (`!=`)—you can find the full list at https://hexdocs.pm/elixir/operators.html. The one that's worth pointing out is the *strict equality* operator, which, besides comparing values, compares types:

```
iex> 3 == 3.0
true
iex> 3 === 3.0
false
```