
```
iex> alias ElixirDrip.Storage.Workers.AgentCacheWorker, as: AgentCache
ElixirDrip.Storage.Workers.AgentCacheWorker

iex> {:ok, agent} = AgentCache.start_link("1", "content")
{:ok, #PID<0.473.0>}

iex> 1..3 |> Enum.map(fn _ -> AgentCache.get_media(agent) end)
["content", "content", "content"]

iex> Agent.get(agent, &(&1))
%{content: "content", hits: 0}
```

让我们通过将 Agent.get/2 函数改为 Agent.get_and_update/2 来修复前面的问题，并将 lambda 的返回值改为包含两个元素的元组，其中第一个是返回给客户端的值，第二个是新的状态值，这次 hits 条目会递增：

```
defmodule ElixirDrip.Storage.Workers.AgentCacheWorker do
  use Agent
  alias ElixirDrip.Storage.Workers.CacheWorker
  # ...
  def get_media(pid),
    do: Agent.get_and_update(pid,
      fn (%{hits: hits, content: content} = state) ->
        {state.content, %{state | hits: hits + 1}}
      end)
end
```

如果我们现在使用 Agent.get_and_update/2 从一个新 agent 获取内容，就能得到正确的 hits 数量：

```
iex> {:ok, agent2} = AgentCache.start_link("2", "other content")
{:ok, #PID<0.539.0>}

iex> 1..3 |> Enum.map(fn _ -> AgentCache.get_media(agent2) end)
["other content", "other content", "other content"]

iex> Agent.get(agent, &(&1))
%{content: "other content", hits: 3}
```

需要注意的一点是，我们作为第二个参数传递的 lambda 在 Agent 进程上运行，因此在 lambda 执行期间，agent 会将其他请求挂起。如果你希望 Agent 状态能被多个客户端更新，你应该始终在服务器进程上执行状态更新（在 lambda 函数作用域内执行）。这样可以避免竞态条件，即在你忙于确定新状态时，agent 状态可能被另一个客户端更新。

乍一看，在所有相关方面，我们的 AgentCacheWorker 看起来与它的 CacheWorker 对应物做的事情一样。我们可以通过 start_link/2 启动它，传入媒体 id 和其内容，并可以使用 AgentCacheWorker.get_media/1 函数获取缓存内容。然而，我们的 agent 进程会永远存活，因为我们没有实现过期机制。我们可能设计的任何过期机制都必须在 agent 之外，因为正如我们将看到的，Agent 无法使用 handle_info/2 回调处理带外消息。如果我们像下面这样为 AgentCacheWorker 添加 handle_info/2 函数，它仍然无法处理通过 Process.send/3 发送的消息：

```elixir
defmodule ElixirDrip.Storage.Workers.AgentCacheWorker do
  use Agent
  alias ElixirDrip.Storage.Workers.CacheWorker
  # ...
  def handle_info(message, state) do
    Logger.debug("#{inspect(self())} Just got an OOB message: #{message}")
    {:noreply, state}
  end
end
```

如果我们向 AgentCacheWorker 进程发送 :oob_message，我们将在 IEx shell 上收到错误消息，提示该消息未被预期，即使 handle_info/2 已在模块上定义：

```
iex> {:ok, agent} = AgentCache.start_link("6", "alchemy for beginners")
{:ok, #PID<0.475.0>}

iex> Process.send(agent, :oob_message, [])
:ok

22:24:07.362 [error] Agent.Server #PID<0.475.0> received unexpected
message in handle_info/2: :oob_message
```

因此，使用 Process.send_after/3 和 handle_info/2 函数回调的优雅过期机制在 Agent 进程中不起作用。一种可行但可能过于复杂的替代方案是在 Agent 初始化期间生成一个链接进程，该进程随后依赖 Process.send_after/3 函数向自身发送一个 :expire 消息。收到此消息后，进程将执行 Process.exit(agent, :expired)。由于 agent 没有预期 :expired 退出信号并且未捕获退出，它将简单地以 :expired 原因退出。

我们将之前的建议留作读者的练习。请记住，如果手头的抽象从一开始就不能提供你所需的灵活性，有时最好一开始就避免使用它。查看我们的 `Agent` 示例，由于我们希望在一段时间后使缓存内容过期，选择 `GenServer` 路线更简单。

![](img/c636f4cfb07f25f1db535995e50706ed_165_0.png)

> 你写的代码越少，引入 bug 的概率就越小。在 Elixir 中，如果你尽可能多地依赖 OTP 抽象，你就能很好地保持代码在未来几年内以最少的中断运行。

