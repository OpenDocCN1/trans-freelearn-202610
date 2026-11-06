
```
[ Step1Module.P44712585, # name [ # subscriptions {StartModule.P5EDD23C9, [max_demand: 6, min_demand: 4]}, {StartModule.P12858ABF, [max_demand: 6, min_demand: 4]} ], ]}, :permanent, 5000, :worker, [Step1Module]}, {"Elixir.Step1Module.P68393018", ...}, {"Elixir.Step1Module.P0F918D60", ...} ] }
```

正如你所见，该函数返回了三个名为 Step1Module.P44712585、Step1Module.P68393018 和 Step1Module.P0F918D60 的 Step1Module 工作者规范，每个规范都订阅了通过 :names_to_subscribe 关键字传递的两个 StartModule 进程。鉴于 Step1Module 阶段未定义任何参数，只有名称和订阅将被传递给 Step1Module.start_link/2 函数。

在继续之前，我们需要创建一个类似的 get_worker_specs/1 函数，该函数改为匹配 :consumer 关键字条目：

```
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliner.ex
defmodule ElixirDrip.Pipeliner do
  import Supervisor.Spec
  # ...
  def get_worker_specs(producer_consumer: producer_consumer,
    args: args, options: options, names_to_subscribe:
    names_to_subscribe) do
    get_worker_specs_with_subscriptions(producer_consumer,
      args: args, options: options, names_to_subscribe:
      names_to_subscribe)
  # ...
  end
```

我们现在可以着手处理下一个阶段：如何收集每个阶段的工作者规范。

## 收集管道工作者规范

目标现在是生成并聚合每个阶段的工作者规范，同时将前一阶段的进程名称传递给它们（第一个生产者阶段除外，该阶段不订阅任何阶段）。

这正是下面的 pipeline_specs/1 函数所做的：它首先减少接收到的所有步骤。当 resulting worker_specs 为空时，表示我们正在查看第一个生产者步骤，因此我们只需将 get_worker_specs(step) 的结果添加到最终结果中。如果它不为空，则表示我们正在处理 ProducerConsumer 或 Consumer 阶段，因此在获取其工作者规范之前需要向步骤添加 names_to_subscribe 关键字：

```
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliner.ex
defmodule ElixirDrip.Pipeliner do
  import Supervisor.Spec
  # ...
  def pipeline_specs(steps) do
    steps
    |> Enum.reduce([], fn step, worker_specs ->
      step = case Enum.empty?(worker_specs) do
        true -> step
        _ ->
          {names_to_subscribe, _} = Enum.at(worker_specs, -1)
          step ++ [names_to_subscribe: names_to_subscribe]
      end
      worker_specs ++ [get_worker_specs(step)]
    end)
    |> Enum.unzip()
    |> elem(1)
    |> List.flatten()
  end
end
```

Enum.reduce/2 的结果是一个列表，其中每个元素都是包含工作者名称及其规范的元组，因此我们对其进行解包，以获得一个元组，其中一个元素包含所有工作者名称的列表，另一个元素包含所有工作者规范的列表，获取包含工作者规范的列表并将其展平。我们最终拥有可由管道监督器解析的工作者规范！请看下面的示例：

