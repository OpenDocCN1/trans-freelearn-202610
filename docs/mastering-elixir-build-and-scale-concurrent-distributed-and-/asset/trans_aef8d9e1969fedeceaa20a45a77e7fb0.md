
```
# ...
end
```

请注意，修改后，我们需要运行 mix deps.get 来获取这个新的依赖。最后一步设置 Mox 是定义我们在测试中使用的模拟。与其它测试框架不同，使用 Mox 你需要在一个地方完成，而不是每个测试都做。其原因是你不应该在测试中生成动态模块作为模拟，而应在一个中心位置定义它们；例如，在 test_helper.exs 中，或在 setup_all 块中，如果出于某种原因需要在测试模块中执行此操作。在这个示例中，我们将在 test_helper.exs 中执行此操作：

```
$ cat apps/elixir_drip/test/test_helper.exs
ExUnit.start()

Mox.defmock(ElixirDrip.Storage.Providers.GoogleCloudStorageMock, for:
ElixirDrip.Behaviours.StorageProvider)
```

我们使用 Mox.defmock/2 来定义一个模拟，其名称作为第一个参数给出，该名称基于第二个参数中 :for 选项传递的行为。Mox 不允许在不链接到行为的情况下创建模拟。虽然乍一看这可能感觉像是多余的工作，但请记住，这将确保模拟的 API 和真实实现不会产生偏差。秉承 Elixir 设计和哲学中存在的显式性，Mox 希望你定义明确的契约，实际实现和模拟都必须遵守。这样，你就不会出现测试全部通过但应用程序无法正常工作的情况，这通常是因为测试滥用模拟，并且与实际应用程序生活在不同的现实中。

![](img/c636f4cfb07f25f1db535995e50706ed_425_0.png)

我们已经在第 2 章《Elixir 项目的内部结构》中创建了 ElixirDrip.Behaviours.StorageProvider 行为。

如果我们还没有这样做，我们需要创建行为，让真实实现采用该行为，然后再在 test_helper.exs 中进行此更改。基础工作已完成。让我们现在检查测试本身：

```
$ cat apps/elixir_drip/test/elixir_drip/storage/pipeline/
remote_storage_test.exs
defmodule ElixirDrip.Storage.Pipeline.RemoteStorageTest do
  use ExUnit.Case, async: true
  import Mox

  alias ElixirDrip.Storage.Providers.GoogleCloudStorageMock
```

```
@subject ElixirDrip.Storage.Pipeline.RemoteStorage

setup :verify_on_exit!

test "on upload it calls the GoogleCloudStorage client with the right arguments" do
  expect(GoogleCloudStorageMock, :upload, fn "test_key", "test_content" -> {:ok, :uploaded} end)

  task = %{type: :upload, media: %{id: "test_id", storage_key: "test_key"}, content: "test_content"}
  @subject.handle_events([task], nil, nil)
end
end
```

此测试的目的是检查我们的依赖是否使用正确的参数被调用。然后由该其他模块的单元测试负责检查其在给定正确参数时是否表现正确。首先，我们导入 Mox，然后调用 `setup :verify_on_exit!`。此设置调用将使 Mox 在进程退出之前立即验证当前进程中定义的所有期望是否已满足。然后，在测试本身中，我们使用 `expect` 函数（从 Mox 导入）为我们之前创建的模拟设置期望。第一个参数是我们设置期望的模块，第二个参数是函数名称，最后一个参数是一个 lambda，当模拟接收到此函数调用时将被调用。

![](img/c636f4cfb07f25f1db535995e50706ed_426_0.png)

如果你想设置期望，使模拟接收该函数调用多次，可以向 `expect` 函数提供另一个参数，定义此函数在模拟上将被调用多少次。例如，扩展我们上一个示例，如果我们期望模拟中的 `:upload` 函数被调用三次，我们将使用：`expect(GoogleCloudStorageMock, :upload, 3, fn "test_key", "test_content" -> {:ok, :uploaded} end)`

我们使用 lambda 参数上的模式匹配来验证模拟是否使用正确的参数被调用。如果模拟使用不同的参数被调用，匹配将失败，进而导致测试失败。如果模拟使用正确的参数被调用，它将返回 lambda 的结果。在这种情况下，我们只是返回 `{:ok, :uploaded}`，以便在被测试的模块中模式匹配正常工作，使测试通过。

