```
07:23:08.252 #PID<0.895.0>: Received :get_media and served
"Content of media 8." bytes 7 times.

Seconds elapsed: 8 secs
Seconds elapsed: 9 secs

** (exit) exited in: Task.Supervised.stream(10000)
  ** (EXIT) time out
(elixir) lib/task/supervised.ex:269: Task.Supervised.stream_reduce/7
(elixir) lib/enum.ex:1919: Enum.map/2
(elixir_drip) lib/elixir_drip/storage/search.ex:44:
ElixirDrip.Storage.Search.task_stream_search_for/4
(stdlib) timer.erl:166: :timer.tc/1
(elixir_drip) lib/elixir_drip/utils.ex:13: ElixirDrip.Utils.measure/1

Seconds elapsed: 10 secs
```

What we want is to return after 10 seconds with the results we have, instead of not returning at all because one task didn't end before the bell. For situations like these, we have the :on_timeout option. Its value is :exit by default, meaning it will propagate the exit signal from the task, thus the process that spawned the tasks will exit too. This is the behavior we just observed. We can also pass it as :kill_task, and the task that took too long is simply ended. When a task is terminated like this, the result it yields will always be a {:exit, :timeout} tuple, instead of the successful {:ok, result}:

```
defmodule ElixirDrip.Storage.Search do
  # ...
  def safe_task_stream_search_for(media_ids, expression, concurrency \\ 4, timeout \\ 10_000) when is_list(media_ids) do
    options = [max_concurrency: concurrency, timeout: timeout,
    on_timeout: :kill_task]

    media_ids
    |> Task.async_stream(__MODULE__, :search_for, [expression],
    options)
    |> Enum.map(&elem(&1, 1))
    |> Enum.reject(&(&1 == :timeout))
    |> Enum.into(%{})
  end
end
```

Recall that the search task for media "25" timed out and therefore it yielded a {:exit, :timeout} tuple, that will be then filtered with the call to Enum.reject/2 before returning the results. With this version, a search on media with ids "25", "8", "6", and "7" yields the following after 10 seconds: