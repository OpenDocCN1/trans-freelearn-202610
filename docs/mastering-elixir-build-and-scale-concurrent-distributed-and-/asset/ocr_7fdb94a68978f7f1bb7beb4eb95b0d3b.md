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

Let's now implement our text search feature using the Task.async_stream/5 function so that we can be sure it only waits 10 seconds for task results. This function gives us the semantics we associate with the Stream module, in the sense that each task will be lazily performed. By default, the number of running tasks at the same time is given by the numbers of schedulers in the VM. We can also control this via the :max_concurrency option. Compare this approach with the previous one, where we spawned all the tasks in advance, disregarding the number of available schedulers. If each task needs to use a finite resource, such as a database connection, the eager approach with many tasks could quickly deplete the pool of available resources:

```
defmodule ElixirDrip.Storage.Search do
  # ...
  def task_stream_search_for(media_ids, expression, concurrency
```