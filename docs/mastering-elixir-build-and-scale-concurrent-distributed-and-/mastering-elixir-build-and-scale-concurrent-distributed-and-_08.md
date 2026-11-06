# 9 Finding Zen through Testing

在功能方面，我们的 ElixirDrip 应用程序已经完成。然而，我们还没有任何测试，因此它实际上还不准备部署到实时环境。
尽管我们把测试留到了最后，但我们要指出，这并不是我们通常开发应用程序的方式。我们非常支持在构建应用程序功能的同时逐步创建测试。然而，我们认为这种方法会令人困惑，对本书的可读性弊大于利。

因此，我们在本章中汇总了我们将要探索的所有类型的测试。我们不会涵盖 ElixirDrip 的每个模块和函数，但我们将介绍测试此应用程序所需的所有概念。我们不会涵盖所有类型的测试——例如端到端测试——因为我们将重点关注在构建 Elixir 应用程序时更常用的测试类型。

有一些测试原则在各种语言和框架中都是共享的。我们不会深入探讨每种原则的优点和缺点，因为这超出了本书的范围。尽管如此，让我们简要介绍一下适用于我们稍后将深入探讨的每种测试类型的一些原则：

- 测试应具有适当的隔离级别。在某些类型的测试中，我们希望使用模拟，而在其他类型中，我们希望测试所有真实组件之间的交互。
- 测试结果应可重复。我们的测试套件的结果应该是确定的，因此不应依赖任何外部因素。测试套件中存在不稳定的测试会降低我们对它的信心，而这正是我们开始编写测试的最初原因。
- 运行整个测试套件应很快。*快* 的定义非常相对，但无论如何你都不应该考虑是否要运行它。它应该快到你会不断运行它，并获得非常短的反馈循环。




In this chapter, we'll cover the following topics:

- Creating unit tests, with and without mocks
- Having separate repository tests, which will test the interaction with the database
- Using doctest to embed tests inside a function's documentation
- Writing integration tests, which verify that the application is working as a whole
- Creating tests for our Phoenix Channels, which verify the connection to the socket and exchange of events work as expected
- Testing macros in a maintainable way
- Creating property-based tests, and some considerations on how to integrate them into an application

Let's now write the first type of tests in our application: unit tests.

## Unit testing

In this section, we'll explore how to create tests in Elixir, using ExUnit. We'll split it between tests that exercise functions with and without side-effects, since we have different approaches for each type of function. Let's begin by the simpler case: testing functions that don't have side-effects.

### Testing functions without side-effects

A unit test exercises a function from a certain module of the application. We might have more than one test exercising the same function (with different inputs, for instance). Let's begin by creating a simple test, which will allow us to analyze ExUnit in detail. We'll be creating the tests for the is_valid_path? function, which belongs to the ElixirDrip.Storage.Media module. For context, we present its implementation as follows:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/media.ex
defmodule ElixirDrip.Storage.Media do
  # ...

  def is_valid_path?(path) when is_binary(path) do
    valid? = String.starts_with?(path, "$" ) && !String.ends_with?
    (path, "/")

    case valid? do
      true -> {:ok, :valid}
        false -> {:error, :invalid_path}
    end
    end

    # ...
end
```

The logic in this function is very straightforward: to validate a path, it checks if the provided string starts with a $ and doesn't end with a /. Let's now see our first test for this function:

```
$ cat apps/elixir_drip/test/elixir_drip/storage/media_test.exs
defmodule ElixirDrip.Storage.MediaTest do
  use ExUnit.Case, async: true

  @subject ElixirDrip.Storage.Media

  test "is_valid_path?/1 returns {:ok, :valid} for a valid path" do
    path = "$/abc"

    assert {:ok, :valid} == @subject.is_valid_path?(path)
  end
end
```

We begin by calling use ExUnit.Case, which will make some macros available in our test module. We're passing the async: true option, which will make the tests in this module run concurrently. By default, the tests don't run concurrently. Since in Elixir we generally try to write code that's side-effect free, most of our tests will be able to run concurrently. Then we define a module attribute, named @subject, which contains the module that we're testing. This way, it's very clear which module is being tested, and also brings the benefit of only needing to change one line in the event that we rename the module.

Let's now analyze the test macro, imported from ExUnit.Case. This macro receives a string with the description for the test, and a block for the test code. In this case, we're testing for when the path is valid. We then use the assert macro, also from ExUnit.Case, to validate that the return value of the is_valid_path? function is the expected one. Let's now run our test. Since we have an umbrella application, running the tests in the root folder will target every application inside the umbrella.

For now, we're only interested in the elixir_drip application, so we change the working directory to apps/elixir_drip:

```
$ cd apps/elixir_drip
$ MIX_ENV=test mix test
.

Finished in 0.02 seconds
1 test, 0 failures

Randomized with seed 69987
```

As you can see, we run our tests with the mix test task. Without any argument, it will run every test in our suite. You provide the path to a test file to run the tests of a certain file, or even a path of a test file with a colon and a line number suffixed, which will just run the test on that line.

It's not mandatory to use the assert macro to check the outcome of each test. We might as well just use plain old pattern matching to validate the return values of our functions. However, we recommend the usage of the assert macro in test cases, mainly because it'll yield better error messages when the tests fail, allowing you to quickly understand what's going on. For instance, we've intentionally made the previous test fail, by matching on {:ok, :validdd}. Running the test with this change yields:

![](img/c636f4cfb07f25f1db535995e50706ed_418_0.png)

```
$ cd apps/elixir_drip
$ MIX_ENV=test mix test
1) test is_valid_path?/1 returns {:ok, :valid} for a valid path (ElixirDrip.Storage.MediaTest)
test/elixir_drip/storage/media_test.exs:6
Assertion with == failed
code: assert {:ok, :validdd} ==
@subject.is_valid_path?(path)
left: {:ok, :validdd}
right: {:ok, :valid}
stacktrace:
test/elixir_drip/storage/media_test.exs:9: (test)
Finished in 0.02 seconds
1 test, 1 failure
Randomized with seed 69303
```

We get both sides of the match we passed to assert, making it easy to understand why the match failed.

To end this section, we'll test a different function from the same module, which will allow us to explore ExUnit even further. We'll test the create_initial_changeset/4 function, which is responsible for creating the %Media{} changeset based on the provided attributes. Whereas in other frameworks, you have the validation logic tightly coupled with the database, in Ecto, we're able to test our validation logic as pure unit tests, which we're able to run concurrently. This is possible because Ecto has a modular design, decoupling the underlying database schema from the repository. Later in this chapter, we'll talk about repository tests that will actually hit the database. For now, we'll just focus on testing changesets.

In the previous test, the input data was created inside the test itself. Now, we'll see how to use the setup macro to inject context into our tests:

```
$ cat apps/elixir_drip/test/elixir_drip/storage/media_test.exs
defmodule ElixirDrip.Storage.MediaTest do
  use ExUnit.Case, async: true

  @subject ElixirDrip.Storage.Media

  setup do
    user_id = "0ujsswThIGTUYm2K8FjOOXfXtY1K"
    file_name = "test.txt"
    full_path = "$/abc"
    file_size = 123

    {:ok, user_id: user_id, file_name: file_name, full_path: full_path,
    file_size: file_size}
  end

  test "create_initial_changeset/4 returns a valid changeset", context do
    changeset = @subject.create_initial_changeset(context.user_id,
    context.file_name, context.full_path, context.file_size)

    assert changeset.valid? == true
  end

  # ...
