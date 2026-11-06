
与上传管道监督者相比，下载管道监督者仅在不同阶段之间的订阅顺序上有所不同（现在 RemoteStorage 阶段是第二个而不是第三个），以及 @direction 模块属性的值，现在为 :download。

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/supervisors/download_pipeline_supervisor.ex
defmodule ElixirDrip.Storage.Supervisors.Download.Pipeline do
  use Supervisor
  # ...
  @direction :download
  # ...
  def init(_) do
    remote_storage_subscription = [subscribe_to: [{@starter_name, min_demand: 1, max_demand: 10}]]
    encryption_subscription = [subscribe_to: [{@storage_name, min_demand: 1, max_demand: 10}]]
    notifier_subscription = [subscribe_to: [{@encryption_name, min_demand: 1, max_demand: 10}]]

    Supervisor.init([
      # ...
    ],
    strategy: :rest_for_one,
    name: __MODULE__
    )
  end
end
```

再一次，让我们从下载管道的末端开始，通过分析其消费者 Notifier 阶段。

## Notifier 阶段

下载管道的 Notifier 阶段订阅 Encryption 阶段。该阶段使用与上传管道相同的 Notifier 模块。区别在于赋予下载 Notifier 进程的进程别名（ElixirDrip.Storage.Pipeline.Notifier.Download），以及 notify_step/1 函数的新实现，该函数现在对事件类型键的 :download 值进行模式匹配。其余部分保持不变：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/notifier.ex
defmodule ElixirDrip.Storage.Pipeline.Notifier do
  # ...
  def start_link(name, subscription_options),
    do: GenStage.start_link(__MODULE__, subscription_options, name: name)

  def init(subscription_options),
    do: {:consumer, @dummy_state, subscription_options}

  def handle_events(tasks, _from, _state) do
    Enum.each(tasks, &notify_step(&1))

    {:noreply, [], @dummy_state}
  end

  defp notify_step(%{media: %{id: id}, content: content, type:
  :download}) do
    # Invoke the notifier instead!

    Logger.debug("#{inspect(self())}: NOTIFICATION! Downloaded media
    #{id}, content: #{inspect(content)}, size: #{byte_size(content)}
    bytes.")
  end
end
```

如你所见，处理下载事件的 notify_step/1 函数目前与处理上传事件的函数相同（唯一变化的是日志消息）。我们只是使用 Logger.debug/1 函数向控制台记录一条消息，但一旦我们使用 Phoenix Channels 实现用户通知，这一点就会改变。

现在让我们检查 Encryption 阶段如何处理下载事件。

## Encryption 阶段

在下载管道中，Encryption 阶段订阅来自 RemoteStorage 阶段的事件。与 Notifier 阶段类似，处理该阶段下载事件的函数与处理上传事件的函数并存。

然而，这次 Encryption 模块有两个 encryption_step/1 函数来处理下载事件：一个对状态事件字段的 :original 值进行模式匹配，另一个处理所有其他下载事件，对应于媒体内容仍被加密的情况：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/encryption.ex
defmodule ElixirDrip.Storage.Pipeline.Encryption do
  # ...
  def start_link(name, subscription_options),
    do: GenStage.start_link(__MODULE__, subscription_options, name: name)

  def init(subscription_options),
      do: {:producer_consumer, @dummy_state, subscription_options}

  # the GenStage.handle_events/3 callback
  # delegates the processing of each event to another encryption_step
  /1 function
  # that pattern-matches on %{type: :download}
  def handle_events(tasks, _from, _state) do
      encrypted = Enum.map(tasks, &encryption_step(&1))

      {:noreply, encrypted, @dummy_state}
  end

  # the %{status: :original} indicates the content is ready to be delivered
  defp encryption_step(%{media: media, content: _content, status:
  :original, type: :download} = task),
      do: task

  # given there's no %{status: :original}, we need to decrypt the content
  defp encryption_step(%{media: %{id: id, encryption_key:
  encryption_key},
  content: content, type: :download} = task) do
      Process.sleep(1000)

      clear_content = Provider.decrypt(content, encryption_key)
      Cache.put(id, clear_content)

      %{task | content: clear_content}
  end
