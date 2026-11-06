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
```