end
```

Inside the setup block, we're creating the variables we need and returning them in the block's response. This will be injected into each test, and we can access it by binding it to a variable before the test block. We could even use pattern matching here to extract a certain field from the context map. Then, inside the test, we create a changeset with the context data and use the assert macro to make sure the changeset is valid. This approach allows us to define test data in only one place, which will then be shared by all the tests in this module. The setup macro runs before each test. If you want to run a certain block just once, for instance to initialize something that is shared between the tests, you can use the setup_all macro.

When creating unit tests, we usually don't just cover the happy path, and also test the error cases. Let's now create a test to check that a changeset isn't valid when we provide incorrect data:

```
$ cat apps/elixir_drip/test/elixir_drip/storage/media_test.exs
defmodule ElixirDrip.Storage.MediaTest do
  # ...

  describe "when the file name is invalid" do
    setup :invalid_file_name

    test "the changeset isn't valid", context do
      changeset = @subject.create_initial_changeset(context.user_id,
      context.file_name, context.full_path, context.file_size)

      assert changeset.valid? == false
    end

    test "the changeset contains the expected error message", context do
      changeset = @subject.create_initial_changeset(context.user_id,
      context.file_name, context.full_path, context.file_size)

      assert changeset.errors == [file_name: {"Invalid file name", []}]
    end
  end

  # ...

  defp invalid_file_name(_context) do
    {:ok, file_name: "a/bc" }
  end
end
```



我们使用 `describe` 函数来对具有相同上下文的测试进行分组。在此例中，我们为 `file_name` 参数无效时创建了两个测试。在 `describe` 块内部，我们可以再次使用 `setup` 来为此测试用例准备上下文。在本示例中，我们还看到了一种不同的使用 `setup` 宏的方式。与之前示例中提供块不同，我们也可以提供一个原子，即本模块中用于设置所需上下文的函数名称。传递给 `setup` 的函数始终接收一个参数，即上下文映射，并返回可能被修改过的上下文。就像之前的示例一样，此上下文随后会被注入到 `describe` 块内的每个测试中。

除了使用 `setup` 宏的不同之处外，此示例与前一个示例非常相似。我们使用上下文中的数据创建一个 `changeset`，然后对每个测试的预期进行断言。在本示例中，这是在检查 `changeset` 是否无效，并且包含预期的错误信息。

我们想指出 `describe` 块的一个可能让你意外的特定方面：它们只能有一层。换句话说，不允许在其他 `describe` 块内部嵌套 `describe` 块。虽然这乍一看可能很奇怪，特别是如果你习惯在其他测试库中这样做，但这带来了很大的好处：每个测试的目的和上下文变得非常清晰。这是 Elixir 核心团队的有意决定，其理由是这些块中过多的嵌套会妨碍测试的可读性。你必须导航到最外层的块，将其上下文记在脑中，然后逐步向下移动直到找到期望的测试。通过不允许这种嵌套，ExUnit 迫使我们在保持测试对其他人（或我们未来的自己）可理解的同时，使其更具描述性。

![](img/c636f4cfb07f25f1db535995e50706ed_421_0.png)

你可以向 `mix test` 任务传递 `--trace` 选项，以在测试运行时将每个测试的描述打印到终端。除了描述之外，你还可以获得每个测试完成的耗时。

我们现在已经到达本节的末尾。如你所见，通过将数据验证与存储库分离，我们能够并行运行与 `changeset` 函数交互的测试，因为这些函数没有副作用。然而，最终我们的应用程序中会包含一些具有副作用的函数。现在让我们探索如何为这些函数编写测试。

## 使用副作用的函数进行测试

虽然我们在应用程序中力求编写纯函数，但这并不总是可能的。在本节中，我们将探讨如何测试具有副作用的函数。在大多数其他编程语言中，这是通过创建模拟来验证是否产生了正确的副作用来实现的。然而，在 Elixir 中，这类模拟尽可能被避免，因为它们通常依赖于全局更改某个模块或函数。这种更改使我们必须顺序运行测试，这会增加运行时间，从而从长远来看降低我们的生产力。

我们仍然会探讨如何在 Elixir 中创建模拟，但请注意它们的使用频率不如其他编程语言的测试框架那么高。如果你要测试的模块依赖于应用程序中另一个模块的简单纯函数，为它添加模拟有什么额外好处呢？这显然是我们每个人都必须处理的权衡，而走向另一个极端可能会导致冗余的覆盖以及所有相关问题。

无论你采用什么方法，某些情况下使用模拟可能是必要的。例如，当我们的应用程序联系外部实体时，我们可能希望创建一个模拟，以便测试不依赖于实际请求。我们现在将看到如何在 Elixir 中创建不改变全局状态的模拟，从而允许我们并发运行测试。

## 使用 Mox 创建模拟

如前所述，我们通常尽量避免在 Elixir 中使用模拟，尤其是动态模拟（也称为临时模拟）。它们之所以被称为这样，是因为它们通常在每个测试内部动态创建，只是为了验证该测试上是否满足了某个期望。除了改变全局行为且不允许我们并发运行测试外，这类模拟还有另一个缺点：它们与所替代的真实实现没有任何关联，这使得模拟和真实实现可能逐渐脱节，可能导致所有测试都通过而你的应用程序在生产环境中却损坏。

出于所有这些原因，我们将使用由 José Valim 创建的 Mox 库来创建模拟，它试图解决前面描述的问题。在 Elixir 中创建模拟还有其他选择，但我们认为这是最符合 Elixir 哲学的方式。

在进入测试本身之前，让我们先看看我们将要测试的函数。在第 6 章《元编程 – 编写代码的代码》中，我们构建了一个上传和下载管道，可用于将文件在 Google Cloud Storage 中移动。一个步骤是 :remote_storage 步骤，在此步骤中实际进行上传/下载。为了演示模拟的使用，我们将为此管道步骤的上传部分创建一个测试。让我们看看其实现：

```elixir
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/
remote_storage.ex
defmodule ElixirDrip.Storage.Pipeline.RemoteStorage do
  alias ElixirDrip.Storage.Providers.GoogleCloudStorageLive

  # ...

  @impl GenStage
  def handle_events(tasks, _from, _state) do
    processed = Enum.map(tasks, &remote_storage_step(&1))
    {:noreply, processed, @dummy_state}
  end

  defp remote_storage_step(%{media: %{id: id, storage_key: storage_key}
  = media, content: content, type: :upload} = task) do
    Logger.debug("#{inspect(self())}: Uploading media #{id} to #{storage_key}, size: #{byte_size(content)} bytes.")
    {:ok, :uploaded} = GoogleCloudStorageLive.upload(storage_key,
    content)
    task
  end
