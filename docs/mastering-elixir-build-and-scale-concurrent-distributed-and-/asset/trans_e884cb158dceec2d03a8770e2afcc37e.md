
```
if length(tasks) < how_many do
  Process.send_after(self(), :try_again, @queue_polling)
end

{:noreply, tasks, %{state | pending: how_many - length(tasks)}}
end
end
```

本节我们已经涵盖了 GenStage 的很多内容。不过我们还有一点内容需要完成。

## 最后说明

尽管 GenStage 及其表亲 Flow 都是令人印象深刻的工具，但它们并不是万能的解决方案。使用这些抽象会因初始设置而产生一定的开销。请记住，在底层，进程正在被创建，消息正在被交换并可能被缓冲。因此，请考虑你是否真的需要使用 GenStage，还是只需要一个多步骤的管道来处理事件，而不需要背压机制。如果你追求的是并行处理，我们之前讨论过的 Task.async_stream/1 可能就能胜任。

你可以通过 Process.info/1 结果中的 :message_queue_len 条目来了解某个进程当前邮件箱中有多少条消息。以下片段展示了使用 Process.info/1 的情况，其中进程只是无限等待：

```
iex> pid = spawn(fn -> Process.sleep(:infinity) end)
#PID<0.450.0>

iex> Process.info(pid, [:message_queue_len])
0

iex> Process.send(pid, :hi_there, [])
:ok

iex> Process.send(pid, :hows_the_weather, [])
:ok

iex> Process.info(pid, [:message_queue_len])
2

iex> Process.info(pid)
[
  current_function: {Process, :sleep, 1},
  initial_call: {:erlang, :apply, 2},
  status: :waiting,
  message_queue_len: 2,
  messages: [:hi_there, :hows_the_weather],
  # ...
]
```

在软件工程中，我们应避免过早优化。如果你发现进程变慢且消息队列堆积，这表明某些进程无法应对需求，此时你可以考虑使用 GenStage 来解决该问题。

你还应避免在 GenStage 模块上编写业务逻辑。毕竟，它们的唯一目的是在运行时控制事件的流向。将业务逻辑代码放在单独的模块中，可以更轻松地修改、测试它，并清晰地了解每个步骤在做什么。应用 GenStage 只是编排这些步骤执行方式的一种特定方法。

另一个值得注意的方面是，我们以每个阶段一个工作进程的方式启动了两个 GenStage 管道，因此每个步骤只创建了一个进程。这有助于说明 GenStage 的工作原理，但并不意味着你必须这样做。你可以在每个步骤中创建多个工作进程（即 Consumer 或 ProducerConsumer 阶段），然后将它们全部订阅到一个或多个生产者（即 Producer 或 ProducerConsumer 阶段）。这就是为什么 :subscribe_to 选项接收一个元组列表的原因，每个元组配置一个特定的订阅。通过采用这种方法，你可以控制执行每个步骤的并行进程数量，从而扩展成为瓶颈的特定阶段。

如果你想看一个具有并行步骤的 GenStage 管道示例，可以在我们的 ElixirDrip 伞形项目 examples/parallel_genstage 文件夹中找到一个。它与 ElixirDrip 应用一起编译，因此要查看其运行效果，只需启动一个 IEx shell 并运行 ParallelGenStage.Supervisor.start_link 即可。你也可以查看下一章，我们在其中实现了一种领域特定语言，以便轻松定义新的管道，并可选择为每个阶段定义应创建多少个工作进程。

我们现在将研究 Flow，这是一个基于 GenStage 的 Elixir 库，可让我们轻松并行处理集合。

## Flow

从早期开始，Elixir 就允许我们使用 `Enum` 和 `Stream` 模块轻松处理集合。在前一种情况下，集合会在到达 `Enum` 调用时立即遍历，如果改用相应的 `Stream` 函数，则可以将集合的实际枚举延迟到最后一刻。这就是我们说 `Stream` 函数是惰性的而 `Enum` 函数是急切的原因。

因此，如果我们通过链接 `Enum.map/2` 调用来转换集合，则对每个 `Enum` 函数的调用都会创建一个中间集合。这种行为与链接一堆 `Stream.map/2` 调用所得到的行为形成对比。在后一种情况下，整个集合最多只会被遍历一次，并且仅当生成的流到达 `Enum` 函数（如 `Enum.to_list/1`）时，才会强制评估流的每个元素。

