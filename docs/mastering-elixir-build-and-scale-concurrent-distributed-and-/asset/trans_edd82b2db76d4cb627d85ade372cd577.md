
下图展示了 ElixirDrip umbrella 应用的宏观视角：

![](img/c636f4cfb07f25f1db535995e50706ed_97_0.png)

我们在用 Application 模块为 elixir_drip umbrella 应用定义应用模块回调时已经使用过 behaviour 概念。现在让我们了解其目的。

## 使用 behaviours

我们项目的核心部分之一是如何代表用户在云端存储和检索文件。我们决定不将此组件放在其自己的 umbrella 应用下，因为可以在同一个 umbrella 应用下在 ElixirDrip 业务逻辑与其存储对应部分之间定义清晰的边界。为了实现关注点分离，我们将把所有存储逻辑封装在 ElixirDrip.Storage 模块下。该模块将成为每个存储相关操作的单一入口点，同时封装实际的实现细节。

我们将首先创建一个 StorageProvider behaviour，它定义了两个需要实现的回调，并依赖 typespecs 来指示每个回调的参数及其可能的返回值。behaviour 使我们能够建立每个存储提供者必须遵守的契约。来看一下：

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

然后我们将用两个模块实现该 behaviour，即 `Local` 和 `Live` 模块，分别用于本地开发和生产环境。使用 behaviour 可确保我们的模块始终遵守相同的契约，无论是前面两个模块，还是我们在第 9 章《Finding Zen through Testing》中创建测试时用来伪造对提供者实际调用的 mock 模块。

每个实现模块只需使用 `@behaviour` 模块属性来标注其实现的 behaviour。从现在开始，对于每个实现 behaviour 的模块，编译器都会检查它是否以正确数量的参数实现了 behaviour 回调：

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

我们选择 Google Cloud Storage 作为 ElixirDrip 的存储提供者。由于其访问逻辑已通过 `StorageProvider` 自定义 behaviour 完全抽象化，未来可以轻松切换到其他存储提供者。这个假设的新提供者只需要遵守相同的 `StorageProvider` behaviour，因此需要同时实现 `upload/2` 和 `download/1` 回调。

在 Local 或 Live 实现之间做出选择，只需调整全局或生产配置文件即可。这是一种常见模式，当我们想根据当前的 Mix 环境注入不同的模块时就会使用它：

```
$ cat apps/elixir_drip/config/config.exs
use Mix.Config
  # ...
config :elixir_drip,
  storage_provider: ElixirDrip.Storage.Providers.GoogleCloudStorageLocal

import_config "#{Mix.env()}.exs"
```

正如以下代码所示，在 prod.exs 配置中，我们指向 ElixirDrip.Storage.Providers.GoogleCloudStorageLive 模块，该模块将实际与云存储提供者交互。由于此配置文件是在主 config.exs 配置文件之后导入的，它将覆盖之前的 storage_provider 配置项：

```
$ cat apps/elixir_drip/config/prod.exs
use Mix.Config
  # ...
config :elixir_drip,
  storage_provider: ElixirDrip.Storage.Providers.GoogleCloudStorageLive
```

最后一步是创建通用的 ElixirDrip.Storage.Provider 模块，该模块将委托给由 :storage_provider 配置项指定的 StorageProvider 实现。此模块通过将 Provider.upload/2 或 Provider.download/1 调用委托给 Application.get_env(elixir_drip, :storage_provider) 配置返回的模块，从而将实际的存储提供者实现隐藏在调用者之外。

因为我们使用了 defdelegate/2 宏，Elixir 要求 @target 模块定义 upload/2 和 download/1 公共函数，否则委托将无法工作：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/provider.ex
defmodule ElixirDrip.Storage.Provider do
  @moduledoc false

  @target Application.get_env(:elixir_drip, :storage_provider)

  defdelegate upload(path, content), to: @target
  defdelegate download(path), to: @target
end
```

之前，我们决定通过 `ElixirDrip.Storage` 模块公开所有与存储相关的函数，该模块代表我们的 `Storage` 上下文。因此，此上下文将依赖之前的 `ElixirDrip.Storage.Provider` 与我们的存储提供者交互。

![](img/c636f4cfb07f25f1db535995e50706ed_100_0.png)

如果我们不想让这个中间的 `ElixirDrip.Storage.Provider` 模块委托给实际的存储提供者模块，我们可以在消费者 `ElixirDrip.Storage` 模块中直接从配置中获取 `Local` 或 `Live` 模块（即使用 `Application.get_env(:elixir_drip, :storage_provider)`）。

这个上下文在接下来的几章中会做更多的事情，但目前它只公开两个函数，`store/2` 和 `retrieve/1`，两者都依赖于通用的存储提供者：

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

我们现在将通过 `xref` 工具的视角来看待我们的 ElixirDrip 应用。

## 使用 xref 查看交叉引用

自 Elixir 1.3 起，每次编译代码时，你也在使用 `xref` 来查找对不存在的模块和函数的调用。如果我们在 ElixirDrip 应用配置（`config/config.exs`）中指向的模块有拼写错误，我们会得到以下输出：

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

在幕后，编译器运行 mix xref warnings，这会产生这两个警告，告诉我们通用提供者正在将 upload/2 和 download/1 函数委托给一个缺失的模块。

xref 的另一个有用功能是 graph 模式，它通过打印应用依赖图来展示应用文件之间的依赖关系。让我们生成我们的 umbrella 项目的依赖图：

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

xref 检测到我们有一个 umbrella 项目，因此它为每个 umbrella 应用生成了依赖图。
