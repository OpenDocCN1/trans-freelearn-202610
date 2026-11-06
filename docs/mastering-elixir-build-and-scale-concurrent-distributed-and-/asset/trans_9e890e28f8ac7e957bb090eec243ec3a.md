
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
