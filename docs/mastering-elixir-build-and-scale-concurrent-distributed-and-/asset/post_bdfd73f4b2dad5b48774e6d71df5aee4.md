
```
encrypted = Enum.map(tasks, &encryption_step(&1))

{:noreply, encrypted, @dummy_state}
end

defp encryption_step(%{media: %{id: id, encryption_key:
encryption_key},
content: content, type: :upload} = task) do
  Process.sleep(1000)

  Cache.put(id, content)

  %{task | content: Provider.encrypt(content, encryption_key)}
end
end
```

However, this encryption_step/1 function does more than its RemoteStorage.remote_storage_step/1 counterpart. Before actually encrypting the media contents, this stage caches the clear contents by spawning a CacheWorker process with the Cache.put/2 call. Without getting into too much detail, this will allow us to short-circuit a download pipeline if the media being downloaded is already present in memory. If we try to download media that was uploaded seconds before, the response will be much faster since no decryption will have to take place. The downside of this strategy is the additional memory that CacheWorker uses to keep the media contents in memory for a while.

Let's now look at the Starter stage, which, as its name hints, is the one that actually triggers the whole Upload pipeline.

## The Starter stage

We've finally reached the first stage of this upload pipeline walk-through. As an event producer, our Starter stage will implement the handle_demand/2 callback instead of the handle_events/3 we've implemented so far for our Consumer and ProducerConsumer stages. This will also be the only stage that will keep state. As a Producer stage, the Starter stage needs to know at all times the queue from which it fetches new upload tasks and the outstanding demand it received and it has not yet fulfilled. Accordingly, its init/1 callback will return a {:producer, state} tuple where the state contains the queue, the type of events this Starter stage will create, and the pending demand.

![](img/c636f4cfb07f25f1db535995e50706ed_205_0.png)

The call to `QueueWorker.queue_name/1` with `:upload` will return the `QueueWorker` process name that contains the upload queue, similar to how we used the `Pipeline.Common.stage_name/1` function to generate the process names for each stage of the pipeline.

The `Encryption` stage we just saw subscribes to the `Starter` stage. Demand is sent upstream when the `Encryption` stage starts, and consequently the `Starter` stage receives demand through the `handle_demand/2` callback. The first argument passed to the callback is an integer corresponding to the number of events the subscriber is able to receive, whereas the other argument is the process state.

Every time a new demand arrives, the `Starter` stage tries to produce as many events as possible to fulfill the total demand, since it considers not only the new demand it got but also the pending demands it had from previous calls. It then tries to pop up the required number of events from the queue, emitting all of those to start the upload process. If the events fetched from the queue aren't enough to fulfill the total demand, we keep the outstanding demand on the process state:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/starter.ex
defmodule ElixirDrip.Storage.Pipeline.Starter do
  use GenStage
  require Logger
  alias ElixirDrip.Storage.Workers.QueueWorker

  @queue_polling 5000

  def start_link(name, type),
    do: GenStage.start_link(__MODULE__, type, name: name)

  def init(type) do
    {
      :producer,
      %{queue: QueueWorker.queue_name(type), type: type, pending: 0}
    }
  end

  # implementing the GenServer.handle_info/2 callback
  # to check if there are more events in the queue to fulfill
  # pending demand
  def handle_info(:try_again, %{queue: queue, pending: demand} = state),
    do: send_events_from_queue(queue, demand, state)

  # implementing the GenStage.handle_demand/2 callback
  # we end up using the send_events_from_queue/1 function
  # to respond to the received demand
  def handle_demand(demand, %{queue: queue, pending: pending} = state)
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

After initialization, the `Encryption` stage asks the `NaiveStarter` stage for 10 events and waits for new events. As a consequence, the `handle_demand/2` callback is called and the `NaiveStarter` producer tries to fetch events from the queue. Nonetheless, there were only four events available, so the `NaiveStarter` stage sends those four events downstream and updates its `pending` demand to six. The `Encryption` stage will then receive and process those four events. Given its `min_threshold` of three and the fact it only got four of the 10 events it previously asked for the `Encryption` stage won't send any new demands upstream. From the `NaiveStarter` stage perspective, things don't look good either: since it only reacts to new demands, it will never try to push new events to the queue if the `Encryption` stage doesn't ask for them. In this *deadlock* scenario, the `Encryption` stage will sit forever waiting for the remaining six events, and the `NaiveStarter` stage will sit forever waiting for new demand.

That is the reason why our `Starter` stage routinely checks whether the queue has enough events to fulfill the pending demand, and if there are any events, it immediately pushes them downstream. After all, if we registered the demand, we can be sure the subscriber is waiting for events. A corollary that follows is that subscribers only communicate their needs once, so your producers have to not only keep track of pending demands but also respond to them as soon as possible.

We will now perform the same walk-through for the download pipeline.

## The download pipeline

Similar to the previous pipeline, the download pipeline is also composed by the same four stages: `Starter`, `RemoteStorage`, `Encryption`, and `Notifier`. However, this time the `Encryption` stage is performed after we obtain the actual encrypted contents stored on the cloud. This pipeline starts with a download request being enqueued. Then, the `Starter` stage fetches this request from the queue and produces an event from it, which will be processed by the `RemoteStorage` stage. This stage checks whether there is already a `CacheWorker` process for the requested media. If that's the case, it places the clear cached contents on the event and sets the event `status` to `:original`, informing the subsequent stages that the event already contains the *original* media contents. When the `:download` event reaches the `Encryption` stage, the content is only decrypted if the `:original` `status` isn't present on the event. In the end, the `Notifier` step tells the user that the media is now ready to be downloaded.
