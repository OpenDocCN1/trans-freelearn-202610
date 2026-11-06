
```
@expiration_secs 60

def expired_search_results(expiration_secs \\ @expiration_secs) do
  query = expired_search_results_query(expiration_secs)

  :ets.select(@search_cache, query)
end

defp expired_search_results_query(expiration_secs) do
  expiration_time = :os.system_time(:seconds) - expiration_secs

  [
    {
      {:"$1", {:"$2", :"_"}},
      [{:<, :"$2", {:const, expiration_time}}],
      [:"$1"]
    }
  ]
end
```

Even if we can now understand the syntax of these :ets.select/2 queries, their funky expressiveness means that in a month or two, we will probably need a moment to grasp what a query like this does. To help with this difficulty, the :ets Erlang module provides the :ets.fun2ms/1 function, which receives a lambda function and outputs a query with the proper syntax to be passed to the :ets.select/3 function. This is how we could obtain an equivalent query that gives us the entries that expired:

```
iex> query = :ets.fun2ms(
...> fn {key, {created_at, result}} when created_at < expiration_time ->
...>   key
...> end)

[
  {
    {:"$1", {:"$2", :"$3"}},
    [{:<, :"$2", {:const, 1518520114}}],
    [:"$1"]
  }
]
```

As you can see, the :ets.fun2ms/1 call is more intelligible than what we had. It receives a table entry as its single argument, it applies a guard to the created_at field to filter out still valid entries, and then returns the key of the expired ones. There is a catch, though: if you call :ets.fun2ms/1 inside a module of yours, you will get a :badarg error, saying the function we passed has to be a function generated in the shell, or needs to be transformed with an Erlang parse transform before it can be used.

The *parse transform* is a feature of Erlang that allows one to access the code tree representation, a sort of Erlang metaprogramming. The introduction of Elixir 1.5 deprecated the use of these *parse transform* constructs, so we are left with using `:ets.fun2ms/1` with shell functions, like we just did previously.

![](img/c636f4cfb07f25f1db535995e50706ed_182_0.png)

