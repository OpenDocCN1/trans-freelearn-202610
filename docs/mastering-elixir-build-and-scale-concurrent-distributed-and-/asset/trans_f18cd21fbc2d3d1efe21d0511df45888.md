
我们正在启动一个 Calculator 进程并随后使用 start_link/2 和 call/2 函数调用它，这两个函数都属于 GenServer 模块。为了查看调用计算器进程时实际发送的消息，让我们创建一个仅将收到的消息回显到 stdout 的虚拟进程：

```
iex> defmodule Echo do
...> def echoes() do
...> receive do
...> message ->
...> IO.puts "Echo on #{inspect(self())}: #{inspect(message)}"
...> end
...> end
...> echoes()
...> end
...> end
```

```
{:module, Echo,
<<70, 79, 82, 49, 0, 0, 5, 148, 66, 69, 65, 77, 65, 116,
85, 56, 0, 0, 0, 145, 0, 0, 0, 16, 11, 69, 108, 105,
120, 105, 114, 46, 69, 99, 104, 111, 8, 95, 95, 105,
110, 102, 95, 95, 9, 102, ...>>,
{:echoes, 0}}
```

```
iex> echo = Process.spawn(&Echo.echoes/0 , [])
#PID<0.467.0>
```

如果我们现在使用 GenServer 通过 call/2 向 echo 进程发送同步消息，就可以看到发送的消息结构。消息的格式为 {:"$gen_call", from, contents}，其中元组的第二个元素是标识发送者和消息的 {pid, reference} 元组，最后一个元素是我们通过调用传递的内容。我们出现了超时，因为 GenServer.call/2 是同步的，因此调用方期望在遵守 GenServer 协议的同时得到回复，但它没有收到任何回复：

```
iex> GenServer.call(echo, {:hello, :echo})
Echo on #PID<0.467.0>: {:"$gen_call", {#PID<0.448.0>,
#Reference<0.2270275367.1586495491.177847>}, {:hello, :echo}}

** (exit) exited in: GenServer.call(#PID<0.467.0>, {:hello, :echo}, 5000)
** (EXIT) time out
(elixir) lib/gen_server.ex:774: GenServer.call/3
```

我们现在将使用 GenServer.cast/2 发送异步请求，以便观察其消息结构。与之前的同步消息相比，它缺少标识发送者和消息的元组的第二个元素。这完全合理，因为处理请求的服务器不需要对消息进行回复。

我们也没有出现超时，因为我们没有等待服务器的任何响应：

```
iex> GenServer.cast(echo, {:hi_again, :echo})
Echo on #PID<0.467.0>: {:"$gen_cast", {:hi_again, :echo}}
:ok
```

通过使用 GenServer behaviour，当进程异常结束时，我们可以免费获得更好的错误报告。我们还获得了默认的 child_spec 定义，以便轻松地将 GenServer 进程置于监督树下。要查看回调的完整列表，我们可以检查 IEx 建议的可能补全，从 Calculator 模块开始：

```
iex> Calculator.<tab>
child_spec/1 code_change/3 handle_call/3 handle_cast/2
handle_info/2 init/1 terminate/2
```

正如以下错误输出所示，除了实际的 FunctionClauseError 之外，我们还可以了解到最后处理的邮件及其发送者、当时进程的内部状态以及有关客户端的信息，即它是否仍然存活以及调用来自何处：

```
iex> {:ok, calc} = GenServer.start(Calculator, [])
{:ok, #PID<0.526.0>}

iex> GenServer.call(calc, :unexpected)
** (exit) exited in: GenServer.call(#PID<0.526.0>, :unexpected, 5000)
  ** (EXIT) an exception was raised:
  ** (FunctionClauseError) no function clause matching in
Calculator.handle_call/3
  # ...

16:50:40.976 [error] GenServer #PID<0.526.0> terminating
** (FunctionClauseError) no function clause matching in
Calculator.handle_call/3
  iex:14: Calculator.handle_call(:unexpected, {#PID<0.515.0>,
  #Reference<0.2270275367.1586495489.187163>}, [])
  (stdlib) gen_server.erl:636: :gen_server.try_handle_call/4
  (stdlib) gen_server.erl:665: :gen_server.handle_msg/6
  (stdlib) proc_lib.erl:247: :proc_lib.init_p_do_apply/3
Last message (from #PID<0.515.0>) : :unexpected
State: []
Client #PID<0.515.0> is alive
  (stdlib) gen.erl:169: :gen.do_call/4
  (elixir) lib/gen_server.ex:771: GenServer.call/3
  (stdlib) erl_eval.erl:670: :erl_eval.do_apply/6
  (elixir) src/elixir.erl:239: :elixir.eval_forms/4
  (iex) lib/iex/evaluator.ex:231: IEx.Evaluator.handle_eval/5
```

