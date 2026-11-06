
The following diagram shows a macro perspective of how the ElixirDrip umbrella application will look:

![](img/c636f4cfb07f25f1db535995e50706ed_97_0.png)

We already used the behaviour concept when we used the Application module to define the application module callback of the elixir_drip umbrella application. Let's now understand its purpose.

## Using behaviours

One of the core parts of our project will be how we store and retrieve files on the cloud on behalf of our users. We decided not to place this component under its own umbrella application, because it is possible to define a clear boundary between the ElixirDrip business logic and its storage counterpart under the same umbrella application. To achieve this separation of concerns, we will encapsulate all the storage logic under an ElixirDrip.Storage module. This module will be the single entry point for every storage-related operation, while encapsulating the actual implementation details.

We will start by creating a StorageProvider behaviour, which defines two callbacks to be implemented and relies on typespecs to indicate, for each callback, its arguments and the possible return values. A behaviour allows us to establish the contract that every storage provider will have to comply to. Take a look at this:

```
$ cat apps/elixir_drip/lib/elixir_drip/behaviours/storage_provider.ex
defmodule ElixirDrip.Behaviours.StorageProvider do
  @type path :: binary()
  @type content :: bitstring()
  @type reason :: atom()
  @type details :: map()

  @callback upload(path, content) ::
      {:ok, :uploaded}
      | {:error, reason}
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
$ cat apps/elixir_drip/lib/elixir_drip/storage/providers/google_cloud_storage/local.ex
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

![](img/c636f4cfb07f25f1db535995e50706ed_98_0.png)

We selected Google Cloud Storage as the storage provider for ElixirDrip. Since its access logic is completely abstracted via a `StorageProvider` custom behaviour, it will be possible to easily switch in the future to other storage provider. This hypothetical new provider would only need to respect the same `StorageProvider` behaviour, hence it would need to implement both `upload/2` and `download/1` callbacks.

Deciding between the Local or Live implementation will be just a matter of adjusting the global or production configuration files. This is a common pattern applied when we want to inject different modules depending on the current Mix environment:

```
$ cat apps/elixir_drip/config/config.exs
use Mix.Config
  # ...
config :elixir_drip,
  storage_provider: ElixirDrip.Storage.Providers.GoogleCloudStorageLocal

import_config "#{Mix.env()}.exs"
```

As you can see from the following code, in the prod.exs configuration, we point to the ElixirDrip.Storage.Providers.GoogleCloudStorageLive module that will actually interact with the cloud storage provider. Since this configuration file is imported after the main config.exs configuration file, it will override the previous storage_provider configuration entry:

```
$ cat apps/elixir_drip/config/prod.exs
use Mix.Config
  # ...
config :elixir_drip,
  storage_provider: ElixirDrip.Storage.Providers.GoogleCloudStorageLive
```

The last piece of the puzzle is the creation of a generic ElixirDrip.Storage.Provider module that will delegate on the StorageProvider implementation designated by the :storage_provider configuration entry. This module hides the actual storage provider implementation from the caller, by delegating a Provider.upload/2 or Provider.download/1 call to the module returned by the Application.get_env(elixir_drip, :storage_provider) configuration.

Because we are using the defdelegate/2 macro, Elixir expects the @target module to define the upload/2 and download/1 public functions, otherwise the delegation won't work:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/provider.ex
defmodule ElixirDrip.Storage.Provider do
  @moduledoc false

  @target Application.get_env(:elixir_drip, :storage_provider)

  defdelegate upload(path, content), to: @target
  defdelegate download(path), to: @target
end
```

Previously, we decided to expose every storage-related function via an `ElixirDrip.Storage` module, which represents our `Storage` context. As such, this context will rely on the previous `ElixirDrip.Storage.Provider` to interact with our storage provider.

![](img/c636f4cfb07f25f1db535995e50706ed_100_0.png)

If we don't want to have this intermediary `ElixirDrip.Storage.Provider` module delegating to the actual storage provider module, we could immediately fetch the `Local` or `Live` module from the configuration (that is to say, using `Application.get_env(:elixir_drip, :storage_provider)`) in the consumer `ElixirDrip.Storage` module.

This context will do a lot more in a couple of chapters, but, for now, it only exposes two functions, `store/2` and `retrieve/1`, both relying on the generic storage provider:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/storage.ex
defmodule ElixirDrip.Storage do
  alias ElixirDrip.Storage.Media
  alias ElixirDrip.Storage.Provider

  def store(%Media{} = media, content) do
    media
    |> generate_storage_key()
    |> Map.get(:storage_key)
    |> Provider.upload(content)
  end

  def retrieve(%Media{storage_key: storage_key}) do
    Provider.download(storage_key)
  end

  # ...
end
```

We will now look at our ElixirDrip application through the lens of the `xref` tool.

## Viewing cross references with xref

Since Elixir 1.3, every time you compile your code, you are also using `xref` to find calls to non-existent modules and functions. If the module we were pointing to in the ElixirDrip application configuration (`config/config.exs`) had a typo, we would get the following output:

```
$ mix clean && mix compile
==> elixir_drip
Compiling 10 files (.ex)
warning: function ElixirDrip.Storage.Providers.Typo.upload/2 is undefined
(module ElixirDrip.Storage.Providers.Typo is not available)
lib/elixir_drip/storage/provider.ex:6

warning: function ElixirDrip.Storage.Providers.Typo.download/1 is undefined
(module ElixirDrip.Storage.Providers.Typo is not available)
lib/elixir_drip/storage/provider.ex:7

Generated elixir_drip app
==> elixir_drip_web
Compiling 11 files (.ex)
Generated elixir_drip_web app
```

Behind the scenes, the compiler runs mix xref warnings, which results in those two warnings that let us know the generic provider is delegating the upload/2 and download/1 functions to a missing module.

Another useful functionality of xref is the graph mode, which shows how the application files depend on one another by printing the application dependency graph. Let's generate the dependency graph of our umbrella project:

```
$ mix xref graph --format dot
==> elixir_drip
Generated "xref_graph.dot" in the current directory. To generate a PNG:
    dot -Tpng xref_graph.dot -o xref_graph.png

For more options see http://www.graphviz.org/.
==> elixir_drip_web
Generated "xref_graph.dot" in the current directory. To generate a PNG:
    dot -Tpng xref_graph.dot -o xref_graph.png

For more options see http://www.graphviz.org/.
```

xref detected we had an umbrella project, and so it generated the dependency graph for each umbrella application.
