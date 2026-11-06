You may have noticed that we are using the `handle_info/2` function callback to handle the `:expire` message, instead of the `call` and `cast` callbacks we used until now. This is due to the fact that when we do a `Process.send_after/3`, we are sending a *plain* message instead of a message customized by the GenServer behaviour. All of those messages that don't follow the GenServer message protocol will be handled by the available `handle_info/2` callbacks, including our `:expire` message.

![](bbox=[0.12, 0.3, 0.2, 0.4])

For all the other *plain* messages, we simply delegate on the default `handle_info/2` GenServer implementation. This is also important because message tuples such as `{:EXIT, _, _}` and `{:DOWN, _, _, _, _}` may be sent to your GenServer process. Your process will receive the first message when it is trapping exits and you try to end it with `Process.exit/2`. The `:DOWN` message will reach your process when it's monitoring another process, and the latter exits for any reason. If you don't want to explicitly handle these `:EXIT` and `:DOWN` messages yourself, let the default `handle_info/2` do it for you.

Let's see how our expiration mechanism now works:

```
iex> alias ElixirDrip.Storage.Workers.CacheWorker
ElixirDrip.Storage.Workers.CacheWorker

iex> CacheWorker.start_link("5", "some content")
{:ok, #PID<0.904.0>}

#PID<0.904.0>: Terminating cache, served 0 hits # after a minute

iex> {:ok, cache} = CacheWorker.start_link("6", "other content")
{:ok, #PID<0.906.0>}

iex> 1..3 |> Enum.map(fn _ -> GenServer.call(c, :get_media) end)
[ok: "other content", ok: "other content", ok: "other content"]

#PID<0.906.0>: Terminating cache, served 3 hits # after a minute
```

With this in place, we can also add a way to refresh the cache without retrieving its contents. This will allow us to refresh the cache by sending it an asynchronous `:refresh` message using `GenServer.cast(cache_pid, :refresh)`:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/cache_worker.ex
defmodule ElixirDrip.Storage.Workers.CacheWorker do
  use GenServer
  # ...
  def handle_cast(:refresh, %{timer: timer} = state),
    do: {:noreply, %{state | timer: refresh_timer()}}
```