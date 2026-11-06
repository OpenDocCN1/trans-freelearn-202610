
```
def start_link(name, type) do
  GenStage.start_link(__MODULE__, type, name: name)
end

def init(type) do
  Logger.debug("#{inspect(self())}: #{type} Pipeline Starter started.")

  {
    :producer,
    %{queue: QueueWorker.queue_name(type), type: type, pending: 0}
  }
end
# ...
end
```

我们可以在这里看到一个模式：name 参数仅由 start_link/2 函数使用，用于在启动时设置生产者进程名称，而 type 参数则简单地通过 GenStage.start_link/3 调用传递给 init/1 函数，并用于初始化进程状态。从某种意义上说，我们可以将 name 参数视为必需的，因为进程启动依赖于它，而 type 参数是可选的，因为它仅对进程的初始状态有贡献。最终，init/1 函数返回一个包含 :producer 和初始进程状态的二元元组。

让我们创建一个 Pipeliner.Producer 宏模块，帮助定义前面两个函数。我们将再次应用 use/__using__/1 构造来指示可选参数是什么。前面的代码片段将被简化为以下内容：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/starter.ex
defmodule ElixirDrip.Storage.Pipeline.Starter do
  require Logger
  alias ElixirDrip.Storage.Workers.QueueWorker

  use ElixirDrip.Pipeliner.Producer, args: [:type]

  @impl ElixirDrip.Pipeliner.Producer
  def prepare_state([type]) do
    Logger.debug("#{inspect(self())}: #{type} Pipeline Starter started.")

    %{queue: QueueWorker.queue_name(type), type: type, pending: 0}
  end
  # ...
end
```

查看最终结果，我们可以理解该模块是一个生产者，它有一个可选参数，需要执行一些额外的步骤来准备初始进程状态。在幕后，`Pipeliner.Producer` 模块为我们设置了一个 `start_link/2` 函数，该函数为进程设置随机名称，以及一个调用 `prepare_state/1` 函数的 `init/1` 函数。

我们依赖 `Pipeliner.get_or_default/3` 函数来获取通过 `__using__/1` 选项传递的 `args`，然后使用 `create_args/2` 函数创建可选和必需的参数。该函数依赖于 `Macro.var/2` 函数来生成每个变量的引用表示。

![](img/c636f4cfb07f25f1db535995e50706ed_282_0.png)

请记住，每个引用表示都是一个（可能嵌套的）三元素元组。因此，函数变量也以相同的方式表示。执行 `Macro.var(:name, __MODULE__)` 将产生 `{:name, [], __MODULE__}`，即变量的引用表示，准备注入到另一个 AST 中。

由于生成的 `start_link` 函数必须接收可选和必需的参数，我们将它们合并到一个新的 `function_args` 变量中。该函数接收的参数数量与我们拥有的可选参数数量成比例变化。在这种情况下，`unquote_splicing/1` 非常有用，因为它原地取消引用它接收到的列表中的每个元素，使其成为定义具有可变参数的函数时的完美辅助。

在此示例中，`function_args` 变量将包含 `[{:type, [], __MODULE__}, {:name, [], __MODULE__}]`；也就是说，`type` 和 `name` 变量的引用表示。

当我们使用 `unquote_splicing(function_args)` 参数定义 `start_link` 函数时，函数参数将有效地变为 `unquote(type)`、`unquote(name)`。在 `start_link` 函数体中，我们使用 `name` 参数作为新的进程名称，并通过 `GenStage.start_link/3` 函数的第二个参数将 `optional_args` 列表传递给 `init/1` 函数：

```elixir
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliners_producer.ex
defmodule ElixirDrip.Pipeliner.Producer do
  import ElixirDrip.Pipeliner
  # ...
  defmacro __using__(opts) do
    args = get_or_default(opts, :args, [])

    optional_args = create_args(__MODULE__, args)
    required_args = create_args(__MODULE__, [:name])
```

```
function_args = optional_args ++ required_args

quote do
  use GenStage
  # ...
  def start_link(unquote_splicing(function_args)) do
    GenStage.start_link(__MODULE__, unquote(optional_args), name: name)
  end
  # ...
end
end
```

```
defp create_args(_, []), do: []
defp create_args(module, arg_names),
  do: Enum.map(arg_names, &Macro.var(&1, module))
