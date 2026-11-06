```
{:noreply, processed, @dummy_state}
end

defp remote_storage_step(%{media: %{id: id, storage_key: storage_key} = media, content: content, type: :upload} = task) do
  Process.sleep(2000)

  Logger.debug("#{inspect(self())}: Uploading media #{id} to #{storage_key}, size: #{byte_size(content)} bytes.")

  {:ok, :uploaded} = Provider.upload(storage_key, content)

  %{task | media: Storage.set_upload_timestamp(media)}
end
end
```

Each file stored in ElixirDrip is encrypted before being uploaded to the cloud. Let's now check the Encryption stage that precedes the RemoteStorage stage we have just analyzed.

# The Encryption stage

The Encryption module of the upload pipeline is also a ProducerConsumer stage, therefore it will implement the handle_events/3 callback as well. Similar to the previous stage, it doesn't keep state and transforms each event it receives using its own encryption_step/1 function:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/encryption.ex
defmodule ElixirDrip.Storage.Pipeline.Encryption do
  @dummy_state []

  use GenStage
  require Logger
  alias ElixirDrip.Storage.Providers.Encryption.Simple, as: Provider
  alias ElixirDrip.Storage.Supervisors.CacheSupervisor, as: Cache

  def start_link(name, subscription_options),
    do: GenStage.start_link(__MODULE__, subscription_options, name: name)

  def init(subscription_options),
    do: {:producer_consumer, @dummy_state, subscription_options}

  # implementing the GenStage.handle_events/3 callback
  # we delegate the event processing to the encryption_step/1 function
  def handle_events(tasks, _from, _state) do
```