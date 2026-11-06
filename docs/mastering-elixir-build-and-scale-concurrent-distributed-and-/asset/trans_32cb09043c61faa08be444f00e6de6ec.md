
无论接收到什么消息，每个子句都必须返回新的状态（可能与旧状态相同），因为 receive 的返回值绑定到 new_state 变量。然后该变量被传递给 loop 函数，本质上为循环的下一步设置状态。这是一种常见的有状态服务器方法，根据接收到的消息计算新状态，并使用新状态递归循环。

The get_content function 发送一条消息回调用者，其中包含状态中存在的内容。然后，它刷新当前进程的计时器，该计时器包含在新状态中，同时还包含当前进程已有的更新后的访问次数。

refresh_timer 函数负责取消当前计时器（以免我们的进程过早终止），并创建一个新的计时器，该计时器被添加到新状态中。

terminate 函数非常简单，因为它只是使用 Process.exit/2 函数来终止当前进程。该函数的第二个参数是 :normal，即退出原因。此退出原因可以是任意项，但它具有特殊含义——它表示进程因生命周期结束而退出，而不是因崩溃或运行时错误。我们将在下一节再次讨论退出原因。

最后，我们有 unexpected_message 函数，它只是记录一条警告，说明该进程收到了意外的消息，并保持状态不变。

让我们现在看看这个模块的实际运行情况：

```
iex> cache_worker_pid = CacheWorker.init("some binary content")
#PID<0.90.0>
03:36:07.427 [debug] #PID<0.90.0>: CacheWorker started. Will expire in 59997 milliseconds.
iex> send(cache_worker_pid, {:get_content, self()})
03:36:18.094 [debug] Serving request for get_content. Content is some binary content
03:36:18.099 [debug] #PID<0.90.0>: Canceled the previous expiration timer. Will now expire in 60000 milliseconds.
{:get_content, #PID<0.88.0>}
iex> send(cache_worker_pid, :refresh)
03:36:25.200 [debug] #PID<0.90.0>: Canceled the previous expiration timer. Will now expire in 60000 milliseconds.
:refresh
iex> send(cache_worker_pid, {:get_content, self()})
03:36:26.750 [debug] Serving request for get_content. Content is some binary content
03:36:26.750 [debug] #PID<0.90.0>: Canceled the previous expiration timer. Will now expire in 60000 milliseconds.
{:get_content, #PID<0.88.0>}
iex> flush
{:response, "some binary content"}
{:response, "some binary content"}
:ok
iex> send(cache_worker_pid, :other_message)
03:36:35.178 [warn] #PID<0.90.0>: Received unexpected message:
:other_message
:other_message
iex>
03:37:26.755 [debug] #PID<0.90.0>: Terminating process... Served the cached content 2 times.
```

我们首先调用 init 函数，它返回新创建进程的 PID。我们将其绑定到一个变量，以便我们可以向其发送消息。然后，我们发送对应于获取内容的消息。需要注意的是，请求由服务器异步处理。服务器可能在我们发出下一个命令（刷新计时器）时已经向我们响应了结果，但我们可以继续自己的执行，并在需要时再收集结果。发送刷新消息后，我们可以看到调试消息，说明计时器已更新。然后，我们发出另一个获取内容的请求。现在，我们使用 flush 函数，它会从 shell 进程的邮箱中移除消息并打印它们。预期地，我们在那里有两个消息。最后，我们发送服务器不理解的消息，我们可以看到它记录了该事件。等待几秒钟后，我们还可以看到日志消息，告知我们服务器进程已终止，因为它没有收到更多获取内容或刷新计时器的消息。

我们的服务器进程按预期工作，到目前为止我们已经知道如何使用进程来维护状态。在结束本节之前，还有一个最终的更改需要进行。目前，我们的服务器进程的调用者需要知道我们能处理的 exact 消息格式。这样，我们就泄露了通信协议，如果将来想要更改它，这可能是一个问题。为了规避这个问题，通常服务器进程都有接口函数，它们是调用实现函数（我们之前介绍的那些）的包装器。让我们扩展我们的模块以包含接口函数：

```
$ cat examples/cache_worker_with_interface_functions.ex
defmodule CacheWorker do
  # ...

  def get_content(server_pid) do
    send(server_pid, {:get_content, self()})

    receive do
      {:response, content} -> content
    after
      5000 -> {:error, :timeout}
    end
    end

    def refresh(server_pid) do
      send(server_pid, :refresh)
    end

    # ...
    end
```

请注意，我们没有为 :expire 消息定义接口函数。虽然我们的服务器知道如何解释该消息，但它是服务器内部使用的，在这种情况下我们不想将其暴露给客户端。除了抽象通信协议之外，接口函数还允许我们创建同步调用的假象——因为我们发送请求消息并在等待响应后立即阻塞，看起来就像对服务器进行了同步调用。我们还可以根据每个函数的语义具有不同的行为。例如，对于 refresh 接口函数，我们只是发出 fire-and-forget 请求，不关心响应（请注意，在这种情况下，我们无法确定该消息是否实际被传递）。让我们看看这两个函数在实际中的运行情况：

```
iex> cache_worker_pid = CacheWorker.init("some binary content")
#PID<0.90.0>
04:07:40.574 [debug] #PID<0.90.0>: CacheWorker started. Will expire in 59997 milliseconds.
iex> CacheWorker.get_content(cache_worker_pid)
04:07:49.738 [debug] Serving request for get_content. Content is some binary content
04:07:49.738 [debug] #PID<0.90.0>: Canceled the previous expiration timer. Will now expire in 60000 milliseconds.
"some binary content"
iex> CacheWorker.refresh(cache_worker_pid)
04:07:55.703 [debug] #PID<0.90.0>: Canceled the previous expiration timer. Will now expire in 60000 milliseconds.
:refresh
iex>
04:08:55.704 [debug] #PID<0.90.0>: Terminating process... Served the cached content 1 times.
```

正如我们所见，对于 `get_content` 函数，shell 进程只有在返回我们请求的内容后才恢复执行，而对于 `refresh` 函数，它立即返回。请注意，我们现在在 `CacheWorker` 模块中有函数在不同的进程中运行。这是在 Elixir 和 Erlang 应用程序中常见的模式，因为模块和进程之间没有特定的关系。模块中存在的函数可以被任何进程调用。

我们现在已经介绍了如何构建有状态的服务器进程，它们知道如何处理特定类型的请求并且可能长时间运行。现在让我们看看如何链接和监控进程，这是 Elixir 中容错的核心组件。

## 通过链接和监控进程检测错误

到目前为止，我们主要关注进程的并发方面。然而，进程还用于创建容错和可靠的系统，即使在出现错误的情况下也能继续运行。

要拥有容错应用程序，你必须首先认识到故障的存在，其中大多数是意外的。这些故障范围从我们的依赖项宕机（例如数据库）到硬件故障。此外，如果你运行的是分布式系统，你可能会遇到其他问题，例如远程机器变得不可用，或存在网络分区。无论原因如何，这些故障都必须被检测到，以便我们限制其影响并希望在没有人工干预的情况下从中恢复。

几乎不可能预见到所有可能使应用程序停止运行的场景。更有效的方法是接受任何事情都可能失败，并设计系统以做出反应。当应用程序中的某个组件确实失败时，你不需要对系统的所有组件采取行动——与失败组件完全无关的组件应继续其工作，使应用程序尽可能多地为其客户端提供服务。这些是可靠和自愈系统的特征——它们能够尽快从不可预见的错误中恢复。
