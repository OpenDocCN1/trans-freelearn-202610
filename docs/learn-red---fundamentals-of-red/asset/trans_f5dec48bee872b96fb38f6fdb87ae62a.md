
另一种做法是让函数在特定条件下生成错误，但只在函数被调用时捕获错误。例如，这里有一个在 `count` 为 `0` 时出错的 `average` 函数：

```
average: function [
    total [number!] count [integer!]] [
    if zero? count [
        cause-error 'math 'zero-divide []
    ]
    total / count ; might cause an error
]

average 354 6    ;== 59
average 354 0    ;*** Math Error: attempt to divide by zero
```

我们可以用 `error?` `try` 调用此函数，并按如下方式打印结果或错误信息：

```
either error? result: try [average 354 0] [
    print ["Error id: " result/id]
][
    print ["No error, the result is: " result]
]
;== Error id: zero-divide
```

对于这个简单示例来说有点过度，但在某些情况下可能很有用。与其在函数外部引发错误并捕获它，通常更简单的做法是直接测试错误条件、报告它，然后在函数之后继续执行程序，如下所示：

```
average: function [
    total [number!] count [integer!]] [
    if zero? count [return "Divide by zero"]
    total / count ; might cause an error
]

average 354 0 ;== "Divide by zero"
```

如果你只是想在不发出任何消息的情况下退出函数，只需使用 `exit`：

```
if zero? count [exit]
```

=> 现在回答 *问题* 部分的第 8 题。

# 递归函数

当函数用参数的另一个值调用自身时，该函数就是递归的。在多次递归调用之后，必须达到一个结束递归的基本情况（否则它将无限进行下去）。一个常见的例子是计算一个数的阶乘，其过程如下：

`4! = 4 x 3 x 2 x 1 = 4 x 3! = 4 x 3 x 2! = 4 x 3 x 2 x 1! = 4 x 3 x 2 x 1 x 0! = 24` 基本情况为—0! = 1。

这可以很容易地翻译成 Red 的 `fact` 函数：

```
fact: func [n][
        if n = 0 [ return 1]
        n * fact n - 1
]

fact 4 ; 24
```

这里我们使用了 `func`，但有时递归可能需要局部变量，而此时 `function` 会更有用，因为对于 `function` 来说，局部变量是默认的。

现在评估以下内容：

```
fact 4.0     ;(1) == 24.0
fact -4      ;(2) == *** Internal Error: stack overflow
fact "abc"   ;(3) *** Script Error: - does not allow string! for its value1 argument
fact 21      ;(4) *** Math Error: math or number overflow
```

所以你可以看到可以做很多事情来使这个函数更安全！让我们看看一些有用的规则：

* 参数应为整数
* 参数不能为负数
* 参数不能为字符串
* 因为 `fact 21` 对整数来说太大了，计算应停止在 `fact 20`

将此函数记录并实现保护测试作为练习留给你（*问题* 部分的第 9 题）。

# 使用 do 导入函数

在项目启动后不久，你会开始拥有大量函数，并且可能会问，*我应该把它们的代码放在哪里*？你很可能希望有一个紧凑的 `main.red` 文件，它启动你的程序并可以调用存储在其他文件中的函数。

假设这些文件中的一个名为 `useful-functions.red`，并且包含上一节中的 `fact` 函数和 `check` 函数：

```
;-- see Chapter06/useful-functions.red:
check: func [list] [
    answer: "safe"
    foreach l list [
        if find l "--" [answer: "unsafe"]
    ]
    answer
]
```

这里它具体做什么并不重要。它遍历一个 `list` 中的名称，并返回 `safe` 字符串，但如果列表中任何名称包含 `\--`，`check` 将返回 `unsafe` 字符串。

这通过以下示例来说明：

```
names1: ["Joe" "Dan" "Sh--" "Bill"]
names2: ["Paul" "Tom" "Mike" "John"]
check names1 ;==  unsafe
check names2 ;==  safe
```

我们如何从 `main.red` 中调用 `check`？

如果 `main.red` 中没有任何对 `check` 的引用，我们显然会得到 `Script Error: check has no value`。

也许你的第一个想法是使用 `#include %useful-functions.red`，正如我们在猜数字游戏中看到的那样，在 [第4章](#d6a19492-2244-47dc-a52e-64b113c7ce0a.xhtml)*，代码控制结构*。这种函数包含方式有效；`main.red` 中的代码现在可以正确执行：

```
print append "names1 is " check names1
print append "names2 is " check names2 
```

我们将得到以下输出：

```
names1 is unsafe
names2 is safe
```

我们多功能的 `do` 词也可以通过在 `main.red` 开头使用 `do` `%useful-functions.red` 来实现这一点。请注意，以这种方式使用的 `do` 仅在解释时有效，而在脚本编译时无效。这是在代码中导入 Red 脚本的两种方式。

# 代码即数据，数据即代码

现在我们回到 Red 强大的内省和元编程特性——即 Red 如何像处理数据一样处理代码。

# 对函数使用反射

在 Red 中，函数是第一类对象。你可以为自己的函数，甚至内置函数，给出另一个名称，如下所示：

```
;-- see Chapter06/reflection-functions.red:
pr: :print           ;== make native! [[
pr ["Hello" "Red"]   ;== Hello Red
```

`:` 阻止函数执行；`pr: get` `'print` 做同样的事情。

这展示了 Red 的灵活性和强大功能；不仅可以更改名称，甚至可以更改内置词的含义。正如常言所说，能力越大责任越大。谨慎使用这些和其他即将出现的元编程特性，最重要的是明智地使用！

函数可以包含其他局部于该函数的函数，并且可以访问其所有变量。

函数也可以将函数作为结果返回，如以下代码示例所示：

```
make-timer: func [code] [
    func [time] code
]

timer: make-timer [wait time]
timer 3 ;== none
```

`code` 变量获得一个 `wait time` 值，而 `time` 变量获得一个 `3` 值。

# source 词

你可以使用 `source` 词查看用户定义和中层函数的 Red 源代码。

例如，在 REPL 中输入 `source replace` 会显示我们在上一章遇到的 `replace` 函数的源代码。这是该命令的输出：

```
replace: func [
       series [series!]
       pattern
       value
       /all
 ...
 ]
```

它大约有 30 行，但我们在这里只展示了开头；请在控制台中输入该命令以查看完整输出。

这也适用于你自己的函数——例如，在 REPL 或代码文件中定义 `fact` 后，输入 `source fact`，你就可以看到它的代码！你甚至可以使用 `source source`。

通过查看这段代码你可以学到很多东西；也许你可以将其中的部分用于自己的函数或现有函数的新版本。

# body-of 函数

`body-of` 函数顾名思义，返回已定义函数的主体。例如，使用上一节中的 `check` 函数时，我们得到 `body-of :check`，它返回以下内容：

```
[   
    answer: "safe"
    foreach l list [
            if find l "--" [answer: "unsafe"]
    ] 
    answer
]
```

它也适用于内置函数，例如 `body-of :replace`。

起初这可能看起来没什么用。但请想想——函数的主体只是一系列代码行。这一系列行可以用为 `series!` 定义的所有词来操作，就像它是数据一样。这开启了大量新的可能性！

# 通过代码更改函数头和主体

只是为了说明 Red 的灵活性有多高，请考虑这个 `fp1` 函数：

```
fp1: function [x [integer!] y [string!]] [
    prin [x "- "]
    print y
]
```

让我们命名头（这是参数声明的部分）和主体：

```
header: [x [integer!] y [string!]]
body: [
    prin [x "- "]
    print y
]
```

现在我们可以将函数重写为 `fp1-alt: function header body`。