end
```

我们正在测试 handle_events/3 函数，特别是当任务的类型为 :upload 时。我们想要引入的模拟是针对 GoogleCloudStorageLive 模块，因为我们不想在测试中实际联系 Google 的 API。我们现在将看到如何使用 Mox 库来实现这一点。首先，我们需要注入依赖，以便可以根据运行的环境进行替换。以下是我们更新后的模块：

```elixir
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/remote_storage.ex
defmodule ElixirDrip.Storage.Pipeline.RemoteStorage do
  @storage_provider Application.get_env(:elixir_drip, :storage_provider)

  # ...

  defp remote_storage_step(%{media: %{id: id, storage_key: storage_key}
  = media, content: content, type: :upload} = task) do
    Logger.debug("#{inspect(self())}: Uploading media #{id} to #{storage_key}, size: #{byte_size(content)} bytes.")
    {:ok, :uploaded} = @storage_provider.upload(storage_key, content)
    task
  end
end
```

现在我们需要更新应用程序的配置。首先，我们在主配置文件中添加此条目：

```elixir
$ cat apps/elixir_drip/config/config.exs
use Mix.Config

# ...

config :elixir_drip, storage_provider:
ElixirDrip.Storage.Providers.GoogleCloudStorageLive

import_config "#{Mix.env()}.exs"
```

这样，实际实现就是默认实现，然后可以根据环境进行覆盖。这是通过 config.exs 的最后一行实现的，在那里我们根据 MIX_ENV 环境变量导入新的配置文件。现在，我们更新测试环境的配置文件：

```elixir
$ cat apps/elixir_drip/config/test.exs
use Mix.Config

# ...

config :elixir_drip, storage_provider:
ElixirDrip.Storage.Providers.GoogleCloudStorageMock
```

在此阶段，根据环境的不同，会注入不同的模块作为存储提供者。在进入测试代码之前，让我们先设置 Mox。首先，我们需要在 Mix 文件中将其添加为依赖项：

```elixir
$ cat apps/elixir_drip/mix.exs

defmodule ElixirDrip.Mixfile do
  # ...
  defp deps do
    [
      # ...
      {:mox, "~> 0.3", only: :test}
    ]
  end
end
```

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

```
setup [:create_user, :create_media]

@tag :repository
test "finds the media by ID", %{user: user, media: media} do
  {:ok, :download_enqueued, retrieved_media} =
    @subject.retrieve(user.id, media.id)

  expected_media_id = media.id
  assert %{full_path: "$", id: ^expected_media_id, name:
    "test.txt"} = retrieved_media
end
end

defp create_user(_context) do
  {:ok, user} =
    ElixirDrip.Accounts.create_user(%{
      username: "test",
      password: "12345678",
      email: "test@test.com"
    })

  {:ok, user: user}
end

defp create_media(%{user: user}) do
  {:ok, :upload_enqueued, media} = @subject.store(user.id,
  "test.txt", "$", "some cool content")
  {:ok, media: media}
end
end
```

首先，我们有 `setup` 块，在其中我们从连接池检出一个连接，正如我们之前讨论的那样。这样我们的当前进程将成为该数据库连接的所有者。在测试的其余部分，我们使用本章中已学到的技术来创建描述性的测试。我们使用 `describe` 块为某个测试（或一组测试）设定上下文，然后在其内部使用 `setup` 来为测试运行安排必要的状态。在本例中，这意味着在数据库中创建一个新用户，然后为该用户创建一个新的媒体条目。然后，测试本身验证我们是否能够根据其 ID 和关联的用户 ID 获取媒体条目。最后，我们使用 `assert` 宏来验证我们刚刚从数据库中获取的记录是我们期望的记录。

与 Mox 的情况类似，当你的测试（或正在测试的代码）生成新进程时，就需要做额外的工作。在这种情况下，适配器可以从 :manual 更改为 :shared 模式，或者为某个进程设置显式的允许。请参考适配器文档，网址为 https://hexdocs.pm/ecto/Ecto.Adapters.SQL.Sandbox.html，了解如何为这些情况配置适配器。

此测试的最后一个值得注意的部分是 @tag 属性的使用。标签允许我们为每个测试（或一组测试）添加元数据，然后可用于仅运行（或阻止运行）某个测试组。在本例中，我们将此测试标记为 :repository，因为它与存储库交互。有了此设置，我们就可以例如配置本地开发环境以运行除存储库测试之外的所有测试，而仅在 CI（持续集成）环境中运行完整的测试套件。我们可以通过以下方式运行除存储库测试之外的所有测试：

```
$ MIX_ENV=test mix test --exclude repository:true
Excluding tags: [repository: "true"]
.....
Finished in 0.1 seconds
7 tests, 0 failures, 1 skipped
```

正如我们所见，我们有一个测试被跳过，即我们刚刚创建的存储库测试。相反，如果我们只想运行存储库测试，可以传递选项 --only repository:true。此时，我们将 :repository 标签放在测试本身上。当测试模块开始包含更多测试时，我们也希望这些测试被标记。为此，我们可以使用 @moduletag 指令，它将给定的标签放在当前模块的所有测试上：

```
$ cat apps/elixir_drip/test/elixir_drip/storage/storage_repo_test.exs
defmodule ElixirDrip.StorageRepoTest do
  use ExUnit.Case, async: true

  @moduletag :repository

  # ...