```
iex> Qaz.show_pipeline_steps |> ElixirDrip.Pipeliner.pipeline_specs
[
  # two StartModule processes
  {"Elixir.StartModule.PB6AD0F42",
    {StartModule, :start_link, [:start_arg1, :start_arg2,
    StartModule.PB6AD0F42]},
    :permanent, 5000, :worker, [StartModule]},
  {"Elixir.StartModule.PDD3D519D", ...},

  # three Step1Module processes subscribing
  # to the two previous stage processes
  {"Elixir.Step1Module.PAEC81181",
{Step1Module, :start_link,
  [
    Step1Module.PAEC81181,
      [ # two subscriptions for each process
        {StartModule.PB6AD0F42, [max_demand: 6, min_demand: 4]},
        {StartModule.PDD3D519D, [max_demand: 6, min_demand: 4]}
      ]
  ], :permanent, 5000, :worker, [Step1Module]},
{"Elixir.Step1Module.PAEC1AE45", ...},
{"Elixir.Step1Module.PAEC5EBB3", ...},

# five Step2Module processes subscribing
# to the previous three stage processes
{"Elixir.Step2Module.P08E72352",
{Step2Module, :start_link,
  [
    Step2Module.P08E72352,
      [ # three subscriptions for each process
        {Step1Module.PAEC81181, [max_demand: 10, min_demand: 1]},
        {Step1Module.PAEC1AE45, [max_demand: 10, min_demand: 1]},
        {Step1Module.PAEC5EBB3, [max_demand: 10, min_demand: 1]}
      ]
  ], :permanent, 5000, :worker, [Step2Module]},
{"Elixir.Step2Module.P1470075A", ...},
{"Elixir.Step2Module.P9200FBE7", ...},
{"Elixir.Step2Module.PD4EB8A27", ...},
{"Elixir.Step2Module.P7A005C99", ...},

# one FinishModule process subscribing
# to the previous five stage processes
{"Elixir.FinishModule.PA2194E2F",
{FinishModule, :start_link,
  [
    FinishModule.PA2194E2F,
      [
        {Step2Module.P08E72352, [max_demand: 10, min_demand: 1]},
        {Step2Module.P1470075A, [max_demand: 10, min_demand: 1]},
        {Step2Module.P9200FBE7, [max_demand: 10, min_demand: 1]},
        {Step2Module.PD4EB8A27, [max_demand: 10, min_demand: 1]},
        {Step2Module.P7A005C99, [max_demand: 10, min_demand: 1]}
      ]
  ], :permanent, 5000, :worker, [FinishModule]}
]
```

如果我们将前面提到的所有工作者规范传递给 Supervisor.init/2 函数，它将愉快地启动全部 11 个管道进程，前提是 StartModule、Step1Module、Step2Module 和 FinishModule 模块存在并实现了所需的 GenStage 回调函数；也就是说，生产者阶段的 handle_demand/2 以及 ProducerConsumer 或 Consumer 阶段的 handle_events/3。

现在让我们定义管道监督器函数，以便拥有一个完全功能齐全的 DSL！

## 定义监督器函数

鉴于前面的章节中所有繁重的工作都已完成，我们只需要在调用者模块中注入 start_link/0 和 init/1 函数，以便它可以像普通监督器一样启动：

```
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliner.ex
defmodule ElixirDrip.Pipeliner do
  import Supervisor.Spec
  # ...
  defmacro __using__(opts) do
    # ...
    IO.puts "Building Pipeliner: #{name}"
    quote do
      # ...
      Module.register_attribute(__MODULE__, :pipeline_steps,
        accumulate: true)

      @before_compile unquote(__MODULE__)

      def start_link(),
        do: Supervisor.start_link(__MODULE__, [], name: unquote(name))

      def init(_) do
        worker_specs_to_start = get_pipeline_steps()
          |> pipeline_specs()

        Supervisor.init(
          worker_specs_to_start,
          strategy: :rest_for_one,
          name: __MODULE__
        )
      end

      def show_pipeline_steps do
        # ...
      end
    end
  end
end
```

```
end
end
end
```

在 start_link/0 函数中，我们只需使用之前获得的名称启动监督器。在 init/1 函数中，我们获取管道步骤，将其转换为工作者规范，然后使用提到的工作者规范初始化监督器。

请注意，用于 DSL 的大多数函数并未注入到调用者模块中。我们选择改为调用存在于 ElixirDrip.Pipeliner 模块中的函数（调用者模块可以访问这些函数，因为它导入了 Pipeliner 模块）。这是一种良好的实践，旨在避免调用者模块被仅在编译时用于宏展开的函数所 clutter。

![](img/c636f4cfb07f25f1db535995e50706ed_280_0.png)

要查看 ElixirDrip.Pipeliner 模块的完整代码，请查看 ElixirDrip umbrella 项目中的 apps/elixir_drip/lib/elixir_drip/dsl/pipeliner.ex 文件。

我们本可以在这里结束并收工；毕竟，管道 DSL 已完成且按预期工作。但如果有一个机制也能简化管道阶段的定义，不是更好吗？请查看下一节，了解更多元编程技巧。

## 简化 GenStage 模块

目前，我们的管道模块（即 Starter、Encryption、RemoteStorage 和 Notifier）仍自行定义所需的 start_link 和 init/1 函数。让我们先从简化管道生产者模块开始。

## 更简单的管道生产者

对于生产者阶段，我们目前通过 start_link/2 函数传递名称和类型：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/starter.ex
defmodule ElixirDrip.Storage.Pipeline.Starter do
  use GenStage
  require Logger
  alias ElixirDrip.Storage.Workers.QueueWorker

  @queue_polling 5000
```
