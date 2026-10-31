
# 函数体放置

`body` 短语必须与 `function` 和 `header` 在同一行；否则，你会得到 `Script Error: function is missing its body argument` 错误（仅在 `Red` 控制台中）。

两个函数仍然以相同的方式工作，如以下代码片段所示：

```
fp1 108 "Red"      ;== 108 - Red
fp1-alt 108 "Red"  ;== 108 - Red
```

现在让我们操作 `body` 来反转输出中的第二个参数：

```
fp2: function header replace copy body 'y [reverse y]
fp2 109 "Red"    ;== 109 - deR
```

注意你需要将 `y` 视为字面单词 `'y`；否则，你会得到 `Script Error: y has no value` 错误。

这里是另一个示例，我们在其中构造函数的参数和主体部分：

```
params: []
body: []
append params load {message}
append body load {print message}
prn: function params body   ;-- function definition
prn "Hello World"           ;== Hello World
source prn                  ;== prn: func [message][print message]
```

再次，消息必须在这里用 `{ }` 括起来作为字符串，以防止求值。我们也可以使用字面 `'` 单词表示法，用以下内容替换两个 `append` 语句：

```
append params 'message
append body [print message]
```

这产生完全相同的结果。

# 代码即数据 – load

`Red` 中的代码行只是数据结构。让我们从一些非常基础的代码开始，例如算术表达式——例如 `3.14 ** 2`，计算 `3.14` 的平方，结果为 `9.8596`。你在编辑器中编写代码，所以它基本上是一个字符串，对吧？

```
;-- see Chapter06/code-data.red:
calc: "3.14 ** 2"
type? calc     ;== string!
calc           ;== "3.14 ** 2"
```

这里，`:calc` 或 `get 'calc` 返回的结果与 `calc` 相同。

现在我们必须将这个字符串表达式转换为 `Red` 可以处理的东西——即块中的一个序列。这可以通过 `load` 单词来完成，它将字符串加载到内存中，解析它并将其转换为 `Red` 值：

```
code: load calc    ;== [3.14 ** 2]
code               ;== [3.14 ** 2]
type? code         ;== block!
length? code       ;== 3     ; code is only a series at this point
```

使用 `code` 单词目前还没有任何作用。我们的 `do` 单词来救援：

```
do code    ;== 9.8596
```

所以我们需要 `load` 将表达式转换为 `Red` 可以理解的数据，并且由于数据即代码，`do` 可以运行该数据。

现在让我们操作我们的 `code`，毕竟它只是数据。让我们用 `pi` 替换 `3.14` 并计算它的三次方：

```
code/1: pi          ;== 3.141592653589793
code/3: 3           ;== 3
code                ;== [3.141592653589793 ** 3]
do code             ;== 31.00627668029982
```

更改运算符同样容易，但我们必须在这里使用字面 `'` 语法，以便 `\-` 不会被求值，如下所示：

```
code/2: -      ;*** Script Error: - operator is missing an argument
; solution:
code/2: '-     ;== -
code           ;== [3.141592653589793 - 3]
do code        ;== 0.1415926535897931
```

`\-` 的类型只是 `word!`；你必须使用 `get` 来获取其值，即一个操作 `op!`。

我们可以使用 `/2` 表示法或使用 `second` 来查看这一点：

```
code/2                 ;== -
second code            ;== -
type? code/2           ;== word!
type? second code      ;== word!
:code/2                ;== -
type? get second code  ;== op!
```

我们可以随意扩展代码表达式，例如参考以下代码：

```
append code [- pi]      ;== [3.141592653589793 - 3 - pi]
code                    ;== [3.141592653589793 - 3 - pi]
do code                 ;== -3.0
```

现在很清楚，直到你求值代码（使用 `do`），它只是数据。

与我们在前面的代码中将 `print` 重命名为 `pr` 相同，我们甚至可以更改 `\-` 单词使其表示 `+` 操作：

