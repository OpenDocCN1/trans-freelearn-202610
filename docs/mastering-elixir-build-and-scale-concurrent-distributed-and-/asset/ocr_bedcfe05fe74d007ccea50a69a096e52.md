```
| {:error, reason, details}
@callback download(path) ::
  {:ok, content}
| {:error, reason}
| {:error, reason, details}
end
```

We will then implement this behaviour with two modules, a `Local` and a `Live` module, that will be used in local development and production, respectively. Using a behaviour ensures our modules always abide to the same contract, whether they are the previous two modules or the mock module that we'll use to fake the actual call to the provider when creating our tests in Chapter 9, Finding Zen through Testing.

Each implementation module only needs to annotate the behaviour it implements by using the `@behaviour` module attribute. From now on, for every module that materializes the behaviour, the compiler will check whether it implements the behaviour callbacks with the correct number of arguments:

```
$ cat
apps/elixir_drip/lib/elixir_drip/storage/providers/google_cloud_storage/local.ex
defmodule ElixirDrip.Storage.Providers.GoogleCloudStorageLocal do
  @behaviour ElixirDrip.Behaviours.StorageProvider

  require Logger

  def upload(path, content) do
    Logger.debug("Uploading #{inspect(byte_size(content))} bytes to Google Cloud Storage, path: #{path}")
    {:ok, :uploaded}
  end

  def download(path) do
    Logger.debug("Downloading #{path} from Google Cloud Storage")
    {:ok, "downloaded_content"}
  end
end
```

![](bbox=[0.12, 0.75, 0.22, 0.85])

We selected Google Cloud Storage as the storage provider for ElixirDrip. Since its access logic is completely abstracted via a `StorageProvider` custom behaviour, it will be possible to easily switch in the future to other storage provider. This hypothetical new provider would only need to respect the same `StorageProvider` behaviour, hence it would need to implement both `upload/2` and `download/1` callbacks.