end
```

让我们现在分析 init/1 函数是如何生成的。如我们之前所见，optional_args 列表被传递给 init/1 函数（请注意用于匹配 init/1 函数单个参数的包围 [...]）。因此，该函数接收列表然后调用 prepare_state/1 函数的默认实现，该实现也由 Pipeliner.Producer 模块注入到调用者模块中。最终，它返回 GenStage 行为所需的预期 {:producer, state} 元组：

```
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliner_producer.ex
defmodule ElixirDrip.Pipeliner.Producer do
  import ElixirDrip.Pipeliner

  @callback prepare_state(list(any)) :: any

  defmacro __using__(opts) do
    # ... optional_args, required_args and function_args definition

    quote do
      use GenStage
      @behaviour unquote(__MODULE__)
      @behaviour GenStage
      # ...

      @impl GenStage
      def init([unquote_splicing(optional_args)]) do
        args = prepare_state(unquote(optional_args))

        {:producer, args}
      end
```

```
```elixir
@impl unquote(__MODULE__)
def prepare_state(args), do: args

deoverridable unquote(__MODULE__)
end
end
# ...
end
```
```

prepare_state/1 函数的默认实现由一个恒等函数组成，因为它返回接收到的确切参数。为了允许调用者模块实现自己的 prepare_state/1 函数版本并覆盖默认版本，我们将 prepare_state/1 函数定义为 @callback，然后我们使用 deoverridable Pipeliner.Producer 表示可以覆盖被注入模块的函数。通过使用模块调用 deoverridable/1，我们表示指示模块的每个已实现回调函数都是可覆盖的。在我们的例子中，鉴于唯一的回调是 prepare_state/1 函数，我们本可以使用 deoverridable [prepare_state: 2] 来指定特定的回调。

我们使用 @impl Foo 注解来指示编译器这些函数正在实现来自 Foo 模块的回调。这样做的好处之一是，一旦编译器看到一个 @impl 注解，它将检查所有覆盖函数的函数名和元数是否与相应行为回调的签名匹配。只需使用一次 @impl 机制，编译器就足以检查模块实现的所有行为。因此，如果我们在注入到调用者模块的 init/1 函数中省略 @impl GenStage，将会引发以下警告：

```
```
iex> recompile
==> elixir_drip
Compiling 12 files (.ex)
```
```

```
```
warning: module attribute @impl was not set for function init/1 callback
(specified in GenStage). This either means you forgot to add the "@impl
true" annotation before the definition or that you are accidentally
overriding this callback
lib/elixir_drip/storage/pipeline/starter.ex:24
```
```

让我们现在应用相同的方法来简化我们的管道消费者。

## 更简单的管道（生产者）消费者

我们现在采用的方法与之前针对生产者阶段的方法非常相似。
目前，RemoteStorage 阶段看起来是这样的：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/
remote_storage.ex
defmodule ElixirDrip.Storage.Pipeline.RemoteStorage do
  use GenStage
  require Logger
  alias ElixirDrip.Storage
  alias Storage.Provider
  alias Storage.Supervisors.CacheSupervisor, as: Cache

  @dummy_state []

  def start_link(name, subscription_options),
    do: GenStage.start_link(__MODULE__, subscription_options, name: name)

  def init(subscription_options) do
    Logger.debug("#{inspect(self())}: Pipeline RemoteStorage started. Options: #{inspect(subscription_options)}")

    {:producer_consumer, @dummy_state, subscription_options}
  end
  # ...
  end
```

让我们看看应用于同一 RemoteStorage 阶段的最终结果，该阶段是一个生产者消费者。这里的主要区别在于通过 use 选项传递的额外 type 选项（我们将创建的模块将同时被生产者消费者和消费者使用）以及新的订阅选项，两者都由 init/1 回调需要：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/
remote_storage.ex
defmodule ElixirDrip.Storage.Pipeline.RemoteStorage do
  require Logger
  alias ElixirDrip.Storage
  alias Storage.Provider
  alias Storage.Supervisors.CacheSupervisor, as: Cache

  use ElixirDrip.Pipeliners.Consumer, type: :producer_consumer

  @impl ElixirDrip.Pipeliners.Consumer
  def prepare_state(_dummy_state) do
    Logger.debug("#{inspect(self())}: Streamlined Pipeline RemoteStorage started.")
```
