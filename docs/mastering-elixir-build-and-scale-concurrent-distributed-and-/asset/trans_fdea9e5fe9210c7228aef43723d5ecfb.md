
在这种情况下，列表只有一个元素，即调用 worker 函数的结果。该函数是在我们在模块顶部调用 use Supervisor 时从 Supervisor.Spec 模块导入的。该函数将返回子进程的规范。第一个参数是将在子进程中运行的模块的名称，第二个参数是要提供给该模块的 start_link 函数的参数列表。请注意，你不需要特别指定要调用此函数。这是由于 start_link 名称是默认的。你可以覆盖它并使用不同的名称（通过在选项列表中提供 :function），但我们认为最好遵循社区使用的约定，这将帮助其他软件开发者理解你的代码。最后，worker 函数的最后一个参数是一个选项列表。在这种情况下，我们提供了 restart 值。:transient 表示子进程只有在异常终止时才会被重启。我们也可以使用 :temporary，子进程永远不会被重启；或 :permanent，子进程总会被重启，即使它以 :normal 退出原因终止。

传递给 Supervisor.init/2 的第二个参数是一个选项列表。在这种情况下，我们提供了 :strategy 选项，它定义了 supervisor 如何处理子进程的崩溃。在我们的示例中，我们使用 :one_for_one，这意味着当子进程终止时，supervisor 会在其位置生成一个新的进程。其他可能的值有 :one_for_all，表示当一个子进程崩溃时，该 supervisor 的所有子进程都会被重启；还有 :rest_for_one，它会使在崩溃进程之后启动的子进程按顺序重启（连同崩溃的进程本身，它是第一个）。这意味着当我们使用此策略时，启动 supervisor 子进程的顺序很重要。请注意，直到 Elixir 1.6，我们还拥有 :simple_one_for_one 策略，它用于让 supervisor 动态启动子进程。然而，在 Elixir 1.6 中它已被弃用，转而使用 DynamicSupervisor 模块，这将在下一节中介绍。

这里使用 worker 函数意味着该子进程不是一个 supervisor。我们也可以使用 supervisor 函数，它将返回 supervisor 的子进程规范。在此上下文中，worker 指任何不是 supervisor 的东西。让 supervisor 生成其他 supervisor 将导致一个监督树，我们将在本章最后一节介绍。

supervisor 终止子进程的方式也可以通过 :shutdown 选项进行配置。默认情况下，如果子进程是 worker，supervisor 会向其发送退出信号并等待五秒钟，如果在此间隔内未终止，则会强制杀死该子进程。如果子进程是 supervisor，默认情况下它会无限期等待。如果你有兴趣配置此项，请查看官方 Supervisor 文档 https://hexdocs.pm/elixir/Supervisor.html，其如同大多数 Elixir 文档一样非常详尽，并包含有用的示例。

![](img/c636f4cfb07f25f1db535995e50706ed_138_0.png)

在本节中，我们将只使用基于模块的 supervisor，因为它们在教学上更有利，因为我们对 supervisor 如何初始化有更大的控制权。然而，请注意你不需要为每个想要的 supervisor 创建一个模块。你可以将 Supervisor 模块中函数的调用嵌入到其他模块中，这在你熟悉后可能会简化你的代码。请查看 Supervisor 模块文档以了解如何操作的示例：https://hexdocs.pm/elixir/Supervisor.html。

现在让我们关注 start_link 函数。此函数只是 Supervisor.start_link/2 的包装器，类似于我们在为 CacheWorker 引入接口函数时所做的。另一个值得注意的点是使用 __MODULE__，它是一种特殊形式，在编译期间会被当前模块的名称所替代，这意味着传递给 Supervisor.start_link/2 的第一个参数是当前模块的名称。

现在让我们在 IEx 会话中使用它：

```
iex> c("examples/encryption.ex")
[ElixirDrip.Storage.Pipeline.Encryption]
iex> c("examples/upload_pipeline_supervisor.ex")
[ElixirDrip.Storage.Supervisors.Upload.Pipeline]
iex> {:ok, supervisor_pid} =
ElixirDrip.Storage.Supervisors.Upload.Pipeline.start_link()
19:37:33.693 [debug] #PID<0.97.0>: Starting the Upload Pipeline Supervisor module...
19:37:33.695 [debug] #PID<0.98.0>: Starting the Encryption module...
{:ok, #PID<0.97.0>}
iex> send(pid(0, 98, 0), "some message")
#PID<0.98.0>: Encryption module received a message: some message
"some message"
iex> Process.exit(pid(0, 98, 0), :kill)
true
```

