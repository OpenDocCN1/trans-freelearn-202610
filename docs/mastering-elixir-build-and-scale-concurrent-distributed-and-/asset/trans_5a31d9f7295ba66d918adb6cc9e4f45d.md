
现在让我们定义一个 Demo 模块，它将包含我们的第一个 simple/1 宏：

```
iex> defmodule Demo do
...>   defmacro simple(param) do
...>     IO.puts "Inside the Demo.simple/1 macro"
...>     IO.inspect param
...>
...>     result = quote do
...>       IO.puts "This will be injected into the caller context"
...>       :os.system_time(:seconds)
...>     end
...>
...>     IO.puts "Demo.simple/1 result"
...>     IO.inspect result
...>
...>     result
...>   end
...> end
{:module, Demo, <<70, 79, ...>>, {:simple, 1}}
```

在前面的 Demo 模块中，我们记录了传递给 simple/1 宏的 param，并在最后在返回之前也记录了宏的结果。我们的宏返回一个由对 IO.puts/1 的调用和对 :os.system_time/1 的第二次调用组成的块。我们将在前面的 Playground.do_something/0 函数中调用它。宏调用将在扩展时立即被给定的代码块替换在 Playground 模块的语法树上。

通过评估 Playground 模块，我们的宏也会在 Demo.simple/1 宏扩展时被评估。这就是为什么在交互式 shell 上定义模块后，我们会立即看到为 param 和最终宏结果变量生成的由 IO.inspect/1 调用产生的输出的原因：

```
iex> defmodule Playground do
...> import Demo
...>
...> def do_something do
...>   IO.puts "Inside the Playground.do_something/0 function"
...>
...>   simple(5 == (25/5))
...> end
...> end
```

```
Inside the Demo.simple/1 macro
{:==, [line: 9], [5, {:/, [line: 9], [25, 5]}]}

Demo.simple/1 result
```

```
{:__block__, [],
[
  {{:., [], [{:__aliases__, [alias: false], [:IO]}, :puts]}, [],
   ["This will be injected into the caller context"]},
  {{:., [], [:os, :system_time]}, [], [:seconds]}
]}

{:module, Playground, <<70, 79, ...>>, {:do_something, 0}}
```

在前面的示例中，我们可以看到传递参数的引用表示以及 simple/1 宏返回的结果。正如我们所预期的那样，param 变量包含 5 == 25/5 的引用表达式，我们的宏的结果由一个代码块组成（请看 __block__ 元组），其中包含两个条目：一个用于 IO.puts/1 调用，另一个用于 :os.system_time/1 调用。

一切看起来都不错，但让我们真正确保这两个引用表达式代表我们期望的代码。为此，我们将使用 `Macro.to_string/1` 函数，它接收一个引用表达式并返回等效的 Elixir 代码：

```
iex> IO.puts Macro.to_string({:==, [line: 9], [5, {:/, [line: 9], [25, 5]}]})
5 == 25 / 5
:ok
```

```
iex> IO.puts Macro.to_string({:__block__, [],
...> [
...>   {{:., [], [{:__aliases__, [alias: false], [:IO]}, :puts]}, [],
...>    ["This will be injected into the caller context"]},
...>   {{:., [], [:os, :system_time]}, [], [:seconds]}
...> ]})
(
  IO.puts("This will be injected into the caller context")
  :os.system_time(:seconds)
)
:ok
```

如我们所见，两个引用表达式都返回了我们期望的 Elixir 代码。我们现在可以在确信世界仍然有意义的情况下继续探索宏的工作原理。

![](img/c636f4cfb07f25f1db535995e50706ed_236_0.png)

你上面看到的 :ok 原子，每个对应一次 IO.puts Macro.to_string ... 调用，是执行 IO.puts/1 的结果。我们使用它是因为 `Macro.to_string/1` 返回结果 Elixir 代码的漂亮字符串，带缩进和换行符。

现在让我们了解控制我们的宏代码如何访问其所在外部上下文的规则。

## 卫生访问

每当我们处于一个 quote 块内部时，Elixir 会为我们生成该代码块的引用表示。如果在该过程中找到一个 inexisting_var 变量，Elixir 很乐意在代码块的引用表示上返回其引用表示，{:inexisting_var, [], Elixir}。然而，如果我们将此引用表达式注入到调用者模块中，我们会得到一个错误，因为 inexisting_var 在生成的代码将运行的上下文中根本不存在。

![](img/c636f4cfb07f25f1db535995e50706ed_237_0.png)

即使变量 inexisting_var 在调用者上下文中定义，宏也无法访问它，因为我们没有对 inexisting_var 使用 var!/1 包装器。如果我们尝试编译 Playground 模块，我们会收到一个警告，提示变量 "inexisting_var" 不存在并且正在扩展为 "inexisting_var()"，最后我们会收到一个 CompileError，提示未定义函数 inexisting_var/0。

