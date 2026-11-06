
```
The number squared is 16
#PID<0.109.0>
iex> x
4
```

我们首先创建一个变量 x，将其绑定到 4。然后，我们像前面的示例一样使用 spawn，并创建一个新进程，该进程将接收此变量并对其进行平方。注意，在这个新进程中我们重新绑定了 x，然后打印其新值。由于 x 为 4，因此看到控制台打印出 16 是合理的。然而请注意，之后在 shell 进程中，我们打印 x 的值，它仍然是 4。外部作用域中的变量通过闭包机制传递给在新进程上运行的匿名函数，我们在第 1 章《为旅程做好准备》中见过这一点。由于进程不共享内存，匿名函数引用的变量会被深度复制，新进程中对 x 的重新绑定不会影响 shell 进程中定义的 x。

通常，我们不仅对生成新进程感兴趣，还希望在稍后阶段收集其结果。现在让我们看看消息传递，并了解如何实现这一点。

## 进程之间的消息传递

在初次接触 Elixir 之后，当我们开始构建具有一定复杂度的应用程序时，我们经常需要并发进程相互协作。如前所述，进程不共享内存——它们通过相互传递消息进行通信。这些消息包含 Elixir 项——基本上任何可以存储在变量中的内容都可以作为消息发送。

发送消息的本质是将它存储在接收者的邮箱中。进程的邮箱是无界的——然而，实际上它受可用内存的限制。现在让我们通过向 shell 进程自身发送一条消息来实际操作一下。要发送消息，可以使用 Kernel.send/2 函数，将进程 ID 作为第一个参数，消息本身作为第二个参数：

```
iex> send(self(), "hey, I'm messaging myself")
"hey, I'm messaging myself"
```

在此，我们使用 Kernel.self/0 函数获取自己的 PID，从而将此消息发送到自己的邮箱。请记住消息传递是异步的，send 的调用者在将消息放入接收者的邮箱后立即恢复执行。现在 shell 进程的邮箱中有一条消息。要获取它，我们使用 receive 构造：

```
iex> receive do
...>  message -> IO.puts message
...> end
hey, I'm messaging myself
:ok
```

如你所见，receive 能够从邮箱中获取我们之前发送的消息，并且该消息本身被打印到 shell 中。如果你只想处理特定类型的消息，可以在提供给 receive 的子句上使用模式匹配：

```
iex> send(self(), {:result, 2+2-1})
{:result, 3}
iex> receive do
...>  {:result, result} -> IO.puts "The result is #{result}"
...> end
The result is 3
:ok
```

此示例与之前显示的示例类似，但现在我们只从邮箱中获取第一个元素为 :result 原子的、由两个元素组成的元组消息。需要指出的是，如果一条消息与 receive 块中的任何子句都不匹配，它将保留在邮箱中以便稍后处理。这意味着如果一个进程频繁接收到在 receive 块中永远不匹配的消息，其邮箱将无限增长，这最终可能导致 BEAM 因内存使用过度而崩溃。为避免此问题，可以提供一个始终匹配的子句，以防止邮箱失控增长：

```
iex> send(self(), {:other_result, 3+3-2})
{:other_result, 4}
iex> receive do
...>  {:result, result} -> IO.puts "The result is #{result}"
...>  _ -> {:error, :unexpected_message}
...> end
{:error, :unexpected_message}
```

这样，_ 将匹配未在之前子句中匹配的消息，为该消息提供响应并将其从邮箱中移除。然后，我们返回一个错误元组，该元组还可以包含报告（无论是日志还是错误跟踪系统），以便以后可以分析它，并可能追踪意外消息的来源。

![](img/c636f4cfb07f25f1db535995e50706ed_118_0.png)

在 IEx 会话中进行开发和/或调试时，使用 `IEx.Helpers.flush/0` 函数可能会有所帮助，该函数将 shell 进程邮箱中的所有消息移除并打印到终端。

如我们所见，`receive` 将从进程的邮箱中获取一条消息，并尝试将其与提供的子句匹配。到目前为止，我们只在向 shell 进程发送消息后才调用 `receive`。如果我们用空邮箱调用它，`receive` 将阻塞，等待消息：

```
```elixir
iex> receive do
...>   message -> IO.puts "Message is #{message}"
...> end
```
```

