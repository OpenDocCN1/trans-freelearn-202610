
由于 unquote/1 调用现在被禁用，它们将被注入到调用者上下文中，其求值将被推迟，而不是在编译时求值。当它们最终被求值时，预计每个被反引用的变量都已包含以其引用形式存在的代码，准备好被注入到最终的 AST 中。这就是我们对 function_definition 和 body 参数使用 Macro.escape/1 的原因，根据定义，它们已经是引用的（如果你还记得的话，两者都是宏参数）。

这就是使用 defchrono_v5 宏编译 MeasuredModule 时我们看到的内容：

```
iex> c "./examples/measured_module.exs"
On defchrono_v5
{:slow_square, [line: 4], [{:\\, [line: 4], [{:x, [line: 4], nil}, 3]}]}
Escaped function_definition
{:(), []},
[
  :slow_square,
  [line: 4],
  [{:{}, [], [:\\\, [line: 4], [{:{}, [], [:x, [line: 4], nil]}, 3]]}]
]
Function
:slow_square
Arguments
[{:\\, [line: 4], [{:x, [line: 4], nil}, 3]}]
Returning
(
function_definition = {:slow_square, [line: 4], [{:\\, [line: 4],
  [{:x, [line: 4], nil}, 3]}]}
body = {:__block__, [], [{:., [line: 5], [{:__aliases__, [line: 5],
  [:Process]}, :sleep], [2000]}, {:*, [line: 6], [{:x,
  [line: 6], nil}, {:x, [line: 6], nil}]}]}
function = :slow_square
arity = 1

def(unquote(function_definition)) do
  signature = Chronometer.pretty_signature(__MODULE__,
    unquote(function), unquote(arity))
  Chronometer.run_and_measure(signature, fn -> unquote(body) end)
end
)
[MeasuredModule]

iex> MeasuredModule.slow_square(9)
Took 2009285 µs to run MeasuredModule.slow_square/1
81
```

注意我们在 def/2 调用之前注入到调用者模块中的四个赋值，每个 bind_quoted 变量一个。因为我们在通过 bind_quoted 选项将其注入到 quote 块之前对 function_definition 进行了转义，所以我们可以在 function_definition 赋值上看到其预期的引用表示：

```
# ...
Returning
(
    function_definition = {:slow_square, [line: 4], [{:\\, [line: 4], [{:x, [line: 4], nil}, 3]}]}
    # ...
```

如果我们忘记转义 function_definition 变量，我们将看到以下赋值（在因 slow_square/1 函数未定义而导致编译错误之前）：

```
# ...
Returning
(
    function_definition = slow_square(x \\ 3)
    # ...

== Compilation error in file examples/measured_module.exs ==
** (CompileError) examples/measured_module.exs:4: undefined function
slow_square/1
    (elixir_drip) expanding macro: ElixirDrip.Chronometer.defchrono_v5/2
    examples/measured_module.exs:4: MeasuredModule (module)
** (CompileError) compile error
    (iex) lib/iex/helpers.ex:183: IEx.Helpers.c/2
```

这个错误完全合理，因为如果不转义 function_definition 变量，该表达式将作为普通表达式而不是引用表达式到达调用者模块，编译器会尝试像往常一样对其求值。由于此时不存在 slow_square/1 函数，它会使用我们刚刚看到的错误停止编译。

你还可以看到我们对 function_definition 和 body 变量都使用了 Macro.escape/1，但没有对 function 和 arity 变量使用。这是因为后两个变量都是字面量，因为它们始终是一个原子和一个整数。由于无论是否被引用，它们始终以相同的方式表示，我们决定不应用 Macro.escape/1 函数。

现在让我们专注于 `defchrono_v5` 宏生成的代码。我们将创建一个 `MeasuredModuleExpanded`，它*手动*完成宏自动为我们完成的工作。如你所见，我们使用有效的引用表示定义了 `function_definition` 和 `body` 变量（每个元组的元数据部分被设置为 `[]` 以减少噪音）：

