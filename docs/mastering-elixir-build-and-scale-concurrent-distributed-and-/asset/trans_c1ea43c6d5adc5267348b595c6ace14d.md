
"#{module}.#{function}/#{length(args)}"
end
end

让我们在 v3 宏迭代中使用 pretty_signature/3 函数。这次，我们打印由我们的 quote 块生成的 ast_to_return，以查看传递给创建函数全名的 pretty_signature/3 函数的值：

```
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  defmacro defchrono_v3(function_definition, do: body) do
    {function, args} = Macro.decompose_call(function_definition)
    IO.puts "On defchrono_v3"
    IO.inspect function_definition
    IO.puts "Function"
    IO.inspect function
    IO.puts "Arguments"
    IO.inspect args

    ast_to_return = quote do
      def unquote(function_definition) do
        signature =
          Chronometer.pretty_signature(__MODULE__, unquote(function),
          unquote(args))

        Chronometer.run_and_measure(signature, fn -> unquote(body) end)
      end
    end

    IO.puts "Returning"
    ast_to_return
    |> Macro.to_string()
    |> IO.puts()

    ast_to_return
  end
end
```

如果我们现在编译 MeasuredModule 并再次运行其 slow_times/2 函数，我们对被测量的函数没有任何疑问：

```
iex> c "./examples/measured_module.exs"
On defchrono_v3
{:slow_times, [line: 4], [{:x, [line: 4], nil}, {:y, [line: 4], nil}]}
Function
:slow_times
Arguments
[{:x, [line: 4], nil}, {:y, [line: 4], nil}]
Returning
def(slow_times(x, y)) do
  signature = Chronometer.pretty_signature(__MODULE__, :slow_times, [x, y])
  Chronometer.run_and_measure(signature, fn ->
    Process.sleep(2000)
    x * y
  end)
end
[MeasuredModule]
```

```
iex> MeasuredModule.slow_times(8, 9)
Took 2001016 µs to run MeasuredModule.slow_times/2
72
```

你可能已经注意到，我们对传递给 pretty_signature/3 函数的 function 和 args 值调用了 unquote/1。相比之下，我们没有对用于获取被测量函数模块的 __MODULE__/0 调用执行此操作。这是有道理的，因为 __MODULE__/0 实际上是一个宏，我们希望它在调用者上下文中展开。这就是为什么我们在 defchrono_v3 宏返回的 AST 的文本表示中看到 __MODULE__ 表达式的原因。如果我们改为使用 pretty_signature(unquote(__MODULE__), ..., ...)，__MODULE__ 表达式将在宏上下文中被求值，导致 ElixirDrip.Chronometer。

v3 迭代产生我们期望的结果，一切看起来都很好。现在让我们尝试将我们的宏应用于定义了默认参数的 slow_square/1 函数：

```
$ cat examples/measured_module.exs
defmodule MeasuredModule do
  import ElixirDrip.Chronometer
  # ...
  defchrono_v3 slow_square(x \\ 3) do
    Process.sleep(2000)
    x * x
  end
end
```

从这段代码可以看出，如果我们再次编译 MeasuredModule，现在会收到一个错误，提示没有 \\/2 函数：

```
iex> c "./examples/measured_module.exs"
On defchrono_v3
{:slow_square, [line: 4], [{:\\, [line: 4], [{:x, [line: 4], nil}, 3]}]}
Function
:slow_square
Arguments
[{:\\, [line: 4], [{:x, [line: 4], nil}, 3]}]
Returning
def(slow_square(x \\ 3)) do
  signature = Chronometer.pretty_signature(__MODULE__, :slow_square,
    [x \\ 3])
  Chronometer.run_and_measure(signature, fn ->
    Process.sleep(2000)
    x * x
  end)
end
```

```
== Compilation error in file examples/measured_module.exs ==
** (CompileError) examples/measured_module.exs:4: undefined function \\/2
  (stdlib) lists.erl:1338: :lists.foreach/2
  (stdlib) erl_eval.erl:670: :erl_eval.do_apply/6
**
```

为什么编译器会抱怨奇怪的 \\/2 函数？由于我们使用了默认参数，args 值仍然是一个列表，但现在包含一个单个 :\\ 元组，其参数（元组的第三个元素）是 x 变量和 3 默认值。这完全合理，我们实际上可以使用老式的 quote 来重现它：

```
iex> quote do: x \\ 3
{:\\, [], [{:x, [], Elixir}, 3]}

iex> quote do: [x, y]
[{:x, [], Elixir}, {:y, [], Elixir}]
```

因为我们对将传递给 pretty_signature/3 函数的 args 值应用了 unquote/1，所以我们将 [x \\ 3] 的引用表示注入到 Chronometer.pretty_signature(...) 语句的第三个参数中。当编译器尝试编译此表达式时，它会评估 x \\ 3 部分并中断，因为没有 \\/2 函数。

