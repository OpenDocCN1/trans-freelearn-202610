
这目前是 Elixir 社区定义子规范的首选方式。我们想向你展示定义子规范的两种方式，因为你在浏览源代码时很可能会同时遇到这两种方式。然而，在从头开始一个项目（或重构某些代码）时，请选择我们现在介绍的选项，因为你的代码将遵循社区约定，使其他人更容易理解其内容。

## 使用动态监管器在运行时启动子进程

我们现在已经了解如何使用监管器来从错误中恢复，它会根据我们提供的配置重启崩溃的进程。然而，到目前为止，我们必须在启动前预先定义监管器的子进程，这些子进程会在监管器启动后立即启动。有时这并不能满足我们的需求，我们需要在运行时生成监管器的新子进程。这就是动态监管器变得有用的地方。

Elixir 1.6 版本引入了 DynamicSupervisor 模块。如我们所述，这个新模块弃用了常规监管器的 :simple_one_for_one 策略。之所以进行这种提取有很多原因，其中两个主要原因是 :simple_one_for_one 策略改变了监管器初始化和关闭的语义，而且 Elixir 标准库中的某些 API 会根据监管策略期望不同的输入（甚至可能不可用）。

为了演示这一概念，我们将创建一个 CacheSupervisor 模块，它将负责监管本章中一直使用的 CacheWorker 进程。然而，CacheWorker 进程需要进行一些更改，因为我们需要注册进程（在别名下），以便我们的监管正常工作。请注意，在之前的示例中，我们能够观察到监管器对 Encryption 进程崩溃的反应，通过启动一个新的进程，但在实际应用中我们需要注册被监管的进程，否则它们的客户端仍可能联系到已经终止的进程。

当我们引入监管器时，保留先前创建进程的 PID 的做法就变得无关紧要了，因为随着进程崩溃和监管器创建新进程，PID 会发生变化。我们尚未介绍 GenServer 行为，将在下一章开头进行介绍。你仍然可以毫无困难地跟随此示例，因为主要关注点是监管器——事实上，我们在这里不会展示新的 CacheWorker 的代码，因为它与示例无关。如往常一样，如果你好奇，可以在 本章的源代码中找到它。

现在让我们看看缓存监管器的代码：

```
$ cat examples/cache_supervisor.ex
defmodule ElixirDrip.Storage.Supervisors.CacheSupervisor do
  use DynamicSupervisor
  alias ElixirDrip.Storage.Workers.CacheWorker

  def start_link() do
    DynamicSupervisor.start_link(__MODULE__, [], name: __MODULE__)
  end

  def init(_) do
    DynamicSupervisor.init(strategy: :one_for_one, max_children: 100)
  end

  def put(id, content) when is_binary(id) and is_bitstring(content) do
    case find_cache(id) do
      nil -> start_worker(id, content)
      pid -> {:ok, pid}
    end
  end

  def get(id) when is_binary(id) do
    case find_cache(id) do
      nil -> {:error, :not_found}
      pid -> CacheWorker.get_media(pid)
    end
  end

  def refresh(id) when is_binary(id) do
    case find_cache(id) do
      nil -> {:error, :not_found}
      pid -> CacheWorker.refresh(pid)
    end
  end

  defp start_worker(id, content) when is_binary(id) and is_bitstring(content),
    do: DynamicSupervisor.start_child(__MODULE__, cache_worker_spec(id, content))

  defp find_cache(id) when is_binary(id) do
    GenServer.whereis(CacheWorker.name_for(id))
  end

  defp cache_worker_spec(id, content) do
    Supervisor.child_spec(
      start: {CacheWorker, :start_link, [id, content]},
      restart: :transient
    )
  end
end
```

让我们探索一下这里与之前示例相比的差异。除了模块名称不同之外，现在 init/1 不再声明任何子进程。它只是通过提供一些配置来启动监管器。在定义策略之后，我们可以看到 :max_children 选项，这是 DynamicSupervisor 模块特有的。它配置了该进程可以生成的子进程的最大数量。你可以使用它来限制并发，这也会限制资源使用。在我们的示例中，如果让缓存进程无限增长，我们可能会耗尽所有可用内存，导致 BEAM 崩溃。