我们现在将查看 `Task` 模块以及它如何允许我们并行执行一次性计算，并在需要时等待其结果。

## 任务

正如我们之前看到的，在 Elixir 中生成进程成本很低，因为我们始终处于 BEAM 的领域。这为我们提供了在需要时随时生成进程的完美条件。如果你需要并行做某事，可以生成一个进程。如果你需要并行做很多事，可以生成很多进程。虚拟机不会感到吃力，你很快就会拥有一支进程大军来完成你需要的任何任务。

然而，进程通信很繁琐，因此如果你需要这些并行计算的结果，你必须通过消息传递接收结果，并在之后终止进程。

在这种情况下，我们可以借助 `Task` 模块，使用其 `Task.async/1` 函数生成任务，然后在需要之前生成任务的结果时使用 `Task.await/1`。

假设我们想告诉用户他们相对于当前使用的磁盘空间的排名。获取每个用户的磁盘空间可能是一次昂贵的查询，因此我们希望将其卸载到并行任务中，并在真正需要时才查看这些值：

```
disk_space_by_user_task = Task.async(fn ->
  # expensive query
  [
    %{user_id: 4, disk_space: 600},
    %{user_id: 2, disk_space: 123},
    %{user_id: 1, disk_space: 1024},
    %{user_id: 5, disk_space: 50},
    %{user_id: 3, disk_space: 0.5}
  ]
end)

# performing other work here

{%{disk_space: disk_space}, user_rank} = disk_space_by_user_task
|> Task.await(timeout)
|> Enum.sort(&(&1.disk_space <= &2.disk_space))
|> Enum.with_index(1)
|> Enum.find(&(elem(&1, 0).user_id == current_user))

Notifications.send(current_user, "You're using #{disk_space} MB, making you the \##{user_rank} user in terms of disk usage.")
```

在前面的示例中，我们将昂贵的查询委托给异步 Task，因此当前进程可以继续工作，直到真正需要任务结果。当需要任务结果继续时，进程会尝试使用 Task.await/2 获取它们。如果任务在超时到期前未返回，当前进程将退出，因此在我们的例子中，我们可以确定如果磁盘使用统计数据未到达，就不会发送通知。

注意当前进程在 Task.await/2 调用处如何有效地停止等待。我们如何生成多个任务然后聚合它们的结果？不用多说，让我们深入探讨并行任务。

## 并行任务

为了帮助我们理解任务的工作方式，我们需要一种惰性缓存机制。为此，让我们定义一个 SlowCacheSupervisor 进程，它将 put/2 和 get/1 委托给已引入的良好行为的 CacheSupervisor，但在那之前，它会休眠与接收到的媒体 id 相等秒数的时间：

```elixir
defmodule ElixirDrip.Storage.Supervisors.SlowCacheSupervisor do
  @behaviour ElixirDrip.Behaviours.CacheSupervisor

  alias ElixirDrip.Storage.Supervisors.CacheSupervisor, as: RealCache

  def put(id, content), do: RealCache.put(id, content)

  def get(id) do
    secs_to_nap = case Integer.parse(id) do
      {sleep_time, _} -> sleep_time
      _ -> 1
    end

    Process.sleep(secs_to_nap * 1000)
    RealCache.get(id)
  end
  # ...
end
```

我们现在想要为我们的应用程序创建一个并行文本搜索。此 Search 模块将公开一个 search_for/2 函数，它将接收媒体标识符列表（或单个媒体 id）和一个表达式，并为给定表达式搜索每个媒体内容。我们假设我们需要的所有媒体都已存在于缓存中。

这里最小的工作单元由在单个媒体上执行搜索的函数完成。它从缓存中获取内容，将其拆分为行，然后使用 Stream.with_index/1 将每一行与其行号配对。之后，我们对刚刚创建的流进行归约，如果在当前行找到表达式，则将包含行号和内容的元组添加到结果列表中：

```elixir
defmodule ElixirDrip.Storage.Search do
  alias ElixirDrip.Storage.Supervisors.SlowCacheSupervisor, as: Cache
  # ...
  def search_for(media_id, expression) do
    raw_content_lines = media_id
      |> Cache.get()
      |> elem(1)
      |> String.split("\n")

    result = raw_content_lines
    |> Stream.with_index()
    |> Enum.reduce(
      [],
      fn({content, line}, accum) ->
        case found?(expression, content) do
          nil -> accum
          _ -> accum ++ [{line + 1, content}]
        end
      end)

    {media_id, result}
  end

  defp found?(expression, content) do
    regex = ~r/#{expression}/
    Regex.run(regex, content, return: :index)
  end
end
```