如果我们在 Demo.simple/1 宏返回的引用表达式上添加一个 IO.puts inexisting_var 调用，我们会在 __block__ 元组中看到它（就在编译失败之前）：

```
# Demo.simple/1 result doing `IO.puts inexisting_var`
{:__block__, [],
[
  {{:., [], [{:__aliases__, [alias: false], [:IO]}, :puts]}, [],
   [{:inexisting_var, [], Demo}]},
  {{:., [], [{:__aliases__, [alias: false], [:IO]}, :puts]}, [],
   ["This will be injected into the caller context"]},
  {{:., [], [:os, :system_time]}, [], [:seconds]}
]}
```

将之前的 __block__ 元组与下一个进行比较，在下一个中我们改为执行 IO.puts var!(inexisting_var)。我们可以看到 var!/1 的使用，因此我们能够在外部上下文中访问 inexisting_var，在本例中是调用者的上下文。这只有在调用者最初确实定义了 inexisting_var 变量时才有效：

```
# Demo.simple/1 result doing `IO.puts var!(inexisting_var)`
{:__block__, [],
[
  {{:., [], [{:__aliases__, [alias: false], [:IO]}, :puts]}, [],
   [{:var!, [context: Demo, import: Kernel], [{:inexisting_var, [], Demo}]}]},
  {{:., [], [{:__aliases__, [alias: false], [:IO]}, :puts]}, [],
   ["This will be injected into the caller context"]},
  {{:., [], [:os, :system_time]}, [], [:seconds]}
]}
```

我们说宏返回的引用表达式在调用者上下文中运行，因为它被注入到调用者代码自身的引用表示中。最终，宏的引用表示与调用者的所有其他代码无法区分，并在同一时间执行。

你宏中位于返回的 quote 块之外的所有语句，都在 Elixir 编译器在代码中找到你的宏被使用并因此扩展它时进行评估。当这些语句运行时，它们在宏代码被评估时运行，因此我们说它们在宏上下文中运行。

如果你考虑我们之前定义的 Demo.simple/1 宏，语句 IO.puts "This will be injected into the caller context" 在调用者上下文中执行，而语句 IO.puts "Inside the Demo.simple/1 macro" 在宏上下文中执行。

即使你在宏返回的引用表达式内部定义一个变量，引用表达式被注入的位置也无法访问它，因为在 Elixir 中宏是卫生的。我们在研究 quote/2 的工作原理时已经看到这一点，这里行为相同：宏 quote 块内部定义或操作的一个变量只有在被包裹在 var!/1 函数调用中时才能影响调用者上下文。

你也可以向 var!/2 函数传递第二个参数，指示给定变量应存在于哪个上下文中。这在你想在宏引用块内部定义一个变量但又不想污染调用者上下文时很有用。请考虑以下模块：

```
iex> defmodule CleanAccess do
...>   defmacro start_counter(counter) do
...>     quote do
...>       var!(counter_var, CleanAccess) = unquote(counter)
...>     end
...>   end
...> 
...>   defmacro get_counter() do
...>     quote do
...>       var!(counter_var, CleanAccess)
...>     end
...>   end
...> 
...>   defmacro multiply_by(number) do
...>     quote do
...>       var!(counter_var, CleanAccess) =
...>       unquote(number)*var!(counter_var, CleanAccess)
...>     end
...>   end
...> end
{:module, CleanAccess, <<70, 79, 82, ...>>, {:multiply_by, 1}}
```

让我们将之前的 CleanAccess 模块导入到一个新的 CleanAccessDemo 模块中：

```
iex> defmodule CleanAccessDemo do
...>   import CleanAccess
...>   def show_it() do
...>     IO.puts "Start with: #{start_counter(3)}"
...>     IO.puts "Counter: #{get_counter()}"
...>     IO.puts "Multiply by 5: #{multiply_by(5)}"
...>     IO.puts "Counter: #{get_counter()}"
...>     IO.puts "Start with: #{start_counter(1)}"
...>     IO.puts "Counter: #{get_counter()}"
...>     IO.puts "Binding on the CleanAccessDemo module: #{inspect(binding(CleanAccessDemo))}"
...>   end
...> end
```

```
iex> CleanAccessDemo.show_it
Start with: 3
Counter: 3
Multiply by 5: 15
Counter: 15
Start with: 1
Counter: 1
Binding on the CleanAccessDemo module: []
:ok
```

```
iex> counter_var
** (CompileError) iex:117: undefined function counter_var/0
```

CleanAccess 模块内部的宏在 CleanAccess 上下文中操作 counter_var，但正如我们所观察到的，counter_var 从未泄漏到调用者上下文。当你创建自己的宏时，请考虑你是否真的需要访问调用者上下文，或者是否能够像我们在这里所做的那样，简单地在定义宏的模块的上下文中定义这些变量。