该监管器管理我们已经描述过的 CacheWorker 进程。但是请注意，其接口略有变化，因为我们将每个内容与一个 id 关联。当尝试从缓存中放入或获取内容时，它会使用 find_cache 函数，而该函数又使用 GenServer.whereis/1 函数。

通过这样做，我们检查为提供的 id 是否已存在进程。如果存在，我们只需返回包含该进程 PID 的元组（使用 put 时）或使用 CacheWorker.get_media/1 函数检索对应给定 id 的内容（使用 get 时）。如果不存在，我们将创建一个新进程来保存该 id 的内容（使用 put 时）或返回一个元组表示未找到进程（使用 get 时）。当我们调用 put 且该 id 的进程尚不存在时，监管器会介入并动态创建新的子进程。这发生在 start_worker 函数中，该函数又使用 DynamicSupervisor.start_child/2。

最后，请注意我们如何传递这个动态子进程的子规范。我们本可以直接使用前面提到的简化的子规范，在 CacheWorker 模块上有一个 child_spec/1 函数。那也是完全正确的。我们只是这样做是为了让你看到如何从监管器配置子规范，从而允许你覆盖任何感兴趣的特定字段。让我们在 IEx shell 中看看它是如何工作的：

```
iex> c("examples/cache_worker_under_supervisor.ex")
[ElixirDrip.Storage.Workers.CacheWorker]
iex> c("examples/cache_supervisor.ex")
[ElixirDrip.Storage.Supervisors.CacheSupervisor]
iex> ElixirDrip.Storage.Supervisors.CacheSupervisor.start_link()
{:ok, #PID<0.97.0>}
iex> ElixirDrip.Storage.Supervisors.CacheSupervisor.put("abc", "def")
04:13:04.744 [debug] #PID<0.99.0>: CacheWorker started. Will expire in
60000 milliseconds.
{:ok, #PID<0.99.0>}
iex> ElixirDrip.Storage.Supervisors.CacheSupervisor.get("abc")
04:13:17.550 [debug] #PID<0.99.0>: Received :get_media and served 3
bytes 1 times.
04:13:17.550 [debug] #PID<0.99.0>: Canceled the previous expiration timer.
Will now expire in 60000 milliseconds.
{:ok, "def"}
iex> ElixirDrip.Storage.Supervisors.CacheSupervisor.get("xyz")
{:error, :not_found}
04:14:17.565 [debug] #PID<0.99.0>: Terminating process... Served the cached
content 1 times.
iex> ElixirDrip.Storage.Supervisors.CacheSupervisor.get("abc")
{:error, :not_found}
iex> ElixirDrip.Storage.Supervisors.CacheSupervisor.put("abc", "def")
04:14:25.386 [debug] #PID<0.104.0>: CacheWorker started. Will expire in
60000 milliseconds.
{:ok, #PID<0.104.0>}
iex> Process.exit(pid(0, 104, 0), :kill)
true
04:14:42.485 [debug] #PID<0.106.0>: CacheWorker started. Will expire in
60000 milliseconds.
iex> ElixirDrip.Storage.Supervisors.CacheSupervisor.get("abc")
04:14:46.674 [debug] #PID<0.106.0>: Received :get_media and served 3
bytes 1 times.
{:ok, "def"}
04:14:46.674 [debug] #PID<0.106.0>: Canceled the previous expiration timer.
Will now expire in 60000 milliseconds.
04:15:46.675 [debug] #PID<0.106.0>: Terminating process... Served the
cached
content 1 times.
```

编译所需文件后，我们演示了此 `CacheSupervisor` 的常规用法。然后，我们让计时器过期，可以看到监管器没有重启进程。这是因为重启策略是 `:transient`，并且进程正常退出。然后我们通过再次将内容放入缓存来启动新进程。之后，我们杀死这个新创建的进程，立即看到一条日志消息指出 `CacheWorker` 已启动。这次，由于退出原因是 `:kill`（而不是 `:normal`），监管器介入，在崩溃进程的位置启动了新进程。正如示例所示，即使我们的服务器进程崩溃，我们仍然可以访问我们的 `id` 的内容。

我们现在已经了解了如何使用动态监管器，并见证了它们能为我们的应用程序带来的灵活性。让我们进入本章的最后一部分：监管树。