设置此期望后，我们只需创建必要的测试数据，其中定义这是一个上传任务，然后将其传递给我们在主题上测试的函数。查看此测试，很清楚在此测试上设置了哪些期望，因此很容易理解其目的。如果不是设置期望，你只想存根该函数调用，可以直接调用 `Mox.stub/3` 函数。要查看此和其他 Mox 上可用的函数，请查看其文档，网址为 https://hexdocs.pm/mox/Mox.html。

请注意，我们在文件顶部仍然有 `async: true`，这意味着尽管我们使用模拟，此测试仍将并发运行。然而，这可能并不总是如此。特别是，如果你测试的代码正在生成新进程（例如生成一个 GenServer），你将无法对这些进程发出的函数调用设置期望。在这些情况下，你可以配置 Mox 以全局模式工作，其中每个进程都可以访问由不同进程设置的期望（并停止使用 `async: false` 并发运行测试），或明确允许某个进程访问期望（允许测试仍并发运行）。请查看前面提到的文档，了解如何使用这些模式的详细信息。

我们现在已经看到了如何使用 Mox 在 Elixir 中创建功能模拟。在结束本节关于带副作用函数的测试之前，让我们谈谈测试与存储库交互的函数。

## 单独测试与存储库的交互

我们现在将探讨如何单独运行与应用程序存储库交互的测试。在本章开始时，我们为 `%Media{}` 结构体创建了一些测试，并了解了如何在不触及数据库的情况下测试 `changeset` 逻辑。现在，我们将看到如何实际运行命中数据库的测试。

一个时不时被问到的问题是如何为存储库创建模拟，以便我们可以独立测试与之交互的函数。虽然存储库确实是另一个依赖项，但我们认为为其创建模拟毫无意义，因为在存储库测试中，我们希望验证我们的命令和查询按预期工作。我们已经在 `changeset` 测试中测试了业务逻辑，现在我们希望确保一切都按应有的方式粘合在一起。如果我们要模拟存储库，那么我们仍然需要创建集成测试来验证这一点，那么在这种情况下创建模拟有什么优势呢？

人们希望在测试中避免命中数据库的主要原因之一是它会减慢测试套件的速度，从而创建更慢、因此生产力更低的反馈循环。然而，我们能够并发运行与数据库交互的测试，大大消除了这种担忧的原因。让我们现在看看如何并发运行命中数据库的测试。首先，我们需要在测试环境中配置数据库池以使用特殊的沙箱适配器：

```
$ cat apps/elixir_drip/config/test.exs
use Mix.Config

# ...

config :elixir_drip, ElixirDrip.Repo,
  pool: Ecto.Adapters.SQL.Sandbox
```

此适配器将每个数据库连接包装在事务中，从而创建一种安全机制来并发运行针对数据库的测试。尽管适配器的名称包含 SQL，并发模式不能用于所有 SQL 数据库。目前，它只能与 PostgreSQL 一起使用，因为将其与 MySQL 一起使用可能会导致死锁，从而创建脆弱的测试套件。现在，我们需要更新 test_helper.exs，以配置池模式：

```
$ cat apps/elixir_drip/test/test_helper.exs
# ...
Ecto.Adapters.SQL.Sandbox.mode(ElixirDrip.Repo, :manual)
```

这将把池模式配置为 :manual，这意味着在每个测试中我们将显式签出一个连接。让我们现在看看测试本身。尽管我们将对 %Media{} 模式进行查询测试，但由于我们的设计基于有界上下文，此模式的存储库交互位于 Storage 上下文中。因此，我们的测试将针对 Storage 模块。另外，请注意，我们将存储库测试分离到不同的文件中，这样我们可以清楚地看到哪些测试正在与存储库（和数据库）交互，哪些测试是纯单元测试。让我们检查此测试的代码：

```
$ cat apps/elixir_drip/test/elixir_drip/storage/storage_repo_test.exs
defmodule ElixirDrip.StorageRepoTest do
  use ExUnit.Case, async: true

  @subject ElixirDrip.Storage

  setup do
    :ok = Ecto.Adapters.SQL.Sandbox.checkout(ElixirDrip.Repo)
  end

  describe "when the user already uploaded some media" do
```
