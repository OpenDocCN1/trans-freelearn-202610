```
09:57:12.371 #PID<0.613.0> Task PID: #PID<0.742.0> returned with 1
result(s) for 6

09:57:12.372 Took: 8.002136 secs
```

It all looks good; just notice that we are sequentially waiting on each task to complete, so we will only run Task.await/1 for the next task when the current Task.await/1 call returns. That is the reason why we immediately see the shell process <0.613.0> waiting for the first task (with PID <0.740.0>), but then it only does the same for the remaining tasks eight seconds afterwards, after the first task returns.

![](bbox=[0.12, 0.32, 0.2, 0.42])

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