```
$ cat examples/measured_module_expanded.exs
defmodule MeasuredModuleExpanded do
  alias ElixirDrip.Chronometer

  function_definition = {:slow_square, [], [{:\\, [], [{:x, [], nil}, 3]}]}
  body = {:__block__, [], [{:., [], [{:__aliases__, [], [:Process]}, :sleep], [2000]}, {:*, [], [{:x, [], nil}, {:x, [], nil}]}]}
  function = :slow_square
  arity = 1

  def(unquote(function_definition)) do
    signature =
      Chronometer.pretty_signature(__MODULE__, unquote(function), unquote(arity))
    Chronometer.run_and_measure(signature, fn -> unquote(body) end)
  end
end
```

你可能会觉得奇怪，看到几个没有封闭 `quote` 块的 `unquote/1` 调用，鉴于我们之前说过 `unquote/1` 宏只能在 `quote` 块内部使用。但如果你仔细观察，你会看到两个隐藏的 quote 块。

我们使用 `def/2` 宏像往常一样定义函数。由于它是一个宏，其参数始终通过 `quote/1` 转换为其引用表示。因此，我们能够在 `def/2` 宏的第一个和第二个参数内部执行 `unquote(function_definition)` 和 `unquote(function|arity|body)`，分别是因为这些 `unquote/1` 调用发生在每个 `def/2` 宏参数的 `quote` 块内部。

编译 `MeasuredModuleExpanded` 后，我们能够调用*手动*定义的 `slow_square/1` 函数：

```
iex> c "./examples/measured_module_expanded.exs"
[MeasuredModuleExpanded]

iex> MeasuredModuleExpanded.slow_square 5
Took 2000906 µs to run MeasuredModuleExpanded.slow_square/1
25
```

```
iex> MeasuredModuleExpanded.slow_square
Took 2000317 µs to run MeasuredModuleExpanded.slow_square/1
9
```

到目前为止我们涵盖了很多新的概念和技术，一开始可能会让人望而生畏。但请放心，无论我们对宏做什么，唯一发生的“魔法”就是我们将代码注入到调用者模块中。注入的代码需要采用特定的引用格式，仅此而已。

## 使用 use 和 __using__/1 宏

我们的目标现在是选择用于计时函数的时间单位。为此我们将使用 use 宏，你可能已经多次看到过它（我们之前实现的 CacheWorker 模块使用了 GenServer）。我们现在不再导入 ElixirDrip.Chronometer 模块，而是使用它：

```
$ cat examples/measured_module.exs
defmodule MeasuredModule do
  use ElixirDrip.Chronometer, unit: :secs

  defchrono slow_times(x, y) do
    Process.sleep(2000)
    x * y
  end

  defchrono slow_square(x \\ 3) do
    Process.sleep(2000)
    x * x
  end
end
```

当编译器在 MeasuredModule 上找到 use 语句时，它将运行 ElixirDrip.Chronometer 模块的 __using__/1 宏。你会经常看到这种策略，因为它为被使用的模块提供了一种标准调用方式，并且大多数时候会将所需内容注入到调用者模块中。use 的另一个非常有用的特性是它允许我们向被调用的 __using__/1 宏传递参数。在前面的示例中，我们传递了一个包含单个 :unit 条目的关键字列表。

让我们分析 Chronometer.__using__/1 宏为我们做了什么。鉴于我们将 [unit: :secs] 关键字列表传递给宏（请注意 use Chronometer, unit: :secs 语句），我们能够将 :secs 原子存储在 time_unit 变量上。由于 __using__/1 是一个宏，它需要遵守所有宏规则；即，它需要返回一个引用表达式。

在注入到调用者模块的引用块中，我们导入 ElixirDrip.Chronometer 模块，然后注册一个 @time_unit 模块属性来存储所选的时间单位。请注意，如果在 use 语句中未作为 :unit 传递时间单位，@time_unit 将为 nil：

```
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  defmacro __using__(options) do
    time_unit = options[:unit]

    quote do
      import unquote(__MODULE__)

      Module.register_attribute(__MODULE__, :time_unit, [])
      @time_unit unquote(time_unit)
    end
  end

  defmacro defchrono(function_definition, do: body) do
    {function, args} = Macro.decompose_call(function_definition)
    arity = length(args)

    quote bind_quoted: [
      function_definition: Macro.escape(function_definition),
      body: Macro.escape(body),
      function: function,
      arity: arity
    ] do
      def unquote(function_definition) do
        signature =
          pretty_signature(__MODULE__, unquote(function),
            unquote(arity))

        run_and_measure(@time_unit, signature, fn -> unquote(body) end)
      end
    end
    # ...
  end
end
```
