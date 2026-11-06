```
```elixir
when demand > 0 do
  total_demand = demand + pending

  send_events_from_queue(queue, total_demand, state)
end

defp send_events_from_queue(queue, how_many, %{type: type} = state) do
  tasks = queue
    |> QueueWorker.dequeue(how_many)
    |> Enum.map(&Map.put(&1, :type, type))

  if length(tasks) < how_many do
    Process.send_after(self(), :try_again, @queue_polling)
  end

  {:noreply, tasks, %{state | pending: how_many - length(tasks)}}
end
end
```
```

You may have noticed that we also implemented a handle_info/2 callback to react to a :try_again message. When this message arrives, the process will try again to fulfill its pending demand. We are sending that message using the Process.send_after/3 function in the send_events_from_queue/3 function, after checking whether the events we got from the queue are enough to satisfy the current demand. Why is this needed?

If we didn't have this polling strategy in place, our Starter producer would never fulfill the pending demand on its own. Consider the following NaiveStarter module, similar to the Starter module except that it doesn't send any :try_again messages to itself:

```
```elixir
defmodule Pipeline.NaiveStarter do
  # ...
  defp send_events_from_queue(queue, how_many, %{type: type} = state) do
    tasks = queue
      |> QueueWorker.dequeue(how_many)
      |> Enum.map(&Map.put(&1, :type, type))

    # Don't send any :try_again message
    # if length(tasks) < how_many do
    # Process.send_after(self(), :try_again, @queue_polling)
    # end

    {:noreply, tasks, %{state | pending: how_many - length(tasks)}}
  end
end
```
```