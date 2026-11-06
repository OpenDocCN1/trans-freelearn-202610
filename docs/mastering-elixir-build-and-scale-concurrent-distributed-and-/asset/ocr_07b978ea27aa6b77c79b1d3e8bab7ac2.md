```
Map.update(accum, media.user_id, 1, &(&1 + 1))
end
```

On the preceding function, we are passing a lambda function as the :hash option to the Flow.partition/2 call. This lambda receives an element from the collection and has to yield a two-element tuple, which contains the resulting element that will be emitted and the value of the hashing function that will point to the right stage.

![](bbox=[0.12, 0.28, 0.18, 0.38])

We are subtracting one from the user_id value: each stage created by the partition/2 call is 0-index based, hence for N users starting with id=1, we want N-1 hashing values starting at 0. If we incorrectly defined the :hash lambda as fn m -> {m, m.user_id} end, we would obtain something like [{2, 1}, {1, 1}, {3, 3}]. As you can see, the single file user 4 had wasn't considered, because it was allocated to stage 4, and we only had stages 0 to 3.

With this :hash function, we now get the expected result. You should also be aware that with Flow, results aren't sorted by default. We don't control how fast results are produced, so no order is guaranteed. We can now see that user 3 has three files on ElixirDrip, and the other three users only have one each:

```
# 'search.exs' already evaluated

iex> files_by_user_v2 |> Enum.to_list()
[{1, 1}, {2, 1}, {4, 1}, {3, 3}]
```

In situations where we want to distribute the collection items by stages and we have each item uniquely identified by a key, we could instead use the :key option of the Flow.partition/2 function. This option allows you to not implement the hashing function at all. If you pass the {:key, :user_id} tuple as this option value, it assumes all items are maps in order to use the :user_id field of the item as the hashing value. If you pass {:elem, 2} instead, it assumes all items are tuples and it should use the third element (tuples are 0-index based) as the hashing value to route items to stages.

Let's iterate just one more time on the preceding version we already had working:

```
$ cat examples/search.exs
import_file("./examples/sample_data.exs")
# ...
files_by_user_v3 = media_set
|> Flow.from_enumerable(max_demand: 1)
|> Flow.partition(key: {:key, :user_id})
|> Flow.reduce(fn -> %{} end,
fn media, accum ->
```