`Enum` 和 `Stream` 模块提供了处理集合的宝贵工具集，但并未利用 Elixir 并行运行多个执行流的能力。在引入 `Flow` 之前，如果有人想并行处理集合，就必须实现实际的处理逻辑，创建负责执行它的进程，最后定义如何聚合这些多个执行流的结果。

`Flow` 的创建是为了简化这种场景。与其以困难的方式实现并行处理，人们只需使用熟悉的一组函数，如 `each/2`、`map/2`、`filter/2` 或 `reduce/3`，但由 `Flow` 模块提供即可。这些函数在语义上等同于 `Enum` 模块上同名函数，但内置了并行因子。

`Flow` 使用相同的事件、`Producer` 和 `Consumer` 阶段的概念，并且可以有多个中间阶段，即 `ProducerConsumer` 阶段，我们的事件通过这些阶段流动，因此不足为奇的是 `Flow` 利用了在 `GenStage` 行为上所做的一切出色工作。`Flow` 甚至能够处理直接来自其他 `GenStage` 进程的事件，将这些进程转变为正在创建的新 `Flow` 的 `Producers` 阶段。

![](img/c636f4cfb07f25f1db535995e50706ed_216_0.png)

你应该知道，默认情况下，Flow 以 500 个元素的批次处理输入集合。如果你与天真的急切解决方案相比时没有看到性能测试的改善，这可能是原因。尝试微调 :min_demand 和 :max_demand 参数，它们有效地控制每次并行处理的元素数量。此外，如果使用 Flow 无法获得更好的性能，这可能意味着对于那种类型的数据和数据量，你最好不用它。

即使目前我们还没有一种高效查询 ElixirDrip 中存储的媒体的方法，让我们尝试应用 Flow 工具集来找出每个用户使用的磁盘空间。

## 示例数据

要执行查询，我们需要数据。让我们创建 ElixirDrip.Search.SampleData 模块来封装所有负责数据创建的方法。我们将从 users/0 函数开始，它返回一个包含九个用户的列表，将每个用户的 id 与其电子邮件关联：

```
$ cat examples/sample_data.exs
defmodule ElixirDrip.Search.SampleData do
  @moduledoc false

  def users do
    [
      %{id: 1, email: "andre_albuquerque@elixir.pt"},
      %{id: 2, email: "daniel_caixinha@elixir.pt"},
      %{id: 3, email: "jose_valim@elixir.br"},
      %{id: 4, email: "joe_armstrong@erlang.uk"},
      %{id: 5, email: "robert_virding@erlang.se"},
      %{id: 6, email: "mike_williams@erlang.wls"},
      %{id: 7, email: "jose_lusquinos@panda.pt"},
      %{id: 8, email: "atenas@meow.cat"},
      %{id: 9, email: "billy_boy@woof.dog"},
    ]
  end
end
```

我们的目标是了解每个用户的磁盘使用情况，但我们也希望用户信息尽可能易于理解，以便可以在某种仪表板上显示。让我们创建一些函数来改进我们对用户的相关信息：

```
$ cat examples/sample_data.exs
defmodule ElixirDrip.Search.SampleData do
# ...
def set_name_domain(%{email: email} = user) do
{name, domain} = email
|> String.split("@")
|> List.to_tuple()

user
|> Map.put(:name, name)
|> Map.put(:domain, domain)
end

def set_full_name(%{name: name} = user) do
full_name = name
|> String.split(" ")
|> Enum.map(&String.capitalize(&1))
|> Enum.join(" ")

Map.put(user, :full_name, full_name)
end

def set_country(%{domain: domain} = user) do
country = domain
|> String.split(".")
|> Enum.reverse()
|> Enum.at(0)
|> String.upcase()

Map.put(user, :country, country)
end

def set_preferences(%{domain: domain} = user) do
preferences = domain
|> String.split(".")
|> Enum.at(0)

Map.put(user, :preferences, preferences)
end
end
```

我们现在将使用 import_file/1 函数，该函数由 IEx.Helpers 模块提供，在我们的 search.exs 脚本中使用之前的示例数据模块。正如我们将看到的，如果我们将用户通过由这些函数中的每一个组成的数据转换过程，我们将获得更美观的信息集：

```
$ cat examples/search.exs
import_file("./examples/sample_data.exs")

alias ElixirDrip.Search.SampleData, as: Data
```
