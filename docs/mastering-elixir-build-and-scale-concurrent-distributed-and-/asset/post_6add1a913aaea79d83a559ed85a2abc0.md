
Compared to the upload pipeline supervisor, the download pipeline supervisor only differs on the subscription order between the different stages (now RemoteStorage stage is the second one instead of being the third) and the value of the @direction module attribute, which is now :download.

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/supervisors/download_pipeline_supervisor.ex
defmodule ElixirDrip.Storage.Supervisors.Download.Pipeline do
  use Supervisor
  # ...
  @direction :download
  # ...
  def init(_) do
    remote_storage_subscription = [subscribe_to: [{@starter_name, min_demand: 1, max_demand: 10}]]
    encryption_subscription = [subscribe_to: [{@storage_name, min_demand: 1, max_demand: 10}]]
    notifier_subscription = [subscribe_to: [{@encryption_name, min_demand: 1, max_demand: 10}]]

    Supervisor.init([
      # ...
    ],
    strategy: :rest_for_one,
    name: __MODULE__
    )
  end
end
```

Once more, let's start from the end of the download pipeline, by analyzing its consumer, the Notifier stage.

## The Notifier stage

The Notifier stage of the download pipeline subscribes to the Encryption stage. This stage shares the same Notifier module that also serves the upload pipeline. The difference lies in the process alias given to the download Notifier process (ElixirDrip.Storage.Pipeline.Notifier.Download), and in the new implementation of the notify_step/1 function that now pattern-matches on the :download value for the event type key. Everything else remains untouched:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/notifier.ex
defmodule ElixirDrip.Storage.Pipeline.Notifier do
  # ...
  def start_link(name, subscription_options),
    do: GenStage.start_link(__MODULE__, subscription_options, name: name)

  def init(subscription_options),
    do: {:consumer, @dummy_state, subscription_options}

  def handle_events(tasks, _from, _state) do
    Enum.each(tasks, &notify_step(&1))

    {:noreply, [], @dummy_state}
  end

  defp notify_step(%{media: %{id: id}, content: content, type:
  :download}) do
    # Invoke the notifier instead!

    Logger.debug("#{inspect(self())}: NOTIFICATION! Downloaded media
    #{id}, content: #{inspect(content)}, size: #{byte_size(content)}
    bytes.")
  end
end
```

As you can see, the notify_step/1 function, which handles download events, is currently identical to the one that handles upload events (the only thing that changes is the logged message). We are simply logging a message to the console using the Logger.debug/1 function, but this will change once we actually implement the user notifications with Phoenix Channels.

Let's now check how the Encryption stage handles download events.

## The Encryption stage

On the download pipeline, the Encryption stage subscribes to events coming from the RemoteStorage stage. Similarly to the Notifier stage, the function that handles download events for this stage lives side by side with the function that handles upload events.

This time, however, the Encryption module has two encryption_step/1 functions to handle download events: one that pattern-matches on the :original value of the status event field and one that handles all the other download events, corresponding to those cases when the media contents are still encrypted:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/encryption.ex
defmodule ElixirDrip.Storage.Pipeline.Encryption do
  # ...
  def start_link(name, subscription_options),
    do: GenStage.start_link(__MODULE__, subscription_options, name: name)

  def init(subscription_options),
      do: {:producer_consumer, @dummy_state, subscription_options}

  # the GenStage.handle_events/3 callback
  # delegates the processing of each event to another encryption_step
  /1 function
  # that pattern-matches on %{type: :download}
  def handle_events(tasks, _from, _state) do
      encrypted = Enum.map(tasks, &encryption_step(&1))

      {:noreply, encrypted, @dummy_state}
  end

  # the %{status: :original} indicates the content is ready to be delivered
  defp encryption_step(%{media: media, content: _content, status:
  :original, type: :download} = task),
      do: task

  # given there's no %{status: :original}, we need to decrypt the content
  defp encryption_step(%{media: %{id: id, encryption_key:
  encryption_key},
  content: content, type: :download} = task) do
      Process.sleep(1000)

      clear_content = Provider.decrypt(content, encryption_key)
      Cache.put(id, clear_content)

      %{task | content: clear_content}
  end
