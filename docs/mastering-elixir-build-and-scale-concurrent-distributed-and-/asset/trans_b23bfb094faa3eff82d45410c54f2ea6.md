
一般来说，由宏生成的代码调试在很多情况下更棘手。而且，正如 Kernighan 所说，*调试的难度是最初编写代码的两倍*，因此你应该绝对谨慎使用宏。

尽管宏有造成混乱的潜力，但在像 Elixir 这样的现代语言中，宏仍有其用武之地，并且能让你解决否则会非常棘手或极其繁琐的问题。许多 Elixir 库使用宏来降低在其他项目中使用它们的摩擦。例如，通过导入 `Ecto` 库的 `Ecto.Query` 模块（我们将在下一章讨论这个问题），你可以创建如下查询：`query = from m in Media, where: m.file_size > 500, select: m`。这是一个有效的 Elixir 表达式，说明了宏能为你的代码带来的表达力。

一条非常明智的拉丁谚语可以适用于宏，正如它也可以适用于生活：*abusus non tollit usum*，通常被翻译为 *滥用并不会消除其用途*。尽管宏可能导致不必要的复杂性，但它们是方便的工具，在正确使用时可以让你写出更好的代码。而且，即使有时宏没有被正确使用，它们也可能是解决你问题的恰好所需。

关于元编程，取得正确的平衡是关键。现在让我们应用我们所学的内容来创建一个宏，该宏将允许我们测量函数执行所需的时间。

## 扩展模块

在理解了宏的工作原理之后，我们现在将应用刚刚获得的见解，看看如何创建向调用者模块添加功能的宏。我们此处的目标是创建一个宏，`ElixirDrip.Chronometer.defchrono/2`，它等同于现有的 `def/2` 宏，但额外具有记录函数调用花费时间的特性。

让我们先看看最终结果。我们希望能够定义类似 `defchrono fun_name(arg1, arg2), do: ...` 的函数，并且当我们调用 `fun_name/2` 函数时，它会告诉我们 `Took 123 µs to run SomeModule.fun_name/2`。下面的 `MeasuredModule` 将是我们的研究对象：

```
$ cat examples/measured_module.exs
defmodule MeasuredModule do
  import ElixirDrip.Chronometer

  defchrono_vn slow_times(x, y) do
    Process.sleep(2000)
    x * y
  end
end
```

在之前的片段中，我们为宏名称使用 _vn 后缀，因为我们正在开发的 `defchrono` 宏将有多个迭代，从 v0 开始到 v5 结束。每次引入新迭代时，先前的 `MeasuredModule` 将被更新以使用新的 `defchrono` 宏版本。

我们 `defchrono` 宏的第一个迭代的唯一目的是分析传递给它的参数。由于 `function_definition` 和 `body` 参数已经被引用（就像其他任何宏参数一样），我们只需使用 `unquote/1` 将它们的值注入宏返回的 `quote` 块中即可：

```elixir
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do

  defmacro defchrono_v0(function_definition, do: body) do
    IO.puts "On defchrono_v0, function definition:"
    IO.inspect function_definition
    IO.puts "Body:"
    IO.inspect body

    quote do
      def unquote(function_definition) do
        unquote(body)
      end
    end
  end
end
```

让我们在 IEx shell 中尝试一下，以便观察宏参数的值：

```
iex> c "./examples/measured_module.exs"
On defchrono_v0, function definition:
{:slow_times, [line: 4], [{:x, [line: 4], nil}, {:y, [line: 4], nil}]}
Body:
{:__block__, [],
 [
  {{:., [line: 5], [{:__aliases__, [line: 5], [:Process]}, :sleep]},
   [line: 5],
   [2000]},
  {:*, [line: 6], [{:x, [line: 6], nil}, {:y, [line: 6], nil}]}
 ]}
[MeasuredModule]

iex> MeasuredModule.slow_times(2, 3)
6
```

根据我们在这里看到的内容，函数定义是一个普通的三个元素元组，其中最后一个元素是包含函数参数的列表，而函数体则由一个 __block__ 元组组成，其最后一个元素是包含两个语句的引用表示的列表，所以这里也没有什么新东西。

我们能够调用 slow_times/2 函数是因为我们的宏返回的本身是一个 def 宏，其函数定义和函数体与我们作为参数传递的相同。现在的下一步是实际测量函数执行所需的时间。为此，我们将使用 Erlang 函数 :timer.tc/1，它接收一个 lambda 函数并执行它，返回花费的时间（以微秒为单位）和 lambda 结果。

