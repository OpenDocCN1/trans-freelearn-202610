
Since the table can only be written by its process owner, this function will simply do a GenServer.call/2 to the process owner that will then be responsible for updating the ETS table on the correspondent handle_call/3 callback:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/
search_cache_worker.ex
defmodule ElixirDrip.Storage.Workers.SearchCacheWorker do
  # ...
  def cache_search_result(media_id, search_expression, result) do
    GenServer.call(__MODULE__, {:put, media_id, search_expression,
    result})
  end

  def handle_call({:put, media_id, search_expression, result}, _from,
  search_cache) do
    created_at = :os.system_time(:seconds)

    result = :ets.insert_new(search_cache, {{media_id,
    search_expression}, {created_at, result}})

    {:reply, {:ok, result}, search_cache}
  end
end
```

To insert a new value into the ETS table, we pass the table name (stored on the process state) and the new entry to the :ets.insert_new/2 function. This new entry has to always be a tuple with the key on the first element. In our case, we are passing {{media_id, search_expression}, {created_at, result}} as the second argument, so our key is the {media_id, search_expression} tuple, and its value is another tuple with the created_at timestamp and the actual text search result (towards the end of this section, you will understand why we are storing the timestamp as well). The :ets.insert_new/2 call will return a boolean value, indicating the operation's success:

```
iex> SearchCacheWorker.cache_search_result("1", "media",
"lorem ipsum media 1 contents")
{:ok, true}

iex> SearchCacheWorker.cache_search_result("1", "media",
"this can't be cached")
{:ok, false}

iex> SearchCacheWorker.cache_search_result("1", "other",
"other phrase found on media 1 contents")
{:ok, true}
```

We set the table access mode as :protected, hence every process can read its contents.

In this scenario, we don't need to ask the owner process to fetch the data stored on the table for us, we can just interact directly with it. This is why we store the table name on the @search_cache module attribute: since every process is able to read from the table, the table name can't live solely on the state of the process owner.

To access a table entry, you can use the :ets.lookup/2 function, which receives the table name or reference and the key we are looking for. This function returns a list of entries that have that key. In our case, every returned list will only have one element, because we're using a :set ETS table:

```
iex> :ets.lookup(:search_cache, {"1", "media"})
[{"1", "media", {1518475143, "lorem ipsum media 1 contents"}}]

iex> :ets.lookup(:search_cache, {"1", "unknown"})
[]
```

As you just saw, we get a list with a {key, value} tuple when there is an entry for that key. To easily fetch the stored value, we encapsulated the :ets.lookup/2 function in the search_result_for/2 function:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/search_cache_worker.ex
defmodule ElixirDrip.Storage.Workers.SearchCacheWorker do
  # ...
  @search_cache :search_cache

  def search_result_for(media_id, search_expression) do
    case :ets.lookup(@search_cache, {media_id, search_expression}) do
      [] ->
        nil
      [{_key, {_created_at, search_result}}] ->
        search_result
    end
  end
end
```

With the previous function, we can't fetch all the text searches cached for a given media_id. If we want to implement it, we need our query to only look at the first element of the key tuple, which contains the media_id. The :ets.match_object/2 function allows us to do exactly that: with it, you can pattern match on specific fields:

```
iex> :ets.match_object(:search_cache, {{"1", :"_"}, :"_"})
[
  {{"1", "other"}, {1518509888, "other phrase found on media 1 contents"}},
  {{"1", "media"}, {1518475143, "lorem ipsum media 1 contents"}}
]
```

Let's create an all_search_results_for/1 function that pattern matches on the media_id element of the key tuple, similar to what we just did. We've used the special form : "_" to indicate that, for our {{media_id, search_expression}, {created_at, result}} entries, we don't care about the search_expression tuple element nor the last tuple corresponding to the value of each entry:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/
search_cache_worker.ex
defmodule ElixirDrip.Storage.Workers.SearchCacheWorker do
  # ...
  @search_cache :search_cache

  def all_search_results_for(media_id) do
    case :ets.match_object(@search_cache, {{media_id, :"_"}, : "_"}) do
      [] ->
        nil
      all_objects ->
        all_objects
        |> Enum.map(fn {key, value} ->
          {elem(key, 1), elem(value, 1)}
        end)
    end
  end
end
```

:ets.match_object/2 returns a list with every entry that matches our search criteria. In order to return only the search_expression and the result of each entry, we map the resulting list, respectively getting the first from the entry key and the latter from the entry value:

```
iex> SearchCacheWorker.all_search_results_for("1")
[{"other", "other phrase found on media 1 contents"}, {"media",
  "lorem ipsum media 1 contents"}]
```

Finally, let's use the created_at timestamp that's stored every time we cache a search result. The idea here is to create a SearchCacheWorker.expired_search_results/0 that will return all the search results keys that were created more than @expiration_secs ago. For this, we will have to use the :ets.select/2 function. This construct allows us to apply a guard clause to each entry we are pattern matching, and this is exactly what we want here:

```
iex> expiration_secs = 60
60

iex> expiration_time = :os.system_time(:seconds) - expiration_secs
1518512436
```

```
iex> query = [
...> {
...> {:"$1", {:"$2", :_}},
...> [{:<, :"$2", {:const, expiration_time}}],
...> [:"$1"]
...> }
...> ]
[
{{:"$1", {:"$2", :_}}, [{:<, :"$2", {:const, 1518512436}}], [:"$1"]}
]
```

The query structure isn't that friendly, but don't be wary of it just yet. An :ets.select query is composed of one or more clauses, each clause being a three-element tuple. Let's inspect our single clause query. The first element of the clause, {:"$1", {:"$2", :_}}, is the pattern we will apply to every entry in our table. Here, we are associating the :"$1" variable to the {media_id, search_expression} key and the :"$2" to the created_at tuple element.

The second element, [{:<, :"$2", {:const, expiration_time}}], is the guard clause, and tells us that we will only select entries whose :"$2" value is lower than the expiration_time defined in this scope. The last element of the clause tuple, [:"$1"], shows how we want to return the values of the query. If we wanted to return the full entries stored on the table, we could use the :"$ _" variable instead of :"$1".

With our query in place, let's use it to see which search results have expired. First, let's just cache a new search result so that we can be sure the query is working properly:

```
iex> SearchCacheWorker.cache_search_result("7", "media", "really fresh media 7 contents")
{:ok, true}

iex> :ets.select(:search_cache, query)
[{"1","other"}, {"1","media"}, {"5","media"}]
```

The :ets.select/2 call didn't return the key of the search result we've just cached, so we are good to go. We can now implement the SearchCacheWorker.expired_search_results/0 function, which leverages the code we just created. We encapsulated the query on the private expired_search_results_query/1 just to tidy things up:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/
search_cache_worker.ex
defmodule ElixirDrip.Storage.Workers.SearchCacheWorker do
  # ...
  @search_cache :search_cache
```