end
```

In the first case, the download event reaches the Encryption stage already with the original media contents (because the file was fetched from cache in a previous stage). As such, the encryption_step/1 function simply returns the same event it got. If the download event doesn't have the :original value on the status field, it means the media contents present on the event need to be decrypted with the media encryption_key before emitting a new event downstream. The media contents are cached after being decrypted to speed up further downloads.

## The RemoteStorage stage

It's in the remote_storage_step/1 function of the RemoteStorage stage that we try to find whether there is a CacheWorker for the media contained on the download event. This stage receives download events from the Starter event producer and emits events downstream with the media contents. If there was a CacheWorker for the media, the contents of the event emitted by this stage correspond to the media original contents, therefore, the event is tagged as :original using the status event field; if not, this stage simply fetches the media-encrypted contents from the cloud provider and passes them along:

```elixir
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/
remote_storage.ex
defmodule ElixirDrip.Storage.Pipeline.RemoteStorage do
  # ...
  def start_link(name, subscription_options),
    do: GenStage.start_link(__MODULE__, subscription_options, name: name)

  def init(subscription_options),
    do: {:producer_consumer, @dummy_state, subscription_options}

  # the GenStage.handle_events/3 callback
  # delegates the processing of each event to another
  # remote_storage_step/1 function
  # that pattern-matches on %{type: :download}
  def handle_events(tasks, _from, _state) do
    processed = Enum.map(tasks, &remote_storage_step(&1))

    {:noreply, processed, @dummy_state}
  end

  defp remote_storage_step(%{media: %{id: id, storage_key:
  storage_key}, type: :download} = task) do
    Process.sleep(2000)

    result = case Cache.get(id) do
      nil ->
        {:ok, content} = Provider.download(storage_key)
        %{content: content}

      {:ok, content} ->
        %{content: content, status: :original}
    end

    Map.merge(task, result)
  end
end
```

## The Starter stage

When we previously analyzed how the Starter stage worked for the upload pipeline, we saw how we passed a type as an initialization parameter so that the upload Starter stage knew from which QueueWorker process it should fetch new upload requests. For the download Starter stage, the approach is exactly the same: we initialize the Starter process on the download pipeline supervisor with the @starter_name and @direction arguments, which in this case will have the ElixirDrip.Storage.Pipeline.Starter.Download and :download values. This will be enough for the Starter stage to commence producing events on the download pipeline:

```
iex> alias ElixirDrip.Storage.Pipeline.Common
ElixirDrip.Storage.Pipeline.Common

iex> alias ElixirDrip.Storage.Pipeline.Starter
ElixirDrip.Storage.Pipeline.Starter

iex> starter_name = Common.stage_name(Starter, :download)
ElixirDrip.Storage.Pipeline.Starter.Download

iex> starter_process = Process.whereis(starter_name)
#PID<0.426.0>

iex> Process.alive?(starter_process)
true
```

Since the logic for fetching new download requests from the QueueWorker process and injecting them on the download pipeline is exactly the same as the logic we already used on the upload pipeline, the implementation of the send_events_from_queue/3 function used by the Starter.handle_demand/2 and Starter.handle_info/2 callbacks can be exactly the same. This function can serve both download and upload pipelines because, after retrieving the transfer request (be it an upload or download) from the queue, the call to send_events_from_queue/3 also places the type entry on the event map:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/starter.ex
defmodule ElixirDrip.Storage.Pipeline.Starter do
  # ...
  defp send_events_from_queue(queue, how_many, %{type: type} = state) do
    tasks = queue
      |> QueueWorker.dequeue(how_many)
      |> Enum.map(&Map.put(&1, :type, type))
```