end
```

有了它，我们就到达了关于具有副作用的函数的部分的结尾。让我们现在探索 Elixir 的一个非常不错的功能：测试文档。

## 测试文档

在本书中，我们一直说文档是 Elixir 的一等公民，这在为新人员上手该语言（甚至是让经验丰富的开发者接触到某个应用程序）时非常有帮助。其关键推动因素之一是能够在函数的文档中嵌入测试，这将确保文档与代码保持同步。

几乎每个开发者都认识到简洁且描述性文档的价值，因为它极大地帮助理解函数或模块，而无需在整个项目中跳转。文档的问题在于难以维护，因为代码可能在未相应更改文档的情况下更新，从而导致过时且误导性的文档。这导致某些社区干脆停止编写文档，因为没有文档比有欺骗性文档更好。为了解决这个问题，同时保留适当的文档带来的巨大价值，Elixir 和 ExUnit 允许我们在文档中嵌入代码片段，这些片段将被执行并与文档中定义的预期输出进行比较。此功能的名称是 doctest。

为了举例说明 doctest 的创建，我们将为之前测试过的函数 ElixirDrip.Storage.Media.is_valid_path?/1 添加文档：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/media.ex
defmodule ElixirDrip.Storage.Media do
  # ...

  @doc"""
  Verifies if the provided `path` is valid or not.

  ## Examples

      iex> ElixirDrip.Storage.Media.is_valid_path?("$ /abc")
      {:ok, :valid}

When we provide an invalid path (that either doesn't start with `$` or ends with `/`), it returns {:error, :invalid_path}

      iex> ElixirDrip.Storage.Media.is_valid_path?("$ /abc/")
      {:error, :invalid_path}

      iex> ElixirDrip.Storage.Media.is_valid_path?("/abc")
      {:error, :invalid_path}
  """
  def is_valid_path?(path) when is_binary(path) do
    # ...
```
```
end
  # ...
  end
```

在这里我们使用 @doc 模块属性编写文档，如常。然后，在其中我们引入以 iex> 开头的行，并在其后附加我们想要测试的代码。下一行包含预期输出。ExUnit 然后会提取 iex> 之后的代码，运行它，并将其与下一行中写的内容进行比较。为了让这些测试作为我们测试套件的一部分运行，我们需要在该模块的测试文件中添加以下内容：

```
$ cat apps/elixir_drip/test/elixir_drip/storage/media_test.exs
defmodule ElixirDrip.Storage.MediaTest do
  # ...

  doctest @subject

  # ...
end
```

@subject 模块属性之前已定义，包含我们正在测试的模块的名称。通过添加这一行，ExUnit 将把我们刚刚创建的 doctest 片段作为测试套件的一部分运行。让我们现在运行它们：

```
$ MIX_ENV=test mix test
..........

Finished in 12.6 seconds
3 doctests, 7 tests, 0 failures
```

正如我们所见，我们现在在测试套件中有 3 个 doctest。让我们现在故意让测试失败，以便在这种情况下观察其输出。我们修改了文档，使其期望返回值为 {:ok, :other_atom}。让我们检查其输出：

```
$ MIX_ENV=test mix test
......

1) doctest ElixirDrip.Storage.Media.is_valid_path?/1 (1)
(ElixirDrip.Storage.MediaTest)
  test/elixir_drip/storage/media_test.exs:5
Doctest failed
code: ElixirDrip.Storage.Media.is_valid_path?("$ /abc") === {:ok, :other_atom}
left: {:ok, :valid}
stacktrace:
  lib/elixir_drip/storage/media.ex:74: ElixirDrip.Storage.Media
```
```
(module)
..
Finished in 0.3 seconds
3 doctests, 7 tests, 1 failure
```

在这种情况下，测试失败，我们会得到一个很好的错误消息，使其易于理解发生了什么并进行必要的更正。至此，我们完成了关于 doctest 的这一小节，也完成了关于单元测试的更广泛部分。让我们现在继续下一节，在那里我们将学习如何编写集成测试。

## 集成测试

在涵盖了单元测试之后，让我们现在转向集成测试。在这种类型的测试中，我们感兴趣的是断言应用程序的不同层按预期协同工作。这通常通过在系统的边缘进行测试来实现，让应用程序中的所有组件相互交互，然后验证响应是否符合预期。

还有其他类型的测试，例如端到端测试，它更进一步，从最终用户的角度测试系统。在我们的例子中，这将通过浏览器进行，点击应用程序中的按钮和链接，然后断言用户看到预期信息。虽然本书不涵盖这种类型的测试，但本节中我们将讨论的大部分内容也适用于端到端测试。

在这种从外部观察应用程序的测试中，我们不想测试可能发生的每一种状态组合。那应该在单元测试级别完成，检查被测对象的每个可能分支。在集成级别，我们测试用户可能采取的主要路径。

如前所述，这些测试将在系统的边界进行。这意味着我们将像浏览器一样命中端点，这将经过 Phoenix 管道和控制器，与 elixir_drip umbrella 应用程序交互，最后通过渲染视图返回响应。作为集成测试的示例，我们将创建一个带有 GET /files 请求的测试，该请求将进入文件控制器的 index 动作。让我们现在检查它：

```
$ cat apps/elixir_drip_web/test/elixir_drip_web/controllers/
file_controller_test.exs
defmodule ElixirDripWeb.FileControllerTest do
  use ElixirDripWeb.ConnCase, async: true
```

```
setup [:create_user, :login_user, :create_media]

test "GET /files lists the files for the current user", %{conn: conn, media: media} do
  response = get(conn, "/files?path=$")

  assert html_response(response, 200) =~ "test.txt"
  assert html_response(response, 200) =~ "/files/#{media.id}/download"
end

defp create_user(_context) do
  {:ok, user} =
    ElixirDrip.Accounts.create_user(%{
      username: "test",
      password: "12345678",
      email: "test@test.com"
    })

  {:ok, user: user}
end

defp login_user(%{conn: conn, user: user}) do
  conn = Plug.Test.init_test_session(conn, user_id: user.id)
  {:ok, conn: conn}
end

defp create_media(%{user: user}) do
  {:ok, :upload_enqueued, media} =
    ElixirDrip.Storage.store(user.id, "test.txt", "$", "some cool content")

  {:ok, media: media}
end
end
```

我们首先调用 use ElixirDripWeb.ConnCase，它会导入一些便捷模块用于在测试中创建和使用连接。如果你想查看其内容，它位于 apps/elixir_drip_web/test/support/conn_case.ex。我们随后有一个 setup，包含三个函数，它们会创建并登录一个用户，然后为该用户创建一个媒体文件。用户及其媒体的创建方式与我们在 StorageRepoTest 中看到的相同。这里的新内容是 login_user 函数。该 setup 函数使用 Plug.Test.init_test_session/2 函数来初始化会话，本质上是对之前创建的用户进行登录。这样，我们的测试请求就会像真实请求一样运行，包括经过我们在上一章中创建的身份验证 Plugs。

