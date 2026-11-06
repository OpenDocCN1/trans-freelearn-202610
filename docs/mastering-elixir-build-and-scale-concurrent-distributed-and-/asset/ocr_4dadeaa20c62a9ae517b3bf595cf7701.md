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