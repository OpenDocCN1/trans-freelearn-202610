
我们可以在下一个代码片段中看到该模块的工作情况：

```
iex> CLIPresenter.present("printing text to the command line")
printing text to the command line
:ok
```

从 Elixir 1.5 开始，我们可以使用 @impl 指令来标记作为行为回调而实现的功能。在我们的例子中，我们会在 present 函数上方放置 @impl 指令：

```
$ cat examples/cli_presenter_with_impl_annotation.ex
defmodule CLIPresenter do
  @behaviour Presenter

  @impl true
  def present(text) do
    IO.puts(text)
  end
end
```

我们可以更具体一些，使用 @impl Presenter 来说明该函数是来自 Presenter 行为的回调。这带来了两个主要优势：

- 可读性提高，因为现在明确了哪些函数构成我们 API 的一部分，哪些函数是回调实现。
- 一致性更高，因为 Elixir 编译器会检查你用 @impl 标记的函数是否属于模块所采用的行为的一部分。

请注意，当你在模块上设置 @impl 时，必须将其设置在该模块的所有回调函数上，否则会发出警告。此外，你只能在回调函数上使用 @impl，否则也会发出编译警告。

![](img/c636f4cfb07f25f1db535995e50706ed_64_0.png)

你可以使用 @optional_callbacks 在采用某个行为时将一个或多个函数标记为可选。你需要提供一个关键字列表，其中函数名作为键，元数为值。如果我们希望 Presenter 行为中的 present/1 函数是可选的，我们将使用：

```
$ cat examples/presenter_with_optional_callbacks.ex
defmodule Presenter do
  @callback present(String.t) :: atom
  @optional_callbacks present: 1
end
```

如果我们没有实现 Presenter 行为中声明的所有函数（例如通过注释掉 CLI 的 Presenter.present/1 函数），Elixir 编译器将发出以下警告：

```
warning: undefined behaviour function present/1 (for behaviour Presenter)
```

行为帮助我们遵循开闭原则，因为我们可以扩展系统而无需修改已有内容。如果将来需要一种新的呈现器类型，我们只需创建一个采用 Presenter 行为的新模块，而无需更改行为或其当前实现。我们将在本书中构建的应用程序中使用行为，你将在下一章再次看到它们的使用。

## 协议

在本入门章节中，我们多次提到 Elixir 有协议，其中 Enumerable 就是一个例子。在本节中，我们将深入探讨协议，甚至自己定义一个！

协议与上一节中看到的行为类似，定义了一组必须实现的功能。从这个意义上说，这两种结构都可以用作在 Elixir 中实现多态的一种方式——能够表现出多种行为形式，但都链接到单一接口。虽然行为定义了模块需要实现的一组函数，因此与模块绑定，但协议定义了数据类型必须实现的一组函数。这意味着，使用协议时，我们具有数据类型多态性，并且能够编写根据参数类型而行为不同的函数。

现在让我们看看如何创建一个新协议。我们将采用并扩展官方入门指南（位于 http://elixir-lang.github.io/getting-started/protocols.html）中的示例。我们将定义一个 Size 协议，将由每种数据类型实现。要定义新协议，我们使用 defprotocol 构造：

```
$ cat examples/size.ex
defprotocol Size do
  @doc "Calculates the size of a data structure"
  def size(data)
end
```

我们声明我们的 Size 协议期望实现它的数据类型必须定义一个 size/1 函数，其中参数是我们想要知道其大小的数据结构。

你可以使用 @doc 指令为此函数添加文档，就像你通常对模块内部的命名函数所做的那样。现在我们可以使用 defimpl 构造为我们感兴趣的数据类型定义此协议的实现：

```
$ cat examples/size_implementations_basic_types.ex
defimpl Size, for: BitString do
  def size(string), do: byte_size(string)
end

defimpl Size, for: Map do
  def size(map), do: map_size(map)
end

defimpl Size, for: Tuple do
  def size(tuple), do: tuple_size(tuple)
end
```

![](img/c636f4cfb07f25f1db535995e50706ed_66_0.png)

我们没有为列表定义实现，因为在 Elixir 中，size 通常用于其大小预先计算的数据结构。对于需要按需计算的类型，例如列表，则使用 length 而不是 size。这一点可以通过查看获取列表维度的函数名称进一步观察到：
Kernel.length/1.

在此定义后，我们可以看到我们的协议在实际运行：

```
iex> Size.size("a string")
8
iex> Size.size(%{a: "b", c: "d"})
2
iex> Size.size({1, 2, 3})
3
```

如果我们尝试在未定义实现的类型上使用我们的协议，将会引发错误：

```
iex> Size.size([1, 2, 3, 4])
** (Protocol.UndefinedError) protocol Size not implemented for [1, 2, 3, 4]
```