If you still want to use an `:ets.fun2ms/1`-based approach, check the `ex2ms` package (https://hex.pm/packages/ex2ms), which allows you to accomplish the same using Elixir macros. For our specific use-case, it was enough to generate the `select` query inside an IEx shell with `:ets.fun2ms/1` and then just copy the result to `SearchCacheWorker.expired_search_results_query/1`.

Let's now create a way to delete entries from our search results cache. Since this operation changes the ETS table, it will have to be performed by its processor owner, hence the use of the `GenServer.call/2` function. We have two `delete_cache_search` functions, one that creates the tuple key with the received `media_id` and `search_expression` to then perform the `GenServer.call/2`, while the other just passes the received key list to it:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/
search_cache_worker.ex
defmodule ElixirDrip.Storage.Workers.SearchCacheWorker do
  # ...
  def delete_cache_search(media_id, search_expression) do
    GenServer.call(__MODULE__, {:delete, [{media_id, search_expression}]})
  end

  def delete_cache_search(keys) when is_list(keys) and
    length(keys) > 0 do
    GenServer.call(__MODULE__, {:delete, keys})
  end

  def handle_call({:delete, keys}, _from, search_cache)
    when is_list(keys) do
      result = delete(keys)
        |> Enum.reduce(true, fn r, acc -> r && acc end)

      {:reply, {:ok, result}, search_cache}
  end

  defp delete([]), do: []
  defp delete([key|rest]),
    do: [delete(key)] ++ delete(rest)
  defp delete(key), do: :ets.delete(@search_cache, key)
```

The delete_cache_search calls are both handled by the same handle_call/3 callback that pattern matches on {:delete, keys}. This handler relies on the delete/1 private function to actually execute the :ets.delete/2 call, recursively iterating on the list of keys it receives. Also note that we are reducing the results of all delete operations just for the sake of it, since :ets.delete/2 always returns true no matter what.

We can now pipe the results of SearchCacheWorker.expired_search_results/0 to SearchCacheWorker.delete_cache_search/1 in order to sweep those older cached search results:

```
iex> SearchCacheWorker.expired_search_results
[{"1", "other"}, {"1", "media"}, {"5", "media"}]

iex> SearchCacheWorker.expired_search_results() |> SearchCacheWorker.delete_cache_search()
{:ok, true}

iex> SearchCacheWorker.expired_search_results
[]
```

Considering we are using a GenServer as the ETS table guardian, we could implement the same approach we previously adopted on the CacheWorker to expire itself, but this time we would periodically sweep old cached results instead. We will leave this improvement as a suggestion for the reader.

Our SearchCacheWorker lives happily in memory and performs as expected, providing a constant access time to the millions of search results it stores. But as you can see in the following code, an ETS table lives in memory and it will go to /dev/null as soon as its process owner dies:

```
iex> :ets.info(:search_cache)
[
  read_concurrency: false,
  write_concurrency: false,
  compressed: false,
  memory: 331,
  owner: #PID<0.672.0>,
  heir: :none,
  name: :search_cache,
  size: 1,
  node: :nonode@nohost,
  named_table: true,
  type: :set,
  keypos: 1,
  protection: :protected
]
```

```
iex> guardian = #PID<0.672.0>
#PID<0.672.0>

iex> Process.alive? guardian
true

iex> Process.exit guardian, :bye
true

iex> Process.alive? guardian
false

iex> :ets.info(:search_cache)
:undefined
```

Let's now check how can we persist an ETS table in disk.

## Disk-based ETS

There may be scenarios where you want the information stored on the ETS table to be persisted in disk. You lose the performance of an in-memory data structure, but you get your information stored to disk if anything goes south. To cater for this case, Erlang provides DETS, a disk-based ETS solution that abides almost completely to the same ETS contract you already know and expect. You can't use it if you need an :ordered_set table since you're limited to 2 GB of data; instead, you will have to start it in a different way and properly close it, and that's essentially it.

Let's now implement a FlexibleSearchCacheWorker module that, depending on the argument passed to its start_link/1 function, uses a DETS or an ETS to store search results. This module will implement all the functions provided by SearchCacheWorker, but the changes we'll need to do are very small:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/
flexible_search_cache_worker.ex
defmodule ElixirDrip.Storage.Workers.FlexibleSearchCacheWorker do
  use GenServer
  require Logger

  @search_cache :search_cache

  def start_link(storage \\ :ets) do
    GenServer.start_link(__MODULE__, storage, name: __MODULE__)
  end

  def init(storage) do
```
```elixir
Logger.debug("#{inspect(self())}: FlexibleSearchCacheWorker started with #{storage}.")
search_cache = case storage do
  :ets -> :ets.new(@search_cache, [:named_table, :set,
    :protected])
  :dets ->
    {:ok, name} = :dets.open_file(@search_cache, [type: :set])
    name
end

{:ok, {storage, search_cache}}
end
end
```

As you can see, depending on the storage value (one of :ets or :dets), we initialize an ETS or a DETS table. We then save both the storage and search_cache values on the process state. By default, the :dets.open_file/2 function creates a file with the table name if it doesn't exist yet. The next time we start a DETS-based FlexibleSearchCacheWorker process, it will read from the file instead of creating a table from scratch.

Recall that
ElixirDrip.Storage.Workers.FlexibleSearchCacheWorker is actually a module alias that gets expanded to an atom (for example, :"Elixir.ElixirDrip.Storage.Workers.FlexibleSearchCacheWorker"). Likewise, all Erlang modules are represented as atoms. This is the reason why we use :ets.new(...) to call the new/2 function of the :ets module and also why we can call functions like storage.lookup(...).

Let's now see the changes we had to do for FlexibleSearchCacheWorker so that it also has a search_result_for/2 function. Previously, instead of using a GenServer.call/2, we were directly doing a lookup on SearchCacheWorker.search_result_for/2. Here, we are using the GenServer approach because we need the storage value from the process state in order to know which Erlang module will handle our function call. The rest stays the same. This is only possible because the :ets and :dets APIs are interchangeable:

```
```bash
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/
flexible_search_cache_worker.ex
```
```elixir
defmodule ElixirDrip.Storage.Workers.FlexibleSearchCacheWorker do
  # ...
  def search_result_for(media_id, search_expression) do
    GenServer.call(__MODULE__, {:search_result_for, media_id,
    search_expression})
```
