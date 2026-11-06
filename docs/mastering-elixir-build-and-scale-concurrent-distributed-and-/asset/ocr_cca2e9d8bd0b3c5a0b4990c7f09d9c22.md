Our improved search will spawn a task for each search and then wait for all of them afterwards. Notice that we are using the Task.async/3 function, which receives the module, function name, and arguments, instead of a lambda like we used previously:

```
defmodule ElixirDrip.Storage.Search do
  # ...
  def task_search_for(media_ids, expression, timeout \\ 10_000) when
  is_list(media_ids) do
    media_ids
    |> Enum.map(&Task.async(__MODULE__, :search_for, [&1, expression]))
    |> Enum.map(&Task.await(&1, timeout))
  end
end
```

You can see how it performs in the following code. The complete call took eight seconds to return the results: 1 ms after it started, each task tried to fetch the cache contents and then the three cache workers responded in 6, 7, and 8 seconds afterwards:

```
iex> Logger.debug "#{inspect(self())} Start"; ElixirDrip.Utils.measure(fn -> Search.task_search_for_verbose(["8", "7", "6"], "media") end)

09:57:04.368 #PID<0.613.0> Start

09:57:04.369 #PID<0.613.0> Spawned a search for 8, PID: #PID<0.740.0>
09:57:04.369 #PID<0.613.0> Spawned a search for 7, PID: #PID<0.741.0>
09:57:04.369 #PID<0.613.0> Spawned a search for 6, PID: #PID<0.742.0>

09:57:04.369 #PID<0.613.0> Will now wait for task PID: #PID<0.740.0>

09:57:04.369 Fetching cached content for 8...
09:57:04.369 Fetching cached content for 7...
09:57:04.369 Fetching cached content for 6...

09:57:10.370 #PID<0.560.0>: Received :get_media and served '"Content of media 6."' bytes 1 times.
09:57:11.370 #PID<0.561.0>: Received :get_media and served '"Content of media 7."' bytes 1 times.
09:57:12.370 #PID<0.562.0>: Received :get_media and served '"Content of media 8."' bytes 1 times.

09:57:12.371 #PID<0.613.0> Task PID: #PID<0.740.0> returned with 1 result(s) for 8
09:57:12.371 #PID<0.613.0> Will now wait for task PID: #PID<0.741.0>

09:57:12.371 #PID<0.613.0> Task PID: #PID<0.741.0> returned with 1 result(s) for 7
09:57:12.371 #PID<0.613.0> Will now wait for task PID: #PID<0.742.0>
```