在测试的设置完成之后，我们现在可以分析测试本身。如前所述，我们在测试头部使用模式匹配来获取在 setup 期间创建的 conn 和 media。然后我们使用 get 函数发出请求，指定我们要测试的路径。请注意，我们并不是直接调用控制器，而是通过路由器和管道（如果适用），使我们的请求与真实请求非常相似。

然后我们将此 GET 请求的响应绑定到 response。现在，我们需要对该响应的内容进行断言，以确保我们的应用程序按预期运行。为此，我们使用 html_response 函数，这是一个 Phoenix 辅助函数，可以让我们的测试保持简洁和清晰。使用此辅助函数，我们断言响应的状态码为 200，并且它包含我们在测试中指定的二进制内容。在本例中，我们检查文件名称 test.txt 是否显示在响应中，并且下载此媒体的链接是否存在。

在测试了 happy path 之后，让我们现在测试系统在接收到未登录用户请求时的行为是否符合预期。利用我们学到的知识，我们将创建两个上下文：一个用于用户已登录的情况，其中包含我们刚刚看到的测试，另一个用于用户未登录的情况。让我们查看更新后的测试文件：

```
```elixir
$ cat apps/elixir_drip_web/test/elixir_drip_web/controllers/file_controller_test.exs
defmodule ElixirDripWeb.FileControllerTest do
  use ElixirDripWeb.ConnCase, async: true

  describe "when the user is authenticated" do
    # ...
  end

  describe "when the user is NOT authenticated" do
    setup [:create_user, :create_media]

    test "GET /files redirects the user to '/'", %{conn: conn} do
      response = get(conn, "/files?path=$")

      assert html_response(response, 302) =~ "You are being <a href=\"/\">redirected</a>"
    end
  end

  # ...
end
```
```

此测试与我们之前看到的测试非常相似。不同之处在于，我们现在在 setup 中不再调用 `login_user` 函数，因此我们的断言也不同。我们现在断言状态码为 302，并且我们被重定向到 /，该页面会提示用户登录。

此时，你可能会想，随着测试用例越来越多，这种方法是否还能管理得过来。很可能还会有更多测试需要已登录的用户。为此，我们可以将此测试中的私有函数抽象到一个 `Case` 中，就像 Phoenix 生成器自动为我们提供 `ConnCase` 一样。要创建新的 case，我们只需创建一个新模块，调用 `use ExUnit.CaseTemplate`，然后提供一个 `setup` 块，在其中注入已创建的用户以及用户已登录的修改后连接。然后，每个需要已登录用户的测试只需使用 `use` 调用新创建的模块的名称即可。

减少测试中重复的另一种常见方法是使用工厂库，它们会自动为我们生成测试数据。我们认为只有在真正需要时才应采用此类库。在大多数情况下，仅使用生成测试数据的函数就足够了。这可能会在具有相似输入的模块测试之间引入一些重复，但我们认为立即知道测试需要什么所带来的明确性和可读性提升，超过了重复的成本。这是一个微妙的权衡，就像应用程序中的大多数决策一样，你应定期重新评估它。

为了结束本节，我们想谈谈直接测试控制器和视图。我们将在应用程序中直接测试它们，因为我们刚刚编写的集成测试已经覆盖了它们。一般来说，我们力求避免在控制器和视图中放置过多逻辑，因此集成测试已足够。我们不想专门为控制器和视图编写测试，因为这会导致冗余覆盖。然而，由于 Elixir 和 Phoenix 的函数式特性，控制器中的 action 和视图本身只是函数。如果由于某种原因你确实需要直接测试控制器或视图，可以使用本章中探讨的技术来完成。

现在我们有了第一个集成测试。用于测试 `GET /files` 的原则可用于测试应用程序的其他功能。请记住，我们不想在这里测试每一种可能的情况；我们只想测试部署应用程序时所需的足够多的内容。让我们现在继续创建 Phoenix Channels 的测试。

## 测试 Phoenix Channels

到目前为止，我们已经涵盖了单元测试和集成测试。现在，我们将探讨如何为上一章中创建的 Phoenix Channels 创建测试。这本身并不是一种测试类型，但仍很重要单独探索，因为它附带了一套自己的辅助工具。

如上一章所述，连接到 channel 是一个两步过程：我们首先连接到 socket（在我们的例子中是通过 UserSocket 模块）以建立与服务器的连接；然后，在 socket 已连接到服务器后，我们使用它加入我们感兴趣的 channels。因此，本节将有两个测试用例：一个用于 UserSocket 模块，我们将在其中测试该模块是否正确处理身份验证，另一个用于 UserChannel 模块，我们将在其中测试 channel 是否对我们创建的事件做出预期反应。让我们从 UserSocket 测试开始：

```
$ cat apps/elixir_drip_web/test/elixir_drip_web/channels/user_socket_test.exs
defmodule ElixirDripWeb.UserSocketTest do
  use ElixirDripWeb.ChannelCase, async: true

  @subject ElixirDripWeb.UserSocket

  describe "with a valid token" do
    test "it connects to the socket" do
      token = Phoenix.Token.sign(@endpoint, "user socket auth", "13")

      assert {:ok, socket} = connect(@subject, %{"token" => token})
      assert socket.assigns.user_id == "13"
    end
  end

  describe "with an invalid token" do
    test "it doesn't connect to the socket" do
      assert :error == connect(@subject, %{"token" => "010101"})
    end
  end

  describe "without providing a token" do
    test "it doesn't connect to the socket" do
      assert :error == connect(@subject, %{})
    end
  end
end
```



与我们拥有 ConnCase 来为测试中的连接提供辅助函数一样，我们也有 ChannelCase 来为测试内部的通道提供辅助函数。在此测试用例中，我们将使用 connect 函数，该函数允许我们为 socket 处理程序创建传输连接，以测试其身份验证机制。ChannelCase 模块由 Phoenix 生成，其位于 apps/elixir_drip_web/test/support/channel_case.ex。

在第一个测试中，我们提供一个有效的 token，然后断言 socket 的连接成功，并且 socket.assigns 中的 user_id 是我们期望的那个（即我们在签名 token 时提供的那个）。然后我们测试两个负面条件：当用户提供无效 token 时，以及当用户完全没有提供 token 时。这两个测试非常相似，因为我们在两个测试中都检查是否返回了 :error 原子，这意味着 socket 的连接未建立。由于这些测试不会产生副作用，我们可以并发运行它们，这就是为什么我们在 ChannelCase 中将 async: true 作为选项设置的原因。