```
19:39:11.208 [debug] #PID<0.102.0>: Starting the Encryption module...
iex> send(pid(0, 102, 0), "some message")
#PID<0.102.0>: Encryption module received a message: some message
"some message"
```

首先，我们编译运行此示例所需的文件。在之前的示例中我们省略了这一点，因为它们只使用单个文件。由于此示例稍微复杂一些，我们明确地编译它们。然后，我们调用 supervisor 的 `start_link/0` 函数，它将启动 supervisor 本身及其唯一的子进程，正如我们从调试消息中观察到的那样。之后，我们向加密模块发送一条消息，以验证它是否正常工作。在这种情况下，它只是将我们发送的内容打印到终端。现在我们确定 worker 进程正在运行，使用 `Process.exit/2` 函数将其杀死。我们也可以通过在代码中某处放置 `raise` 来模拟 worker 中的运行时错误，但这种方法使示例更明确。杀死进程后，我们可以看到一条调试消息，指出 `Encryption` 模块正在重新启动。这就是 supervisor 的作用！它检测到其子进程的崩溃并在其位置启动了一个新进程。最后，我们向新的 `Encryption` 进程发出请求，以证明它完全可用。

需要指出的是，如果一个子进程不断崩溃，supervisor 不会永远重启它——它依赖于最大 *restart frequency*。正如我们在上一节开头所说，拥有 supervisor 的原因在于存在一种只在某些情况下发生的错误类型。重启进程使其从头开始，具有干净的状态。如果进程不断重启，问题可能不是由某些复杂的状态引起的。默认情况下，supervisor 允许在五秒间隔内进行三次重启。如果超过此限制，supervisor 将终止自身以及其所有子进程。在这些条件下让 supervisor 终止自身非常重要，因为它允许错误在监督树中传播，正如我们将在本章最后一节看到的那样。时间段内的错误数量和时间段本身可以在启动 supervisor 时通过分别传递 `:max_restarts` 和 `:max_seconds` 选项进行配置。

## Streamlined child specs

Elixir 1.5 引入了 streamlined child specs，它允许在模块中定义子进程规范。不同于依赖于 `Supervisor.Spec` 模块中的 `worker` 和 `supervisor` 函数，supervisor 现在接收模块列表作为子进程，并将对每个子模块调用 `child_spec/1` 函数。这允许子模块控制它们在 supervisor 下的运行方式。

让我们现在使用 streamlined child specs 查看上一示例中呈现的模块。在 supervisor 中，我们仅将 init/1 函数更改为以下内容：

```
$ cat examples/upload_pipeline_supervisor_with_streamlined_child_specs.ex
defmodule ElixirDrip.Storage.Supervisors.Upload.Pipeline do
  # ...

  def init(:ok) do
    Logger.debug("#{inspect(self())} Starting the Upload Pipeline Supervisor module...")
    Supervisor.init([Encryption], strategy: :one_for_one)
  end
end
```

supervisor 上的代码现在更简单，因为部分配置已移至子进程。在 Encryption 模块中，我们现在有以下函数：

```
def child_spec(_) do
  %{
    id: __MODULE__,
    start: {__MODULE__, :start_link, []},
    restart: :transient,
    shutdown: 10000,
    type: :worker
  }
end
```

此函数有一个参数，我们未使用，但可用于允许 supervisor 在此子进程上配置某些选项。我们现在需要提供一个 id，以前它隐式为模块名称。此外，在其他版本中，shutdown（即 supervisor 等待子进程终止的时间，以毫秒为单位）默认为 5,000 ms，而这里我们看到了如何配置它的示例。与之前版本相比的另一个不同之处在于子进程规范现在只是映射，而不是依赖于 worker 和 supervisor 函数的 API。

你可能不必在每次都定义 child_spec/1 函数。例如，下一章中解释的 Agent 和 Task 模块定义了 child_spec/1 函数，如果你使用其中一个模块，可以跳过此函数的创建。请注意，在这种情况下，你始终可以覆盖它并提供自定义的子进程规范。
