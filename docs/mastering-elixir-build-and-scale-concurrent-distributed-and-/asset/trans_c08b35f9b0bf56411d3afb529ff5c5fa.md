
Elixir 允许你使用 `Stream` 模块惰性地处理集合，我们之前看到过如何通过为集合中的每个元素生成任务来并行处理集合。然而，如果我们传递集合的流程包含多个步骤，使用 `Task.async_stream` 并不是最佳方案，因为对于每个步骤，它会等待所有任务返回或超时，然后再调用下一轮 `Task.async_stream`。

我们想要的是能够生成必要的进程来并行消费集合，但无需在每个步骤结束时进行同步。而且由于我们考虑的是多个步骤，其中某个步骤会更慢是完全可能的。在这种情况下，执行该步骤的进程不可避免地会成为整个 *assembly line* 的瓶颈。而且如果我们处理的数据足够大，我们可以通过填满试图应对数据流入的进程的邮箱来堵塞我们的生产线。

`GenStage` 抽象为我们提供了一种优雅的方式来解决这个问题，通过以不同的方式看待它。不是让每个步骤孤立地工作，简单地执行其任务然后将结果传递给下一步，`GenStage` 引入了一种让后续步骤向之前的步骤发出信号的方式，表明它们已准备好接收更多输入。这样，我们可以确保每个步骤只处理其所能处理的内容，不多也不少。这种策略称为反压，它让我们能够尽可能快地处理输入数据，而不会停止 *assembly line*。数据处理的速率不再由数据生产者控制，而是由处理数据的人控制，因此数据处理是需求驱动的。

每个 `GenStage` 管道中的步骤都将是一个 `Producer`、`Consumer` 或 `ProducerConsumer` 进程。这些进程通过消息传递相互通信；在 `GenStage` 的术语中，这些消息称为 `events`。管道的最后一步将是 `Consumer`，其特征是仅消费从管道到达它的 `events`。由于 `Consumer` 阶段之后没有更多阶段，`Consumer` 进程永远不会从后续阶段接收需求。

在任何 `GenStage` 管道的开始，我们都有一个 `Producer` 进程：这些进程的唯一职责是处理下游进程的需求，通过在管道中引入新事件。任何 `Producer` 都必须遵守接收到的需求，绝不会产生超过当前需求的事件。这正是 `GenStage` 能够在不同阶段转换大量事件，而从不停止整个流程的关键所在。

一个 `GenStage` 管道将拥有一个 `Producer` 和一个 `Consumer` 阶段。如果我们想要一个或多个中间阶段，这些阶段必须同时从上游消费事件并向下游推送事件，就像同时是一个 `Producer` 和一个 `Consumer`。这些中间阶段是 `ProducerConsumer` 阶段，它们知道如何处理来自后续阶段的需求以及如何向上游发送需求：

![](img/c636f4cfb07f25f1db535995e50706ed_196_0.png)

我们谈论的是一个 Elixir behaviour；因此为管道创建阶段将像添加 `use GenStage` 然后实现适当的函数回调一样简单。每个 `ProducerConsumer` 和 `Consumer` 都需要订阅之前的 `GenStage` 进程，以便从上游接收事件。这些订阅选项决定了阶段如何监听来自前一个进程的事件，通常在进程初始化期间传递。

![](img/c636f4cfb07f25f1db535995e50706ed_196_1.png)

你也可以单独启动阶段（例如 `GenStage.start_link/3`），然后再使用 `GenStage.sync_subscribe/2` 创建订阅。然而，我们将在它们的 `init/1` 回调上订阅我们的阶段，以确保阶段始终以有效的订阅开始，即使它之前崩溃并由其 supervisor 重启。

`Consumer` 必须在 `init/1` 上返回一个 `{:consumer, state, subscription_options}` 元组，而 `ProducerConsumer` 将返回一个类似的元组，但在第一个元素上使用 `:producer_consumer`。正如你可能猜到的那样，由于 `Producer` 不从任何人接收事件，因此它不需要订阅选项。它的 `init/1` 回调简单地返回一个 `{:producer, state}` 元组。