end
```

在第一种情况下，下载事件到达 Encryption 阶段时已带有原始媒体内容（因为文件在之前的阶段已从缓存中获取）。因此，encryption_step/1 函数仅返回收到的相同事件。如果下载事件在 status 字段上没有 :original 值，则意味着事件中存在的媒体内容需要在向下游发出新事件之前使用媒体 encryption_key 进行解密。媒体内容在解密后会被缓存以加快后续下载速度。

## RemoteStorage 阶段

在 RemoteStorage 阶段的 remote_storage_step/1 函数中，我们尝试查找下载事件中包含的媒体是否存在 CacheWorker。该阶段从 Starter 事件生产者接收下载事件，并向下游发出带有媒体内容的事件。如果媒体存在 CacheWorker，则该阶段发出的事件内容对应于媒体原始内容，因此事件会使用 status 事件字段标记为 :original；如果不存在，则该阶段只是从云提供商获取媒体加密内容并传递下去：

```elixir
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/
remote_storage.ex
defmodule ElixirDrip.Storage.Pipeline.RemoteStorage do
  # ...
  def start_link(name, subscription_options),
    do: GenStage.start_link(__MODULE__, subscription_options, name: name)

  def init(subscription_options),
    do: {:producer_consumer, @dummy_state, subscription_options}

  # the GenStage.handle_events/3 callback
  # delegates the processing of each event to another
  # remote_storage_step/1 function
  # that pattern-matches on %{type: :download}
  def handle_events(tasks, _from, _state) do
    processed = Enum.map(tasks, &remote_storage_step(&1))

    {:noreply, processed, @dummy_state}
  end

  defp remote_storage_step(%{media: %{id: id, storage_key:
  storage_key}, type: :download} = task) do
    Process.sleep(2000)

    result = case Cache.get(id) do
      nil ->
        {:ok, content} = Provider.download(storage_key)
        %{content: content}

      {:ok, content} ->
        %{content: content, status: :original}
    end

    Map.merge(task, result)
  end
end
```

## Starter 阶段

当我们之前分析上传管道的 Starter 阶段的工作方式时，我们看到如何将 type 作为初始化参数传递，以便上传 Starter 阶段知道应从哪个 QueueWorker 进程获取新的上传请求。对于下载 Starter 阶段，采用的方法完全相同：我们在下载管道监督者上使用 @starter_name 和 @direction 参数初始化 Starter 进程，在本例中它们的值分别为 ElixirDrip.Storage.Pipeline.Starter.Download 和 :download。这足以让 Starter 阶段开始在下载管道上生成事件：

```
iex> alias ElixirDrip.Storage.Pipeline.Common
ElixirDrip.Storage.Pipeline.Common

iex> alias ElixirDrip.Storage.Pipeline.Starter
ElixirDrip.Storage.Pipeline.Starter

iex> starter_name = Common.stage_name(Starter, :download)
ElixirDrip.Storage.Pipeline.Starter.Download

iex> starter_process = Process.whereis(starter_name)
#PID<0.426.0>

iex> Process.alive?(starter_process)
true
```

由于从 QueueWorker 进程获取新的下载请求并将其注入下载管道的逻辑与我们在上传管道中已使用的逻辑完全相同，因此 Starter.handle_demand/2 和 Starter.handle_info/2 回调使用的 send_events_from_queue/3 函数的实现也可以完全相同。该函数可同时服务于下载和上传管道，因为在从队列检索传输请求（无论是上传还是下载）之后，对 send_events_from_queue/3 的调用也会在事件映射上放置 type 条目：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/starter.ex
defmodule ElixirDrip.Storage.Pipeline.Starter do
  # ...
  defp send_events_from_queue(queue, how_many, %{type: type} = state) do
    tasks = queue
      |> QueueWorker.dequeue(how_many)
      |> Enum.map(&Map.put(&1, :type, type))
```