```
-: :+         ;== make op! [[ "Returns the sum of the two values" ...
code          ;== [3.141592653589793 - 3 - pi]
do code       ;== 9.283185307179586
```

表达式看起来仍然相同，但现在它具有完全不同的含义！当然，你不应该对内置单词这样做；这只是为了展示 `Red` 的反射能力。

现在让我们以同样的方式探索我们的 `inc` 函数，使用 `inc: func [n][n + 1]`：

```
fun: "inc: func [n][n + 1]"        ;== "inc: func [n][n + 1]"
type? fun                          ;== string!
code: load fun                     ;== [inc: func [n] [n + 1]]
code                               ;== [inc: func [n] [n + 1]]
type? code                         ;== block!
```

在这个阶段，`code` 仍然是 `block!`，`inc 7` 返回 `*** Script Error: inc has no value` 错误，并且 `load` 不运行任何内容。我们需要 `do` 将该数据转换为函数：

```
do code      ;== func [n][n + 1]
inc 7        ;== 8
type? :inc   ;== function!
```

具有 `[inc: func [n] [n + 1]]` 值的 `code` 块需要被求值，以便在内存中构造 `function!` 值，这正是 `do` 所做的。

让我们使用 `foreach item code [print [type? item mold item]]` 检查 `code` 的每个项目的类型。

这会打印以下输出：

```
set-word inc:
word func
block [n]
block [n + 1]
```

如你所注意到的，我们的 `code` 块包含四个 `set-word!` 和 `word!` 类型的 值以及两个 `block!` 实体。`Red` 中的每个值都有其数据类型！

总之，`Red` 中的所有代码也是数据，因此代码可以在执行的任何阶段非常容易地被操作。从另一个角度来说，当我们指示 `Red` 解释它时，数据就变成了代码。

其他动态语言，例如 `JavaScript` 和 `Ruby`，有类似的 `eval` 关键字，但 `eval` 会立即运行源代码字符串；字符串不会被加载并转换为数据，就像 `Red` 中那样。

这种代码即数据的理念使代码的序列化和持久化变得非常容易；在 `Red` 中，远程发送代码或将运行程序的代码快照保存到磁盘都是单行操作，正如我们在下一章中将看到的那样。这也可以应用于代码热交换和运行程序的实时调试等功能。

# 使用对象

现在我们将发现 `Red` 如何处理对象，这可能与你在经典面向对象语言中习惯的不同，例如 `Java`、`C#`、`Python` 和 `Ruby`。但首先，我们认识 `make` 单词。

# make 单词

在 *代码控制结构,* 中的 *使用 error?* 部分 [第 4 章](#d6a19492-2244-47dc-a52e-64b113c7ce0a.xhtml) 中，我们使用 `probe` 来获取特定错误的更多详细信息，这返回的输出以 `make error! [ ... ]` 开头。`error!` 实际上是一个使用 `make` 构造的对象。

事实证明，`make` 可用于为任何数据类型构造新值。它需要类型，以及作为第二个参数的该类型的规范。这可以是一个值或内存分配，或者在 `datatype!`、`native!`、`action!`、`routine!` 和 `event!` 的情况下是另一个规范

以下是一些示例：

```
;-- see Chapter06/objects.red:
i: make integer! 5              ; == 5    ; (1)
output: make string! 1000       ; == ""   ; (2)
blk: make block! 20             ; == []   ; (3)
```

在第 `(1)` 行，规范是整数本身，在第 `(2)` 行，是字符串中的字符数，在第 `(3)` 行，是块的初始大小。

`make` 单词很有用，当你知道你的代码需要某种类型和大小的值，但最初也不知道其内容时。在这种情况下，出于性能考虑，使用 `make` 动态构造序列比使用 `copy ""` 这样的字面量更好。

# 处理对象

在诸如 `Java` 或 `Ruby` 之类的语言中，你首先创建一个具有属性和方法 的类，像一种模板，然后你可以从该类创建对象，通常使用 `new` 构造函数，如

`new ClassName(...)`.
