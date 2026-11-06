```
iex> guardian = pid("0.672.0")
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

# Disk-based ETS

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