```
(iex) lib/iex/evaluator.ex:212: IEx.Evaluator.do_eval/3
(iex) lib/iex/evaluator.ex:190: IEx.Evaluator.eval/3
(iex) lib/iex/evaluator.ex:89: IEx.Evaluator.loop/1
```

正如本节所见，GenServer behaviour 为我们做了大量繁重的工作。通过使用它，开发人员无需实现客户端和服务器之间的通信细节，而能够专门开发特定的服务器逻辑。让我们在 ElixirDrip 应用中使用这种行为。

## Implementing a CacheWorker with GenServer

我们现在将在 ElixirDrip 上实现一个 CacheWorker 进程，我们将用它来在内存中保留最近的媒体。通过使用这种缓存机制，我们希望减少文件下载所需的时间，并避免为 ElixirDrip 处理的每次下载都运行完整的下载管道。

我们的 CacheWorker 将在一段时间内将一些内容保存在内存中，并与特定的缓存键关联。在我们的例子中，缓存键将是媒体的 id。由于我们不想为同一媒体生成多个缓存进程，我们还将使用媒体 id 作为进程名称。这样，如果有人尝试缓存已存在于缓存中的媒体，就会收到已存在同名进程的通知：

```
iex> CacheWorker.start_link("1", "content")
{:ok, #PID<0.468.0>}

iex> CacheWorker.start_link("1", "other content")
{:error, {:already_started, #PID<0.468.0>}}
```

当我们使用媒体 id 和相应的内容调用 CacheWorker.start_link/2 函数时，将使用 CacheWorker 模块、要缓存的内容以及 :name 选项调用 GenServer.start_link/3。传递给它的模块必须实现 init/1 函数回调，该回调将负责服务器的初始化。

![](img/c636f4cfb07f25f1db535995e50706ed_156_0.png)

我们在注册进程时使用 {:global, {:cache, <media_id>}} 元组，以避免为每个进程使用原子作为名称的需求。通过这种方法，每个缓存进程都将命名为 {:cache, <media_id>}。应避免将原子用作进程名称，因为它们不会被垃圾回收，因此，如果我们使用它们来标识动态生成的进程，就有达到原子最大数量的风险。你可以在本章末尾了解有关注册进程的所有信息。

需要注意的是，`GenServer.start_link/3` 函数调用仍由生成 `GenServer` 的进程完成，而 `init/1` 函数已在生成的服务器上运行。对 `GenServer.start_link/3` 的调用将阻塞直到 `init/1` 回调结束，因此你应检查你的 `init/1` 函数是否花费时间过长。传递给 `GenServer.start_link/3` 函数的第二个参数始终传递给 `init/1` 函数，即使生成的服务器不需要初始化参数。在我们的例子中，我们需要将媒体内容传递给 `init/1` 函数，以便生成的进程将其存储在其内部状态中。

如果 `init/1` 上的初始化过程花费时间过长，只需在初始化期间向当前进程发送异步消息，然后再执行实际的服务器初始化工作，完全不阻塞生成进程。

![](img/c636f4cfb07f25f1db535995e50706ed_157_0.png)

这意味着在返回 `init/1` 调用之前执行 `GenServer.cast(self(), {:initialize, some_args})`，然后在 `handle_cast({:initialize, some_args}, _state)` 函数上执行实际的缓慢初始化过程，返回一个 `{:noreply, new_state}` 元组，其中包含新的进程状态。

我们还将在缓存进程上保存它提供缓存内容的次数。你可以在以下代码片段中看到 `CacheWorker` 的当前状态。它启动并将内容保存在内存中，但目前我们仍无法访问进程的内部状态，包括缓存内容：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/cache_worker.ex
defmodule ElixirDrip.Storage.Workers.CacheWorker do
  use GenServer

  def start_link(media_id, content) do
    GenServer.start_link(__MODULE__, content, name: name_for(media_id))
  end

  defp name_for(media_id), do: {:global, {:cache, media_id}}

  def init(content) do
    {:ok, %{hits: 0, content: content}}
  end
end
```

缓存应允许其客户端检索其存储的内容，而我们目前的缓存不能。