我们可以在宏返回的引用块中直接执行此操作，但最好在 Chronometer 模块中单独创建一个函数来执行。这样更容易测试它，并且我们减少了注入到调用者上下文的代码。让我们创建 Chronometer.run_and_measure/1 函数：

```
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  def run_and_measure(to_measure) do
    {time_us, result} = :timer.tc(to_measure)
    IO.puts "Run in #{time_us} µs"

    result
  end
end
```

这样，我们甚至无需使用任何宏就可以使用此函数。如果我们调用 result = Chronometer.run_and_measure(fn -> Process.sleep(1000) end)，我们将看到 Run in 1000961 µs，并且 result 将获得 Process.sleep/1 调用返回的 :ok 原子。

现在让我们在下一个宏迭代 defchrono_v1 上使用 run_and_measure/1：

```
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  defmacro defchrono_v1(function_definition, do: body) do
    IO.puts "On defchrono_v1, function definition:"
    IO.inspect function_definition

    quote do
      def unquote(function_definition) do
        Chronometer.run_and_measure(fn -> unquote(body) end)
      end
    end
  end
end
```

如果我们现在调用 MeasuredModule.slow_times/2 函数，我们知道它需要两秒。但如果我们使用宏来测量多个函数，仅通过查看日志就很难弄清楚哪个测量时间对应哪个函数：

```
iex> c "./examples/measured_module.exs"
On defchrono_v1, function definition:
{:slow_times, [line: 4], [{:x, [line: 4], nil}, {:y, [line: 4], nil}]}
[MeasuredModule]

iex> MeasuredModule.slow_times(4, 5)
Run in 2000621 µs
20
```

为了解决之前的问题，让我们创建一个 v2 宏，除了实际花费的时间外，还记录函数名称。查看前一个代码块中的 IEx 输出，你可以看到在编译 MeasuredModule 时，我们打印了函数定义。这个函数定义是一个预期的三元素元组，其中函数名位于第一个元素，函数参数列表位于最后一个元素。我们可以对这个元组进行模式匹配以提取函数名和参数，但已经有 Macro.decompose_call/1 函数可以为我们完成此操作。下面是 defchrono_v2 宏现在的样子：

```
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  defmacro defchrono_v2(function_definition, do: body) do
    {function, args} = Macro.decompose_call(function_definition)
    IO.puts "On defchrono_v2, function definition:"
    IO.inspect function_definition
    IO.puts "Function"
    IO.inspect function
    IO.puts "Arguments"
    IO.inspect args

    quote do
      def unquote(function_definition) do
        Chronometer.run_and_measure(unquote(function), fn ->
          unquote(body) end)
      end
    end
  end
end
```

对于之前的迭代，我们创建了一个 run_and_measure/2 函数，除了要运行的 lambda 之外，它还接收函数名作为参数：

```
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  def run_and_measure(to_run, to_measure) do
    {time_µs, result} = :timer.tc(to_measure)
    IO.puts "Took #{time_µs} µs to run #{to_run}"

    result
  end
end
```

通过在 MeasuredModule 上使用 defchrono_v2 宏，我们现在得到了更漂亮的结果。也可以观察到应用 Macro.decompose_call/1 函数的结果：

```
iex> c "./examples/measured_module.exs"
On defchrono_v2, function definition:
{:slow_times, [line: 4], [{:x, [line: 4], nil}, {:y, [line: 4], nil}]}
Function
:slow_times
Arguments
[{:x, [line: 4], nil}, {:y, [line: 4], nil}]
[MeasuredModule]

iex> MeasuredModule.slow_times(6, 7)
Took 2000960 µs to run slow_times
42
```

但如果我们有许多 slow_times 函数，具有不同的元数或它们位于不同的模块中呢？我们宏的当前输出只显示函数名，因此如果有多个 slow_times 函数，仅通过查看宏输出就无法知道哪个函数已运行。让我们创建一个 Chronometer.pretty_signature/3 函数，它接收模块、函数名和参数列表，并返回格式为 Module.function/arity 的字符串：

```
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  def pretty_signature(module, function, args) when is_list(args) do
    module = module
              |> Atom.to_string()
              |> String.replace_leading("Elixir.", "")
```