shell 现在被阻塞了，你必须手动终止它（例如按 `Ctrl-\`）。如果邮箱中有未匹配任何提供子句的消息，我们会观察到相同的行为。如果我们不希望 `receive` 无限期地等待消息，可以提供一个 `after` 子句，指定 `receive` 等待的时间（以毫秒为单位）：

```
```elixir
iex> receive do
...> message -> IO.puts "Message is #{message}"
...> after
...> 2000 -> IO.puts "Timed out. No message received in 2 seconds."
...> end
Timed out. No message received in 2 seconds.
:ok
```
```

正如你在第一章中学到的那样，Elixir 中的一切都是表达式。与 `case` 构造发生的情况类似，`receive` 的返回值是已执行子句中最后一个表达式的结果。

我们将以一个更复杂的示例结束本节。现在我们知道如何创建进程，也知道如何在它们之间传递消息，让我们把这一切联系起来，同时展示并发运行进程的一些优势。

首先，让我们定义一个匿名函数，它通过休眠半秒来象征一个长时间运行的计算：

```
```elixir
iex> long_running_computation = fn number ->
...> :timer.sleep(500)
...> "result for #{number}"
...> end
#Function<6.99386804/1 in :erl_eval.expr/5>
```
```

它接收一个数字作为参数，表示将要应用长时间运行计算的数字。为了后续比较，让我们创建一个包含 1 到 50 的整数的列表，并在此列表的每个元素上运行此长时间计算。现在，我们将使用 Enum.map/2 顺序执行：

```
```elixir
iex> computation_results = fn ->
...> (1..50)
...> |> Enum.map(long_running_computation)
...> end
#Function<20.99386804/0 in :erl_eval.expr/5>
```
```

我们将其定义在匿名函数内部，以便可以使用 timer Erlang 模块中的 tc 函数检查执行所需的时间，如下所示：

```
```elixir
iex(3)> :timer.tc(computation_results, [])
{25063563,
["result for 1", "result for 2", "result for 3", "result for 4",
"result for 5", "result for 6", "result for 7", "result for 8", ...]}
```
```

此函数返回一个元组，第一元素为执行时间（以微秒为单位），第二元素为被计时函数的执行结果。注意，出于简洁考虑，函数的执行结果被裁剪，但它包含到第 50 个元素的结果。如我们所见，顺序版本大约花费了 25 秒，这很容易解释，因为对列表中的每个元素都发生了 50 次半秒的休眠。

由于我们现在知道如何生成进程并与之通信，让我们对列表中的每个元素并发地执行此计算，为每个元素生成一个进程，然后在之后聚合结果。

首先，让我们定义一个函数来接收发送到 shell 进程的消息：

```
```
iex> fetch_result = fn ->
... > receive do
... > {:computation_result, result} -> result
... > _ -> {:error, :unexpected_message}
... > end
... > end
#Function<20.99386804/0 in :erl_eval.expr/5>
```
```

此函数将从邮箱中获取一条消息。如果该消息格式符合预期，我们返回结果本身。对于任何不匹配我们期望格式的消息，我们只返回一个错误元组。现在让我们在并发版本中使用此匿名函数：

```
```
iex> me = self()
#PID<0.85.0>
iex> concurrent_computation_results = fn ->
... > (1..50)
... > |> Enum.map(&(spawn(fn -> send(me, {:computation_result,
long_running_computation.(&1)})) end))
... > |> Enum.map(fn _ -> fetch_result.() end)
... > end
#Function<20.99386804/0 in :erl_eval.expr/5>
```
```

让我们分解这个示例，因为这段代码非常密集。我们首先将当前进程的 PID 绑定到 me。这很重要，因为我们稍后会在 spawn 函数内部使用它，以便新进程知道其调用者的 PID 并发送回响应。我们取 50 个整数的列表，并为每个整数创建一个进程。每个进程随后会向其调用者（使用 me 变量）发送一条消息，每条消息的内容都是一个包含两个元素的元组：第一个只是标识消息用途的原子；第二个是执行长时间运行计算的结果，我们将列表中当前整数元素作为参数传递给它。在最后两个片段中，你可以看到进程的创建和它们之间的消息传递的实际操作！

现在让我们计时此版本的执行：

```
```
iex> :timer.tc(concurrent_computation_results, [])
{513566,
["result for 1", "result for 2", "result for 3", "result for 9",
"result for 4", "result for 10", "result for 5", "result for 6", ...]}
```
```