在有了这些测试之后，我们可以继续测试 UserChannel 模块。现在让我们查看它的代码：

```
$ cat apps/elixir_drip_web/test/elixir_drip_web/channels/user_channel_test.exs
defmodule ElixirDripWeb.UserChannelTest do
  use ElixirDripWeb.ChannelCase

  setup [:create_users, :create_media, :connect_to_socket]

  test "join users:lobby pushes a 'presence_state' event to the joiner", %{socket: socket} do
    {:ok, _reply, _socket} = subscribe_and_join(socket, "users:lobby")

    assert_push "presence_state", %{}
  end

  test "'share' events are emitted to the owner and sharee", %{socket: socket, user: user, sharee: sharee, media: media} do
    {:ok, _reply, socket} = subscribe_and_join(socket, "users:#{user.id}")
    ElixirDripWeb.Endpoint.subscribe(self(), "users:#{sharee.id}")

    push(socket, "share", %{"username" => "other_user", "file_id" => media.id})

    assert_broadcast "share", %{message: "Successfully shared the file with other_user."}
    assert_receive %Phoenix.Socket.Broadcast{
    payload: %{message: "test has just shared a file with you!"}
    }
    end

    # ...
    end
```

在深入测试之前，让我们先回顾一下应用程序中拥有的通道，这样可以使测试更容易理解。首先，我们有 "users:lobby" 主题，所有用户都会连接到它。它用于跟踪当前在线的用户，并由 Phoenix.Presence 提供支持。然后我们为每个用户都有一个不同的主题。主题的格式为 "users:<user_id>"，其中 <user_id> 表示连接到 socket 的用户的 ID。该通道用于仅向单个用户传递通知，例如下载或上传完成时。

为了使解释更清晰，我们隐藏了 setup 函数，因为它们与我们已经看到的类似。我们开始看到一种模式，并且我们本可以从拥有自己的 Case 中受益，在其中创建用户和媒体。然而，请注意，在这种情况下需求有些不同。我们现在需要创建两个用户：一个名为 test 的用户，它是创建的媒体的所有者，另一个名为 other_user 的用户，它是我们将与之共享媒体的用户。如果我们过早地将用户创建抽象到 Case 中，现在就需要停止并尝试将其适配到这个新需求。我们的建议是尽可能推迟这种类型的提取，先建立应用程序将拥有的大部分测试。一旦有了它们，模式会自然浮现，你将能够看到可以在哪里去除重复，而不会妨碍测试的可读性。至于 connect_to_socket 函数，它只是使用 connect 函数来建立连接，就像我们在 UserSocket 测试中所做的那样。

在我们的第一个测试中，我们检查加入 "users:lobby" 通道是否按预期工作。在这种情况下，我们希望验证 "presence_state" 事件是否被推送回刚刚加入的用户，因为这是加入该通道时发出的唯一事件。我们使用 assert_push 辅助函数，它将确保通道已为给定事件向用户推送消息。

下一个测试稍微复杂一些。我们现在想要测试当服务器接收到 "share" 事件时，它将调用必要的函数来与另一用户共享文件，同时也会向文件所有者和被分享者发出事件，告知共享成功。为了测试这一点，我们需要订阅两个通道：一个是文件所有者的通道，使用 subscribe_and_join 函数，如我们之前所做的那样；另一个是被分享者用户的通道，我们使用 ElixirDripWeb.Endpoint.subscribe 函数来完成。

完成此设置后，我们然后推送一个 "share" 事件，其中包含被分享者的用户名和我们想要共享的媒体的 ID。现在，我们想要断言正确的事件已被发出。首先，我们使用 assert_broadcast 来确保文件所有者收到了确认消息。其次，我们使用普通的 assert_receive 来检查测试进程是否收到了发给被分享者的消息。测试进程作为每个通道的常规客户端工作，这使我们能够做出我们一直在讨论的断言。

请注意，ElixirDripWeb.ChannelCase 提供的辅助函数使我们的测试简洁且描述性强。它们在幕后做了大量工作，简化了测试，使其清晰明了。

另一个需要注意的重要部分是，在此测试用例中我们没有 async: true。在此测试中与数据库交互的进程是通道进程，它在我们加入通道时动态生成。当并发运行测试时，这个新创建的进程不知道我们在测试进程中签出的数据库连接。因此，在这种情况下，我们需要使此测试顺序运行，以便 SQL.Sandbox 适配器以共享模式运行，允许进程共享数据库连接。然而，我们认为这不值得担心。如果我们的大多数测试已经并发运行，在运行测试套件时可能所有 CPU 内核都会被使用。如果在某些条件下你需要使测试串行运行，这不应对测试套件的运行时间产生明显影响。

在某些情况下，没有必要使测试串行运行，因为我们可以定义显式许可来共享数据库连接。在此测试中无法做到这一点，因为当我们获得新创建的进程 ID（在加入之后）时，该进程已经在尝试查询数据库，即在处理它自己发送的 :after_join 消息时。请参考 Ecto 的文档以了解如何使用显式许可，网址为 https://hexdocs.pm/ecto/Ecto.Adapters.SQL.Sandbox.html。

我们现在已经了解了如何为 Phoenix Channels 创建测试，因此本节到此结束。现在让我们学习如何为宏创建测试。

## 测试宏

在本节中，我们将了解如何为宏创建测试。特别是，我们将为我们在第 6 章《元编程 – 编写代码的代码》中创建的 defchrono 宏添加一个测试。与上一节中为 Phoenix Channels 添加测试一样，本节与某种测试类型无关，而是与展示如何测试我们创建的特定组件有关。

测试宏最常见且有效的方法是断言宏生成的代码的行为，而不是代码生成本身。直接测试生成的 AST 通常会导致脆弱且难以维护的测试。这些测试可以被视为宏的单元测试，因为它们测试的是宏展开本身。我们的重点应该是宏的集成测试。这意味着我们将创建一个测试模块，其唯一目的是在我们的宏上调用 use，然后在此测试模块上放置断言。让我们查看此测试的代码：

