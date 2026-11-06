# The RemoteStorage stage

It's in the remote_storage_step/1 function of the RemoteStorage stage that we try to find whether there is a CacheWorker for the media contained on the download event. This stage receives download events from the Starter event producer and emits events downstream with the media contents. If there was a CacheWorker for the media, the contents of the event emitted by this stage correspond to the media original contents, therefore, the event is tagged as :original using the status event field; if not, this stage simply fetches the media-encrypted contents from the cloud provider and passes them along:

```
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
```