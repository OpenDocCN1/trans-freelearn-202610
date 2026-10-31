
```
;-- see Chapter06/do-does-has-func.red:
cls: does [ loop 100 [print newline] ] ; == func [][loop 100 [print newline]]
cls ; call function cls
```

The `newline` phrase is the character that prints a new line.

在前面的示例中，我们创建了一个名为 `cls` 的函数，它在被调用为 `cls` 时通过打印 `100` 行空白来清除输出屏幕。它是一个函数（`func`），我们可以在定义 `cls` 时从返回值中看到这一点。当你需要一个没有参数且没有局部变量的函数时，使用 `does`。

=> 现在回答 *问题* 部分的第 2 题。

重要区别应在于以下两种变量之间：

* 局部变量，仅在函数体本身中已知
* 全局变量，在函数内部以及外部都已知

# has 关键字

要定义一个没有参数但具有局部变量的简单函数，这些局部变量在代码块之前的块中定义，请按如下方式使用 `has`：

```
calc-hours-year: has [number] [
    number: 365 * 24 ; number is local
    print number
]   ;== func [/local number][...]
calc-hours-year ;== 8760
number   ; *** Script Error: number has no value 
```

`calc-hours-year` 的返回值表明它是一个函数（`func`），具有一个在函数外部未知的局部 `number` 变量。另一种看待方式是，`has` 将其第一个块中的单词变成了局部变量。

=> 现在回答 *问题* 部分的第 3 题。

# func 关键字

要定义一个带参数（们）的函数，请使用 `func` 关键字，如下所示的执行递增的函数：

```
inc: func [n][n + 1] ;== func [n][n + 1]
inc   ; *** Script Error: inc is missing its n argument
inc 7 ;== 8
```

请注意，`func` 需要其参数由调用代码作为参数传递；否则，你会得到错误 `... is missing its ... argument`。但是，与大多数其他编程语言不同，调用函数时你不需要用括号 `()` 将参数括起来。

顺便说一下，通过函数示例你会更好地理解 `:` 或 `get` 关键字。`inc` 词（或更好 `inc n`）调用（求值）函数，但 `:` 返回其自身的值：

```
:inc          ;== func [n][n + 1]
get 'inc      ;== func [n][n + 1]
```

下面的 `func` 将其两个参数 `a` 和 `b` 求和：

```
sum: func [a b][
    number: a + b
    print number ; global variable
]                ;== func [a b][...]
sum 3 5 ;== 8
print number + 1    ;== 9  ; number is a global variable
a                   ; *** Script Error: a has no value
```

重要的是要记住，在 Red 中，函数必须始终在被使用之前定义；否则，在调用 `fname` 函数时你会得到 `*** Script error: fname has no value`。始终在程序开始时定义你的函数，并在之后调用它们。然而，函数定义的顺序无关紧要；在另一个函数内部，可以在函数被定义之前调用它。

请注意，在 `func` 中定义的所有变量（如示例中的 `number`）都是 *全局* 的：它们在函数外部已知。如果该变量已在函数外部存在，其值将被函数更改；我们说它是在遮蔽全局变量（试试看）。这几乎肯定会导致错误。为了避免该冲突，你可以在函数中将 `number` 变量定义为局部变量，如 `sum` 函数中所示：

```
number: 108                      ; global
sum: func [a b /local number][
    number: a + b
    print number                 ; local 
]                                ;== func [a b][...]
sum 3 5                          ;== 8
print number + 1                 ;== 109 ; global
```

最好的解决方案可能是完全给它起另一个名字。

# function 关键字

`function` 关键字允许最完整的函数定义。我们将在下一节中描述并展示所有可能属性的示例。它与其他形式的一个关键区别是 `function` 自动使其变量为局部变量，因此不需要 `/local`。从前面的示例中省略 `/local` 给我们 `function sum`：

```
number: 108               ; global
sum: function [a b][
    number: a + b         ; local
    print number          ; local 
]                         ;== func [a b /local number][...]
sum 3 5                   ;== 8
print number + 1          ;== 109 ; global
```

从 `sum` 定义后的返回值中，我们可以看到 `number` 被自动设为局部变量——全局值和局部值之间不再可能发生冲突。

如果你真的需要，可以使用 `/extern` 强制变量为全局。应用此操作会将我们的 sum 函数更改为 `sum: function [a b /extern number][...]`，并且 `number + 1` 现在的值将为 `9`。

下表将帮助你清楚地理解不同类型的函数：

| 它做什么？ | `does` | `has` | `func` | `function` |
| --- | --- | --- | --- | --- |
| 指定参数 | N | N | Y | Y |
| 指定局部变量 | N | Y | With `/local` | Default |
| 引用全局变量 | Y | Y | Default | With `/extern` |

`function` 体中的所有内容都是局部的，而 `func` 体中的所有内容都是全局的。这就是为什么在更复杂的代码中使用 `function` 更好，因为 `function` 的代码与周围代码更加隔离。

=> 现在回答 *问题* 部分的第 4 题。

# 复制局部序列

我们已经在 [第 5 章](#35fe4037-dd49-406f-b90b-217052b1d33f.xhtml) 的 *复制序列* 部分中提到过这一点，*使用序列和块*，但由于这是新 Red 程序员的一个陷阱，我们在这里再次强调——函数局部的序列（也称为 `string`）应使用 `copy` 初始化。如果不这样做，序列将在后续函数调用中保持其值，这对新手来说是一个意外的结果。以下是如何使用函数正确初始化局部序列的正确示例：

```
correct: function[] [
    data: copy []             ; correct initialization!
    append data 108
    data
]

correct ;== [108]
correct ;== [108]
correct ;== [108]
```

使用 `data: []` 运行该片段，自己看看！

# 函数属性

在本节中，我们将学习如何为参数指定类型，如何从函数返回值，如何定义细化，如何记录函数，以及如何使用函数进行错误处理。

# 传递参数

这里，我们必须在单个、所谓的 `scalar!` 值（如数字、日期、字符等）和所有其他值之间做出区分（参见 [第 3 章](#6609439d-2797-41d5-97e3-5b2efe61d4bb.xhtml) 中的 *类型系统* 部分，*使用词、值和类型*）：

* 标量值按 *值* 传递，这意味着值的副本被发送到函数
* 其他值按 *引用* 传递，这意味着对值的引用被发送到函数

其结果是标量值不能被函数更改，而其他值（如序列和对象）会被更改。这可以在下面的代码片段中看到：

```
;-- see Chapter06/function-attributes.red:
passing-ref: function [data][
    append data 108
]

data: [2 3 7 42]
passing-ref data

data    ;== [2 3 7 42 108]
```

# 为参数指定类型

之前，我们看到了一个递增 `inc: func [n][n + 1]` 函数。该函数接受一个 `n` 参数，但未为 `n` 指定类型。`inc` 对数字有效（`inc 3.14` 返回 `4.14`），但例如对字符串无效，因为 `+` 操作未为字符串定义。因此，当我们使用 `inc "abc"` 时，它会显示 `*** Script Error: + does not allow string! for its value1 argument` 错误。