```
$ cat apps/elixir_drip/test/chronometer_test.exs
defmodule ElixirDrip.ChronometerTest do
  use ExUnit.Case, async: true

  import ExUnit.CaptureIO

  defmodule Factorial do
    use ElixirDrip.Chronometer, unit: :secs

    defchrono calculate(x) do
      _calc_factorial(x)
    end

    defp _calc_factorial(0), do: 1
    defp _calc_factorial(x) do
      x * _calc_factorial(x - 1)
    end
  end

  test "defchrono measures and prints the function execution time" do
    assert capture_io(fn ->
      Factorial.calculate(10000)
    end) =~ ~r/Took \d+\.\d+ secs to run
    ElixirDrip.ChronometerTest.Factorial.calculate\/1/
  end
end
```

在我们的测试中，我们创建了一个名为 Factorial 的模块。该模块有一个函数 calculate/1，用于计算所提供数字的阶乘。在该模块内部，我们在要测试的宏上调用 use，然后对 calculate/1 函数使用 defchrono 宏。由于我们使用了 defchrono 宏，每当调用此函数时，它都会将执行时间写入标准输出。



鉴于这是宏的行为，我们的测试随后需要重点断言在调用 calculate/1 函数时，该消息会被打印到标准输出。为此，我们使用 ExUnit 提供的一个辅助函数，名为 capture_io。我们使用此函数来断言，当我们调用 Factorial.calculate(10000) 时，预期的消息会被发送到标准输出。我们使用正则表达式是因为此函数调用的运行时间是可变的，我们只关心正确的消息是否被打印，而不关心确切花费了多长时间。

在有了这个测试之后，我们可以验证宏工作正常，而无需为此放弃测试套件的可维护性。由于此测试像真实模块一样使用宏，我们可以确信宏的行为符合预期，这使我们能够在不担心引入回归的情况下对其进行更新和改进。如果由于某种原因你发现自己需要为宏创建单元测试，你可以使用 Macro.to_string/2 函数，将宏从 AST 转换为 Elixir 字符串。然后，你可以比较这个字符串来检查宏是否按预期展开。然而，正如我们之前所建议的，使用这种方法时要谨慎，你可能会创建脆弱的测试，并检查是否可以在不检查宏展开的情况下测试相同的行为。

我们现在已经结束了关于测试宏的简短章节。接下来，我们将讨论一种非常不同类型的测试方法：基于源代码属性创建测试。

## Property-based testing

我们将通过讨论一种令人兴奋的新型测试方式来结束本章：基于属性的测试。我们称之为新是因为这种测试类型在 Erlang 和 Haskell 社区（以及可能其他社区）中已经使用了很长时间，现在正在进入 Elixir 社区。在撰写本文时，有强烈迹象表明基于属性的测试将成为 Elixir 核心的一部分，据说将在 Elixir 1.7 版本中发布。特别是，我们将在后面的示例中使用的库（stream_data）将被合并到 Elixir 本身。目前尚不清楚这将如何实现，以及库的 API 是否会发生变化。尽管如此，通过阅读本文并查看示例，你已经能够进入这种新的测试方式，即使未来做法有所不同。

在进入示例之前，我们需要讨论基于属性测试的目的和特征。与单元测试中我们作为开发者手动为每个测试定义输入条件不同，在基于属性的测试中，我们只需定义被测函数所具有的属性。在单元测试中，我们提前定义将要测试的条件，并检查我们认为重要的边界条件。在基于属性的测试中，在定义了属性之后，我们使用数据生成器创建大量输入数据，然后将其提供给被测函数。然后我们断言，由基于属性的测试框架生成的每个输入都尊重我们定义的属性。

在谈论基于属性的测试时，单元测试通常被称为 *example-based tests*。它们之所以这样称呼，是因为我们需要为每个函数想出示例进行测试。手工制作示例的一个缺点是，由于我们是编写代码的人，我们存在偏见。这些偏见会渗透到我们创建的示例中，导致测试套件不理想。另一方面，如果我们定义函数属性，并让测试框架生成输入数据，我们就不太可能遇到同样的缺点。

尽管更困难，但仍然可能创建有偏见或不准确的属性。但什么是属性？为了能够创建良好的属性，我们首先需要回答这个问题。属性是我们代码的特征行为，对于某些数据集而言是成立的。属性因被测函数而异，一开始很难定义它们。我们太习惯于传统的单元测试，以至于停下来思考代码的属性是非常困难的。虽然一开始这种做法会很痛苦，但随着时间的推移，你会越来越自信地定义你编写的代码的属性。除了通过用大量输入空间测试函数来提供更好的覆盖率之外，思考代码的属性也会在应用程序设计中带来好处。当我们停下来思考某个函数的属性，即对于某个输入空间始终成立的条件时，我们经常会发现一些之前未意识到的新范围，或开始思考与其他函数或模块发生的交互。

理论够多了！让我们查看我们第一个属性测试的代码：

```
$ cat apps/elixir_drip/test/elixir_drip/storage/pipeline/media_property_test.exs
defmodule ElixirDrip.Storage.MediaPropertyTest do
  use ExUnit.Case, async: true
  use ExUnitProperties

  @subject ElixirDrip.Storage.Media

  property "is_valid_path?/1 with valid paths returns {:ok, :valid}" do
    check all path <- string(:ascii),
              String.last(path) != "/" do
      assert @subject.is_valid_path?("$" <> path) == {:ok, :valid}
    end
  end
end
```

我们将为我们创建单元测试的同一个函数添加属性测试：is_valid_path?/1。我们只是为了使示例更清晰才将属性测试分离到自己的文件中，但我们完全可以在 %Media{} 模式的现有测试模块内部创建这些属性测试。首先要指出的是，我们调用了 use ExUnitProperties。这将为基于属性的测试提供一些必要的函数，例如 property/3 和 check/1。

然后，我们为有效输入范围定义属性，并且 is_valid_path?/1 将返回 {:ok, :valid}。为了定义有效输入范围，我们使用基于属性的测试框架提供的数据生成器。首先，我们生成一个包含 ASCII 字符的字符串。下一步是拒绝路径以 "/" 结尾的生成，因为那是无效路径。最后，在调用我们测试的函数之前，我们在路径前加上 "$"，以确保它以该字符开头。最后，我们的断言非常简单，匹配对于此输入范围函数始终返回 {:ok, :valid}。

现在让我们运行这些测试。由于我们使用了 property 函数，我们已经为其设置了标签。这样，要仅运行属性测试，我们可以执行以下操作：

```
$ MIX_ENV=test mix test --only property
Including tags: [:property]
Excluding tags: [:test]
.

Finished in 0.1 seconds
3 doctests, 1 property, 8 tests, 0 failures, 11 skipped
```

全部通过！唯一运行的测试是属性测试。此属性测试仅花费了 0.1 秒运行。由于它运行得如此之快，让我们稍微增加运行次数，看看如何操作。要配置此库中的运行次数，我们执行以下操作：