我们现在知道问题所在了。让我们通过在宏上下文中直接计算 arity 并将其直接传递给更新后的 pretty_signature/3 函数来修复它，该函数现在接收 arity 值而不是 args 列表作为最后一个参数：

```
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  defmacro defchrono_v4(function_definition, do: body) do
    {function, args} = Macro.decompose_call(function_definition)
    arity = length(args)
    IO.puts "On defchrono_v4"
    IO.inspect function_definition
```
```elixir
IO.puts "Function"
IO.inspect function
IO.puts "Arguments"
IO.inspect args

ast_to_return = quote do
  def unquote(function_definition) do
    signature =
      Chronometer.pretty_signature(__MODULE__, unquote(function),
      unquote(arity))

    Chronometer.run_and_measure(signature, fn -> unquote(body) end)
  end
end

IO.puts "Returning"
ast_to_return
|> Macro.to_string()
|> IO.puts()

ast_to_return
end

def pretty_signature(module, function, arity) do
  module = module
    |> Atom.to_string()
    |> String.replace_leading("Elixir.", "")

  "#{module}.#{function}/#{arity}"
end
end
```

即使使用默认参数，它现在也能工作，因为我们在 advance 计算了 arity 值：

```
```elixir
iex> c "./examples/measured_module.exs"
On defchrono_v4
{:slow_square, [line: 4], [{:\\, [line: 4], [{:x, [line: 4], nil}, 3]}]}
Function
:slow_square
Arguments
[{:\\, [line: 4], [{:x, [line: 4], nil}, 3]}]
Returning
def(slow_square(x \\ 3)) do
  signature = Chronometer.pretty_signature(__MODULE__, :slow_square, 1)
  Chronometer.run_and_measure(signature, fn ->
    Process.sleep(2000)
```
```
    x * x
    end)
end
[MeasuredModule]

iex> MeasuredModule.slow_square()
Took 2000981 µs to run MeasuredModule.slow_square/1
9

iex> MeasuredModule.slow_square(5)
Took 2000324 µs to run MeasuredModule.slow_square/1
25
```

这提供了一个在使用宏时应遵循的宝贵教训。在宏上下文中尽可能多地完成工作，以将尽可能小的代码块注入调用者上下文。并且当你的宏接收参数时，尝试思考这些参数可能被传递的每一种方式，以应对所有边缘情况。使用你宏的人可能不如你精通元编程，而使用宏产生的错误有时是晦涩的，与我们从普通 Elixir 中期望并欣赏的详细错误信息形成对比。这就是为什么你在使用宏时应加倍谨慎的原因之一。

我们的 defchrono_v4 宏现在按预期工作。让我们再进一步，使用 quote 块上的 bind_quoted 选项来实现这一点。这是我们的 defchrono 宏中最棘手的部分，因为宏被评估的时间点不同。

在此次新的宏迭代中，我们将首次使用 Macro.escape/1 函数。让我们稍作 digression 来了解它是如何工作的。你可以将此函数视为 quote/1 宏的表亲，因为它也返回一个引用表达式。然而，它并不是总是返回接收到的代码的引用表示，而是先评估接收到的代码，然后再返回其引用表示。查看以下片段以说明这种差异：

```
iex> a = %{this: "is", a: "map"}
%{a: "map", this: "is"}

iex> quoted_a = quote do: a
{:a, [], Elixir}

iex> escaped_a = Macro.escape a
{:%{}, [], [a: "map", this: "is"]}
```

我们现在将分析 defchrono_v5 宏如何与 bind_quoted 选项一起工作。正如我们已经看到的，bind_quoted 选项让你可以在 quote 块上访问由它定义变量，因为它为我们定义了与 bind_quoted 关键字列表中条目一样多的赋值。其使用还默认禁用了 unquote/1，因此那些 unquote/1 调用（针对 function_definition、function、arity 和 body）将以其 AST 形式出现在宏返回的引用表示上：

```
```elixir
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  defmacro defchrono_v5(function_definition, do: body) do
    {function, args} = Macro.decompose_call(function_definition)
    arity = length(args)
    IO.puts "On defchrono_v5"
    IO.inspect function_definition
    IO.puts "Escaped function_definition"
    IO.inspect Macro.escape(function_definition)
    IO.puts "Function"
    IO.inspect function
    IO.puts "Arguments"
    IO.inspect args

    ast_to_return = quote bind_quoted: [
      function_definition: Macro.escape(function_definition),
      body: Macro.escape(body),
      function: function,
      arity: arity
    ] do
      def unquote(function_definition) do
        signature =
          Chronometer.pretty_signature(__MODULE__, unquote(function),
          unquote(arity))

        Chronometer.run_and_measure(signature, fn -> unquote(body) end)
      end
    end

    IO.puts "Returning"
    ast_to_return
    |> Macro.to_string()
    |> IO.puts()

    ast_to_return
  end
end
```