在 `Consumer` 或 `ProducerConsumer` 阶段初始化期间传递的 `:subscribe_to` 选项应指向我们订阅的 `GenStage` 进程，而 `:min_demand` 和 `:max_demand` 选项将分别定义通过该管道阶段的事件的最小和最大数量。

你可以在以下代码片段中看到一个假设的 Shipping 阶段的订阅选项示例。该阶段将订阅 Packaging 阶段，并且最多从前一阶段请求 10 个事件。这意味着在任何给定时间，通过该阶段的事件不会超过 10 个：

```
defmodule Shipping do
  use GenStage
  # ...
  @dummy_state []

  def start_link() do
    subscription_options = [
      subscribe_to: [{Packaging, min_demand: 3, max_demand: 10}]
    ]

    GenStage.start_link(__MODULE__, subscription_options, name:
    __MODULE__)
  end

  def init(subscription_options),
    do: {:consumer, @dummy_state, subscription_options}

  def handle_events(tasks, _from, _state) do
    Enum.each(tasks, &perform_step(&1))

    {:noreply, [], @dummy_state}
  end
end
```

让我们分析 Shipping 阶段的需求在 GenStage 管道中是如何管理的：

- `:min_demand` 选项定义了触发新需求的阈值。启动时，Shipping 阶段请求 10 个事件（对应于 `:max_demand` 值），因为它没有任何事件需要处理。
- Packaging 阶段只有五个事件可发送，因此 Shipping 阶段将只接收五个事件进行处理。由于 `:min_demand` 阈值为三，Shipping 阶段将等待再接收两个事件后才向 upstream 请求更多事件。
- 当这两个事件到达时，Shipping 阶段已从最初请求的 10 个事件中接收了七个事件，因此达到了其 `:min_demand` 阈值三。此时，它将请求另外七个事件，从而再次等待达到 `:max_demand` 数量的事件。

![](img/c636f4cfb07f25f1db535995e50706ed_198_0.png)

GenStage behaviour 基于 GenServer 构建，这意味着你也可以使用它的 handle_call/3 和 handle_cast/2 回调。然而，这两个回调允许你通过分别返回 {:reply, reply, [events], new_state} 和 {:noreply, [event], new_state} 来发出事件（注意 [events] 元素）。你可以将 GenStage 进程视为一个定制的 GenServer 进程，它还知道如何处理 handle_events/3（在 Consumer 或 ProducerConsumer 的情况下）或 handle_demand/2（在 Producer 的情况下）。

任何 Consumer 阶段也必须实现 handle_events/3 回调，通过它接收需要处理的事件。最后，它需要返回一个 {:noreply, [], state} 元组，其中第二个元素是要发出的事件，最后一个是 GenStage 进程的新状态。由于我们考虑的是 Consumer 阶段，第二个元素将始终为空列表，因为根据定义，Consumer 阶段不发出事件。

ProducerConsumer 阶段也必须实现相同的 handle_events/3 回调，但与之相反，它需要发出新事件。为了发出事件，该阶段需要返回一个类似的 {:noreply, events, state} 元组，其中 events 元素包含要传递到下游的事件。

我们刚刚观察到如何通过在初始化期间传递它们的订阅选项来启动 Consumer 和 ProducerConsumer 阶段。Producer 阶段启动起来更简单，因为它不订阅任何前一阶段。Producer 阶段的 init/1 函数只需要返回一个 {:producer, state} 元组。然后，它需要实现一个不同的回调 handle_demand/2，以便通过产生新事件来响应新需求，从而在将事件传递到下游时有效地启动管道。

我们的 ElixirDrip 应用将有两个由多个步骤组成的管道。用户的文件上传将启动一系列步骤，负责加密文件、将其存储在云端，最后通知用户上传已成功完成。在相反的方向上，当用户点击下载文件时，下载过程将开始，从云端获取文件、解密它，然后最终通知用户。

让我们开始彻底分析上传流程的工作方式。