```
$ cat apps/elixir_drip/config/test.exs
use Mix.Config

# ...

config :stream_data, max_runs: 500
```

默认值为 100。将它增加到 500，让我们再次运行测试：

```
$ MIX_ENV=test mix test --only property
Including tags: [:property]
Excluding tags: [:test]

.

Finished in 0.1 seconds
3 doctests, 1 property, 8 tests, 0 failures, 11 skipped
```

我们现在拥有更广的输入空间，而我们的属性仍然只花费了 0.1 秒运行。在本地开发中，我们希望执行时间短以获得快速反馈循环。然而，在 CI 中执行时间并不那么重要，这使我们可以稍微增加这个数字并检查更大的输入范围，可能发现我们之前未意识到的边缘情况。现在让我们为 is_valid_path?/1 函数添加一个新的基于属性的测试：

```
$ cat apps/elixir_drip/test/elixir_drip/storage/pipeline/media_property_test.exs
defmodule ElixirDrip.Storage.MediaPropertyTest do
  # ...

  property "is_valid_path?/1 with invalid paths returns {:error, :invalid_path}" do
    check all path <- string(:ascii),
              String.first(path) != "$" do
      assert @subject.is_valid_path?(path) == {:error, :invalid_path}
    end

    check all path <- string(:ascii) do
      assert @subject.is_valid_path?(path <> "/") == {:error, :invalid_path}
    end
  end
end
```

这个新属性指出，对于所有无效路径，我们的函数返回 {:error, :invalid_path}。为了检查该属性对所有类型的无效路径都成立，我们在其中有两个生成器。一个生成器检查对于所有不以 "$" 开头的路径，函数是否返回预期值。另一个生成器检查对于所有以 "/" 结尾的路径，函数是否也返回预期值。现在让我们再次运行基于属性的测试：

```
$ MIX_ENV=test mix test --only property
Including tags: [:property]
Excluding tags: [:test]
```

```
Finished in 0.3 seconds
3 doctests, 2 properties, 8 tests, 0 failures, 11 skipped
```

再次，一切都是绿色的！测试框架生成了一大批输入数据，对于所有这些输入，is_valid_path?/1 函数的行为都符合预期。请注意，这并不意味着我们的函数没有 bug，因为基于属性的测试并不是软件的形式化验证。然而，我们对该函数编写正确有很大信心。在撰写本书时，我们未能让这些测试失败，但如果我们遇到失败，测试框架会将输入缩小到破坏该属性的最小可能输入。这非常有帮助，因为它使边界条件更容易理解，通常输入会被缩小到我们立即就能看出错误所在的地步。为了保持基于属性测试的可收缩性，请注意你必须使用 StreamData 库中的函数。

那么基于属性的测试如何与其他类型的测试集成到应用程序中呢？与测试相关的大多数方面一样，这将取决于团队的偏好。通常，它们并不打算取代单元测试或集成测试。这两种测试类型仍然各有优势。例如，单元测试非常适合直接测试繁琐的边界情况的场景。此外，由于基于属性的测试通常需要一些时间运行（因为它们会生成大量测试执行），我们认为你不应该为应用程序中的每个函数都编写它们。这会导致测试套件缓慢且难以管理。或者，你可以选择对保持应用程序运行至关重要的若干函数或模块，并为这些函数或模块创建基于属性的测试。这将提高你对这些关键组件的信心，进而也会提高应用程序的健壮性。

正如本节所见，基于属性的测试为你的测试套件带来了一些好处。它们让我们能更有信心地发布代码，这正是所有类型测试的目的。由于这些测试在每次运行都会生成不同的输入数据，随着时间的推移，你的测试会更进一步。除了额外的执行时间外，采用这种测试类型的唯一缺点是陡峭的学习曲线。为我们的函数创建和定义属性需要改变思维方式，这在开始时感觉非常困难。正如任何手艺一样，经验会让你越来越好，随着时间的推移，你会发现为函数创建属性变得自然而然。在开始这段旅程时，你可以使用一些模式。例如，如果你测试的函数有一个逆函数，你可以创建一个属性，说明逆函数会撤销原始函数执行的操作。

另一个有用的技巧是使用已有的实现来断言你正在测试的函数的行为。例如，如果你正在测试 Erlang 标准库中可用的函数，那么你的函数的输出可能与 Erlang 标准库中的输出相同。随着经验的积累，其他模式将会出现并成为你的工具箱的一部分。

我们现在已经到达了这个令人兴奋的部分的结尾。让我们通过回顾本章的内容来结束本章。

## 总结

在本章关于测试的内容中，我们涵盖了广泛的主题。让我们回顾最重要的内容：

- Elixir 中的测试由功能全面的 ExUnit 框架驱动，它使我们能够创建描述性强且简洁的测试，并且能够并发运行。
- 我们可以在 Elixir 中使用 Mox 库创建函数 mock，使测试仍能并发运行。Mock 必须基于 behaviour 创建，以确保 mock 的 API 和真实实现不会分歧。
- Ecto 的模块化设计使我们能够将 changeset 测试（验证业务逻辑）与 repository 测试（验证查询和与数据库的交互）分开。在任何情况下，我们都能够并发运行测试。
- Elixir 提供了一个很棒的功能，称为 `doctest`，它使我们能够在编写的文档中嵌入测试。这些嵌入式测试确保文档保持最新，这是在 Elixir 社区中文档享有第一类地位的关键推动因素。
- 集成测试是通过从边缘测试我们的应用程序来完成的。在我们的例子中，边缘是 Web 界面。Phoenix 提供了一组有用的辅助工具，简化了我们在集成测试中需要进行的断言。
- 与集成测试类似，Phoenix 提供了一组用于测试 Channels 的辅助工具。特别是，它公开了不仅允许我们连接到 socket，还可以验证特定事件的广播是否发生，或服务器是否向 Channel 推送了特定事件的函数。
- 对宏测试的关注应放在测试宏展开后的代码上，而不是代码展开本身。通常应避免测试展开本身，因为这可能导致脆弱的测试。
- 基于属性的测试是一种非常不同的测试方法，其中我们不是为每个测试想出示例，而是定义被测试函数所具有的属性，然后让测试框架生成输入数据。然后，框架会检查对于函数提供的每个输入，属性是否都得到尊重。

测试部分完成后，我们的应用程序已准备好部署到实时环境。这正是我们在下一章要做的事情，在那里我们将学习如何将 ElixirDrip 应用程序部署到云端。
