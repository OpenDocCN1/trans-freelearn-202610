
```
iex> raise ArithmeticError, message: "Some weird math going on here"
** (ArithmeticError) Some weird math going on here
```

你可以通过使用 `rescue` 构造来捕获错误（你可以从 `try` 块或整个函数中捕获，与 `def` 配对）。你可以在 rescue 子句上定义模式。你可以使用 `_` 来匹配任何内容。如果没有任何模式匹配，错误将不会被捕获，程序的行为将如同没有 rescue 子句一样：

```
iex> try do
...> 5 / 0
...> rescue
...> e in ArithmeticError -> "Tried to divide by 0."
...> _ -> "None of the above matched"
...> end
"Tried to divide by 0."
```

由于我们并没有对错误做任何处理，只是返回一个字符串，我们可以直接在模式中使用 `ArithmeticError`。只有当你想捕获错误本身时才使用这种语法。当没有任何模式匹配时，我们会在控制台中得到错误：

```
iex> try do
...> 5 / 0
...> rescue
...> ArgumentError -> "ArgumentError was raised."
...> end
** (ArithmeticError) bad argument in arithmetic expression
```

此外，你还可以为 `try/rescue` 块传递 `else` 和/或 `after` 块。`else` 块会在 `try` 主体在未引发任何错误的情况下完成时，对其结果进行匹配。至于 `after` 构造，它将始终被执行，无论是否引发了错误。这通常用于清理某些资源（例如关闭文件描述符）。

![](img/c636f4cfb07f25f1db535995e50706ed_60_0.png)

我们在本节中提到我们不使用异常来控制程序的流程，但事实上 Elixir 为此提供了一个特殊的构造。其语法与前面所示类似，但你使用 `throw` 代替 `raise`，并使用 `catch` 代替 `rescue`。正如官方 *入门指南* 中所述（http://elixir-lang.github.io/getting-started/try-catch-and-rescue.html），这应该在除非使用 throw 和 catch 否则无法检索值的情况下使用。文中还提到在实践中这种情况相当罕见。

## 类型规范和行为

正如本章开头已提到的，Elixir 是一种动态编程语言。因此，我们不声明每个变量的类型，因为它取决于每个变量在每个时刻绑定的值。

通常动态编程语言会带来更高的生产力，因为程序员无需声明类型，可以专注于应用程序的逻辑开发。然而，这是有代价的：在静态类型语言中会在编译时捕获的某些错误，在动态语言中可能只能在运行时捕获。使用动态语言节省的时间随后会被（往往过度地）用于生产环境的调试。

我们并不提倡静态类型语言——毕竟这本书是关于 Elixir 的。但如果可以兼得两者之长呢？

事实证明你可以！类型规范，或 typespecs，是一种用于用相应的类型（参数和返回值）为函数签名做注解的机制。Typespecs 也用于创建自定义数据类型。让我们在深入行为之前更详细地探索它们。

### 类型规范

Typespecs 是使用 `@spec` 模块指令编写的，后跟 `function_name(argument_type) :: return_type`。此模块指令放置在我们要注解的函数定义正前方。

为了演示如何将 typespecs 应用于函数，让我们重新引入我们的 `palindrome?` 函数：

```elixir
$ cat examples/string_helper.ex
defmodule StringHelper do
  def palindrome?(term) do
    String.reverse(term) == term
  end
end
```

鉴于模块名为 `StringHelper`，并且它使用 `String` 模块中的函数，我们可以看到此函数接收一个字符串。由于它使用了 `==` 运算符，并且函数名尾部的 `?` 也暗示了这一点，我们知道此函数返回一个布尔值。有了这些信息，为此函数编写 typespec 很直接：

```elixir
$ cat examples/string_helper_palindrome_with_typespec.ex
defmodule StringHelper do
  @spec palindrome?(String.t) :: boolean
  def palindrome?(term) do
    String.reverse(term) == term
  end
end
```

你也可以使用 @type 指令在模块内定义自己的类型以在 typespecs 中使用（通常是在该类型使用最多的模块中）。假设你在模块中经常使用一个元组，其第一个元素是国家名称，第二个元素是其人口，如 {"Portugal", 10_309_573}。你可以用以下方式为此数据结构创建一个类型：

```
@type country_with_population :: {String.t, integer}
```

然后，你就可以像使用任何其他类型一样在 typespecs 中使用 country_with_population。

在模块中定义类型会将其导出，使其可供所有模块使用。如果你希望类型私有且仅限于定义它的模块，请使用 @typep 指令。与用于分别记录模块和函数的 @moduledoc 和 @doc 指令类似，你可以使用 @typedoc 指令为特定类型提供文档。

![](img/c636f4cfb07f25f1db535995e50706ed_62_0.png)

在 typespecs 中，字符串类型通常由 String.t 表示。你也可以使用 string，但 Elixir 社区不鼓励这样做，目的是避免与表示 Erlang 中字符串的 charlist 类型混淆。如果你在使用 typespecs 时使用 string，编译器会发出警告。

Typespecs 还提供了很好的文档，因为我们可以快速了解此函数接受和返回的类型。此示例仅展示了可用的类型子集——请参阅官方文档以获取完整列表，网址为

https://hexdocs.pm/elixir/typespecs.html.

### Dialyzer

Dialyzer (http://erlang.org/doc/man/dialyzer.html) 是随 Erlang 一起提供的工具，用于对代码进行静态分析。它分析编译后的 .beam 文件，因此适用于所有在 Erlang VM 上运行的编程语言（如 Elixir！）。虽然 Dialyzer 在没有 typespecs 的项目上也很有帮助（例如可以查找冗余代码），但其威力在函数带有 typespecs 注解的项目上最大化。这样，Dialyzer 就能够报告类型错误，使你更接近静态类型语言所能提供的安全性。

尽管本书不会探讨 Dialyzer，我们强烈推荐使用它，因为它非常有帮助。特别是，我们认为 Dialyxir 库（https://github.com/jeremyjh/dialyxir）是将 Dialyzer 集成到 Elixir 项目中的一种很好的方式，因为它抽象掉了直接处理 Dialyzer 的部分复杂性。

### 行为

行为提供了一种描述模块必须实现的一组函数的方式，同时确保模块实现了该集合中的函数。如果你来自面向对象编程背景，可以将行为视为定义接口的抽象基类。在声明行为后，我们可以创建采用此行为的其他模块。行为创建了一个明确的契约，说明采用该行为的模块需要实现什么。这样，我们可以将业务逻辑绑定到抽象（行为）而不是具体实现。我们可以用很少且本地化的更改在应用程序中交换同一行为的两个实现——即在我们定义要使用的行为实现的位置。

让我们用一个例子来演示这个概念。我们将定义一个名为 Presenter 的行为，它只有一个函数：present。然后我们将定义一个采用此行为的模块，称为 CLI.Presenter。我们也可以有其他采用此行为的模块，例如 GraphicalPresenter。行为是使用模块内的 @callback 指令创建的，为该行为包含的每个函数提供 typespec。因此，要定义我们的 Presenter 行为，我们使用以下内容：

```
$ cat examples/presenter.ex
defmodule Presenter do
  @callback present(String.t) :: atom
end
```

现在我们定义将采用此行为的模块：

```
$ cat examples/cli_presenter.ex
defmodule CLI.Presenter do
  @behaviour Presenter

  @impl true
  def present(text) do
    IO.puts(text)
  end
end
```