![](img/c636f4cfb07f25f1db535995e50706ed_66_1.png)

你可以在所有 Elixir 数据类型上为协议定义实现：Atom、BitString、Float、Function、Integer、Tuple、List、Map、PID、Port 和 Reference。请注意，BitString 也用于二进制类型。

为所有类型实现协议可能会很快变得单调乏味。你可以通过为 Any 实现协议来为未实现协议的类型定义回退行为。让我们为我们的 Size 协议这样做：

```
$ cat examples/size_implementation_any.ex
defimpl Size, for: Any do
  def size(_), do: 0
end
```

你必须定义当类型未实现协议时的期望行为。在这种情况下，我们说它的大小为 0（这可能没有意义，因为数据类型可能的大小不同于 0，但让我们忽略这个细节）。

现在我们有两种选择来使用此实现：要么用 @derive [Size] 标记我们想要此回退行为的模块（例如 List 模块），要么在 Size 协议的定义中使用 @fallback_to_any true。前者更费力，因为你必须为每个想要假设 Any 行为的模块进行注解，而后者更简单，因为只需更改协议定义即可使其适用于所有数据类型。在 Elixir 社区中，通常更偏好显式性，因此在 Elixir 项目中你更有可能看到 @derive 方法。

虽然为 Elixir 的数据类型实现协议已经开启了无限可能，但只有将它们与结构体混合使用时，我们才能充分利用 Elixir 的可扩展性。我们尚未讨论结构体，因此将在下一节中介绍它们。

## 结构体

结构体是构建在映射之上的抽象。我们在模块内部使用 defstruct 构造来定义结构体。结构体的名称是定义它的模块的名称（这意味着每个模块只能定义一个结构体）。对于 defstruct，我们传递一个关键字列表，其中包含定义结构体具有的字段的键值对以及它们的默认值。让我们定义一个 Folder 结构体：

```
$ cat examples/folder.ex
defmodule Folder do
  defstruct name: "new folder", files_info: [], path: nil
end
```

现在我们可以在 IEx 会话中使用它：

```
iex> %Folder{}
%Folder{files_info: [], name: "new folder", path: nil}
iex> %Folder{}.name
"new folder"
iex> %Folder{}.files_info
[]
```

Elixir 已有 File 模块，提供多个处理文件的函数。其中之一是 File.stat/2，它返回一个包含所提供路径信息的 %File.Stat{} 结构体。我们 %Folder{} 结构体中的 files_info 字段是一个列表，将包含 %File.Stat{} 结构体作为元素。让我们用一个文件初始化一个文件夹：

```
iex> folder = %Folder{files_info: [File.stat!("string_helper.ex")]}
%Folder{files_info: [%File.Stat{access: :read_write,
  atime: {{2017, 12, 31}, {16, 58, 56}}, ctime: {{2017, 12, 30}, {3, 40, 29}},
  gid: 100, inode: 3290229, links: 1, major_device: 65024, minor_device: 0,
  mode: 33188, mtime: {{2017, 12, 30}, {3, 40, 29}}, size: 509, type: :regular,
  uid: 1000}], name: "new folder", path: nil}
```

请注意，此示例假定你在启动 iex 的目录中有 "string_helper.ex" 文件。另请注意，我们使用的是 File.stat!，它与 File.stat 类似，但不是返回 {:ok, result} 元组，而是直接返回结果本身。

现在我们有一个包含一个文件的文件夹 %Folder{} 结构体。现在我们可以向你展示更新结构体的语法，它与在映射中使用的语法类似（或者你可以使用 Map 模块中的函数）。假设你在当前工作目录中也有 "recursion.ex" 文件，你可以使用此语法来更新结构体：

```
iex> folder = %Folder{ folder | files_info: [File.stat!("recursion.ex") | folder.files_info]}
%Folder{files_info: [%File.Stat{access: :read_write,
  atime: {{2017, 12, 30}, {20, 8, 29}}, ctime: {{2017, 12, 30}, {20, 8, 25}},
  gid: 100, inode: 3278529, links: 1, major_device: 65024, minor_device: 0,
  mode: 33188, mtime: {{2017, 12, 30}, {20, 8, 25}}, size: 270, type: :regular,
  uid: 1000},
  %File.Stat{access: :read_write, atime: {{2017, 12, 31}, {16, 58, 56}},
  ctime: {{2017, 12, 30}, {3, 40, 29}}, gid: 100, inode: 3290229, links: 1,
  major_device: 65024, minor_device: 0, mode: 33188,
  mtime: {{2017, 12, 30}, {3, 40, 29}}, size: 509, type: :regular, uid: 1000}],
name: "new folder", path: nil}
iex> folder.files_info
```
