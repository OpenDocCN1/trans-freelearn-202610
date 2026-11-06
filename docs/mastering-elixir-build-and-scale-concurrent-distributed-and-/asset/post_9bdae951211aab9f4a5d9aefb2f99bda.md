
We will now implement our first search, properly named naive_search_for/2, since it simply searches through each piece of media sequentially:

```
defmodule ElixirDrip.Storage.Search do
  # ...
  def naive_search_for(media_ids, expression) when is_list(media_ids) do
    media_ids
    |> Enum.map(&search_for(&1, expression))
    |> Enum.into(%{})
  end
end
```

The preceding implementation doesn't leverage at all how easy is for Elixir to spawn millions of processes. Let's fill our cache with data so that we can implement an improved search:

```
iex> alias ElixirDrip.Storage.Supervisors.SlowCacheSupervisor, as: Cache
ElixirDrip.Storage.Supervisors.SlowCacheSupervisor

iex> fill_cache = fn (how_many, offset) ->
...> offset..(offset + (how_many-1))
...> |> Enum.each(&Cache.put(to_string(&1), "Media content #{&1}."))
...> end
#Function<12.99386804/2 in :erl_eval.expr/5>

iex> fill_cache.(3, 6)
14:17:40.483 [debug] Spawning a slow cache for 6, content size: 16 bytes.
14:17:40.485 [debug] #PID<0.452.0>: CacheWorker started. Will expire in a while.
14:17:40.485 [debug] Spawning a slow cache for 7, content size: 16 bytes.
14:17:40.485 [debug] #PID<0.453.0>: CacheWorker started. Will expire in a while.
14:17:40.485 [debug] Spawning a slow cache for 8, content size: 16 bytes.
14:17:40.485 [debug] #PID<0.454.0>: CacheWorker started. Will expire in a while.
:ok
```

With the previous snippet, we stored three files with ids "6", "7" and "8" in the cache. If we then retrieve those files from the cache in parallel, we will have to wait eight seconds before getting the contents of the three files.

![](img/c636f4cfb07f25f1db535995e50706ed_168_0.png)

Recall that the SlowCacheSupervisor.get/1 function sleeps for the number of seconds indicated by the media id. If we are calling this function in parallel, the time it takes for all calls to return is equal to the time of the slowest call.

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

```
09:57:12.371 #PID<0.613.0> Task PID: #PID<0.742.0> returned with 1
result(s) for 6

09:57:12.372 Took: 8.002136 secs
```

It all looks good; just notice that we are sequentially waiting on each task to complete, so we will only run Task.await/1 for the next task when the current Task.await/1 call returns. That is the reason why we immediately see the shell process <0.613.0> waiting for the first task (with PID <0.740.0>), but then it only does the same for the remaining tasks eight seconds afterwards, after the first task returns.

![](img/c636f4cfb07f25f1db535995e50706ed_170_0.png)

The task_search_for_verbose/2 function called on the previous example is equivalent to the task_search_for/2 function, adding only calls to Logger.debug/1, so we can observe what's happening under the hood.

Now, consider the following situation: we have to return after 10 seconds, no matter what, and we have a really big file in the cache with id "25", so it will take 25 seconds to be fetched from our slow cache. If we call Search.task_search_for(["8", "25"], "media"), it will terminate the current process, which spawned the two search tasks, because the second task doesn't end before the timeout (and we aren't trapping exits). However, as you can see in the following code, it ends after 18 seconds, because it waited eight seconds for the first task to complete, and then it blocks for another 10 seconds (the timeout passed on the await/1 call) before returning from the Task.await/1 for the second task:

```
iex> Logger.debug "#{inspect(self())} Start"; ElixirDrip.Utils.measure(fn -> Search.task_search_for_verbose(["8", "25"], "media") end)
```

```
12:10:06.878 #PID<0.613.0> Start

12:10:06.878 #PID<0.613.0> Spawned a search for 8, PID: #PID<0.947.0>
12:10:06.878 #PID<0.613.0> Spawned a search for 25, PID: #PID<0.948.0>
12:10:06.878 #PID<0.613.0> Will now wait for task PID: #PID<0.947.0>
12:10:06.878 Fetching cached content for 8...
12:10:06.878 Fetching cached content for 25...

Seconds elapsed: 1 secs
# ...
Seconds elapsed: 7 secs

12:10:14.879 #PID<0.562.0>: Received :get_media and served '"Content of media 8."' bytes 1 times.
12:10:14.882 #PID<0.613.0> Task PID: #PID<0.947.0> returned with 1
result(s) for 8
```

```
12:10:14.882 #PID<0.613.0> Will now wait for task PID: #PID<0.948.0>

Seconds elapsed: 8 secs
# ...
Seconds elapsed: 17 secs

** (exit) exited in: Task.await(%Task{owner: #PID<0.613.0>, pid: #PID<0.948.0>, ref: #Reference<0.3922080717.158334979.65414>}, 10000)
  ** (EXIT) time out
  (elixir) lib/task.ex:493: Task.await/2
  (elixir_drip) lib/elixir_drip/storage/search.ex:34: anonymous fn/1 in ElixirDrip.Storage.Search.task_search_for_verbose/2
  (elixir) lib/enum.ex:1294: Enum."-map/2-lists^map/1-0-"/2
  (elixir) lib/enum.ex:1294: Enum."-map/2-lists^map/1-0-"/2
  (elixir_drip) lib/elixir_drip/utils.ex:19: anonymous fn/1 in ElixirDrip.Utils.safe_function/1
  (stdlib) timer.erl:166: :timer.tc/1
  (elixir_drip) lib/elixir_drip/utils.ex:11: ElixirDrip.Utils.measure/1
```

If we want to timeout after a while using the previous approach, we would need to keep track of the remaining time until timeout and only wait for each task accordingly. Fortunately, spawning parallel tasks is a really common pattern in concurrent programming, and therefore, Elixir 1.4 introduced the Task.async_stream functions, which let us, among other nice features, set a single timeout for all the tasks it spawns.

The Task.async_stream functions will spawn a new process, linked to the current one, and this intermediate process will then spawn all the required tasks, aggregating the task results as they conclude. We are still in Task territory, so the default behavior should be familiar: if a spawned task times out or unexpectedly dies, its exit signal will be sent to the intermediate process that will then terminate the remaining tasks and propagate the exit signal to the original process.

Let's now implement our text search feature using the Task.async_stream/5 function so that we can be sure it only waits 10 seconds for task results. This function gives us the semantics we associate with the Stream module, in the sense that each task will be lazily performed. By default, the number of running tasks at the same time is given by the number of schedulers in the VM. We can also control this via the :max_concurrency option. Compare this approach with the previous one, where we spawned all the tasks in advance, disregarding the number of available schedulers. If each task needs to use a finite resource, such as a database connection, the eager approach with many tasks could quickly deplete the pool of available resources:

```
defmodule ElixirDrip.Storage.Search do
  # ...
  def task_stream_search_for(media_ids, expression, concurrency
```
