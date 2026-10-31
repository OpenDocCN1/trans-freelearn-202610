# 高级 Red
在本最终章节中，我们将探索语言的更多高级方面，并展望 Red 在不久的将来的发展方向。响应式编程是编写更具响应性（GUI）应用程序的绝佳方式。我们在 第 1 章，*Red 的使命*，和 第 2 章，*开发环境设置* 中遇到了 `Red/System` 作为 Red 的底层基础。在这里，我们将看到你如何在 Red 应用程序中或独立使用它。

本章将涵盖以下主题：
* 响应式编程
* `Red/System`
* 与操作系统交互
* 用于性能的数据类型
* 嵌入式 Red 和宏
* Red 用于智能合约编程
* Red 的路线图
* 一些有用的参考

## 技术要求
你可以在 [`github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter10`](https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter10) 找到本章的代码。如果你已按照 第 2 章，*开发环境设置* 中所述安装了 Red，你就可以开始了。你可以在 Windows、OS X 或 Linux 上工作。你可以在 `Red` 控制台中输入或粘贴任何代码以查看其结果，或者你可以使用 Visual Studio Code 等编辑器开始使用。

## 响应式编程
在 第 9 章，*构建视觉界面* 中，我们看到了通过附加 actor 代码块来处理小部件上的事件是多么容易。我们遇到了几个例子，其中一个面孔中的更改或操作会影响另一个面孔——例如，点击按钮会更改字段中的文本。文本字段对按钮做出了反应，因为我们明确地在按钮上编写了 actor。

请看以下代码片段：
```
;-- see Chapter10/reactive1.red:
point: [x: 3 y: 5]
distance: square-root (point/x ** 2) + (point/y ** 2)
print distance    ;== 5.8309518948453
point/x: 2
print distance    ;== 5.8309518948453    <-- doesn't change!
distance: square-root (point/x ** 2) + (point/y ** 2)
print distance    ;== 5.385164807134504  <-- changed
```

`distance` 词给出了 `point` 到原点的距离，但当点的坐标发生变化时，`distance` 并不会随之自动改变。只有在更改后要求 `distance` 再次计算自己时，我们才会得到更新的结果。

你可以想象，让词在更改发生时自动更新其值会很有用——例如，在跟踪 running total 时，或在电子表格单元格中的计算。Red 已将其构建到其响应式框架中。以下是如何使用它使 `distance` 跟随 `point` 的更改：
```
point: make reactor! [x: 3 y: 5]
distance: is [square-root (point/x ** 2) + (point/y ** 2)]
print distance    ;== 5.8309518948453
point/x: 2
print distance    ;== 5.385164807134504
```

`point` 词是一个所谓的 `reactor`，即更改的原因或 `source`；这用 `make reactor!` 表示。`reactor` 字段中的更改会自动（即无需调用函数）触发其他 `target` 对象的更改，例如本例中的 `distance`。这些目标通过使用 `is` 词定义为 `reactive expression`。更改从 `source` 传播到 `target`：
`reactor` (`source`) ----> `target(s)`.

reactor 甚至可以更新自身，这需要的代码更少，如下所示：
```
point: make reactor! [
    x: 3 
    y: 5 
    distance: is [square-root (x ** 2) + (y ** 2)]
]
point ;== make object! [x: 3 y: 5 distance: 5.8309518948453]
point/x: 2
point ;== make object! [x: 2 y: 5 distance: 5.385164807134504]
```

不要让更改导致自身更改，引发无休止的循环！

更改的来源必须是具有字段的对象，例如示例中的 `point/x` 和 `point/y`。如果目标引用多于一个嵌套级别（例如 `shape/point/x`），请改用 `make deep-reactor!` 而不是 `make reactor!`。

这种响应性在编写图形界面时也可以使用——而不是编写 actor，请在目标侧使用 `react` 词。以下示例比较了以两种方式完成的滑块和进度条之间的同步。在 `Tab 1` 中，使用了响应式编程（进度条是目标），而在 `Tab 2` 中，我们采用了上一章的 actor 代码：
```
;-- see Chapter10/reactive-gui.red
view [
    origin 0x0 space 0x0
    tab-panel 500x500 [
        "Tab 1 " [
            progress 100x20 data 20% react [face/data: s/data]
            s: slider 100x20 data 20%
        ]
        "Tab 2 " [
            p: progress 100x20 20%
            slider 100x20 data 20% [p/data: face/data]
        ]
    ]
]
```

响应式框架只有大约 250 行 Red 代码，并且非常通用。一个对象可以是许多源对象的目标，你也可以创建反应对象的链。此框架不会让你做以前做不到的事情，但它会让你以另一种方式思考问题，你的代码将更紧凑、可读且声明式。



对于结合 `draw`、`react` 和实时编码的时钟的令人印象深刻的演示，请参阅 [`www.red-lang.org/2016/07/eve-style-clock-demo-in-red-livecoded.html`](https://www.red-lang.org/2016/07/eve-style-clock-demo-in-red-livecoded.html)。

\=> 现在解答 *问题* 部分的第 1 和第 2 题。

# Red/System

Red/System 是 Red 的 C 级、纯命令式方言，具有四个主要目的：

* 它充当编译后 Red 代码的 **IR**（**中间表示**）语言的角色，并能高效地直接编译为所有目标的机器码，因此不会将 C 或汇编作为中间产物生成。
* 它是编写 Red 本身部分内容，尤其是 *运行时系统* 的语言。
* 它可以嵌入 Red 应用中以提供快速代码支持，因此，只要你的 Red 应用需要运行更快，你就可以将性能最差的代码片段用 Red/System 重写以获得巨大的性能提升。
* 它可用于编写 *低级程序* 并执行 *系统编程*。

Red/System 是一种方言，是 Red 的子集，也是其宿主语言 Red 的嵌入式 **DSL**（**领域特定语言**），但宿主却以该方言实现。

你可以在 `red-source/runtime` 和 `red-source/system/runtime` 文件夹中找到 Red/System 源代码。

# 比较 Red 和 Red/System

有非常好的理由去更好地了解 Red/System。它对你来说会很熟悉，因为它使用与 Red 相同的语法结构，所以在 Red 和 Red/System 之间切换时，你不必改变思维方式。事实上，一些小的代码片段可能完全相同，如以下 Red/System 脚本所示：

```
;-- see Chapter10/hello-world.reds:
Red/System [
    Title: "Hello World demo"
]
print "Hello world from Red/System!"
```

看起来很像 Red，不是吗？唯一的区别是 `Red/System \[ \]` 头部和 `.reds` 文件扩展名。

此外，Red/System 代码不能被解释；你必须使用 `red -r hello-world.reds` 编译它，速度非常快。

让我们比较一个计算阶乘的函数（参见 第 6 章 中的 *递归函数* 部分，*使用函数和对象*）。请记住，在 Red 中，以下代码可以实现：

```
 fact: func [n][
     if n = 0 [ return 1]
     n * fact n - 1
 ]
```

在 Red/System 中，它变为如下：

```
;-- see Chapter10/fact.reds:
fact: func [
    n [integer!]
    return: [integer!]
][
    if n = 0 [ return 1]
    n * fact n - 1
]
```

请注意计算代码保持不变，但 Red/System 中，*函数参数类型和返回值的声明* `return:` *是强制性的*。

为了更好地欣赏差异，这里有一个 `as-color` 函数的比较，该函数接受三个整数 `r`、`g` 和 `b`，并将它们作为元组返回。这里是 Red 版本：

```
;-- see Chapter10/as-color.red:
as-color: function [
    r [integer!]
    g [integer!]
    b [integer!]
][
    make tuple! reduce [r g b]
]
print as-color 153 255 51 ;== 153.255.51
```

将其与 Red/System 版本进行比较：

```
as-color: function [
    r [integer!]
    g [integer!]
    b [integer!]
    /local arr1 [integer!]
][
    arr1: (b % 256 << 16) or (g % 256 << 8) or (r % 256)
    stack/set-last as red-value! tuple/push 3 arr1 0 0
]
```

你可以看到最后一个版本使用了位操作，并且结果必须写入栈。

Red/System 提供了对低级 CPU 寄存器操作（中断、I/O、栈、特权模式）的支持，以及内联汇编器。它必须使用可放入 CPU/FPU 寄存器的值，并让你完全控制内存分配。

与宿主语言 Red 相比，它只有少数几种原始类型，例如 `logic!`、`byte!`、`integer!`、`float!`、`float32!`、`function!`、`c-string!`、`struct!` 和 `pointer!`。

`c-string!` 类型促进了与 C 库的交互，而 `struct!` 和 `pointer!` 使对内存分配和访问的精确控制成为可能。

# 从 Red 中调用 Red/System 脚本

为了将高性能代码片段委托给 Red/System，我们需要一种从 Red 调用 Red/System 的方法。假设我们想从 Red 脚本调用我们的 Red/System `fact` 函数。这如下所示完成：

```
;-- see Chapter10/use-fact.red:
#system-global [#include %fact.reds]          ; (1)

red-fact: routine [                           ; (2)
    n [integer!]                              ; (3)
    return: [integer!]
][
    fact n  ; function from fact.reds         ; (4)
]

print red-fact 9 ;== 362880                   ; (5)
```

在第 `(1)` 行，Red/System 脚本像通常的 Red 脚本一样被包含，但它必须以 `#system-global` 为前缀，这是一个 *指令*。该行还执行 Red/System 脚本，因此请确保从中删除或注释掉任何可执行代码。如第 `(2)` 行所示，调用 Red/System 函数的 Red 函数是特殊的 `routine!` 类型。第 `(3)` 行及其后一行告诉我们例程必须为其参数键入并返回一个值。在第 `(4)` 行，引用了 Red/System 函数，而在第 `(5)` 行，`fact` 函数实际被调用。

Red/System 代码也可以包含其他编译器指令，例如 `#define`、`#include`、`#import`、`#syscall`、`#if`、`#either`、`#switch` 等等。它们构成了 *预处理器* 方言的一部分，该方言也可以在 Red 本身中使用。你可以在 [`doc.red-lang.org/en/preprocessor.html`](https://doc.red-lang.org/en/preprocessor.html)[.](https://doc.red-lang.org/en/preprocessor.html) 找到有关预处理器的更多信息。

包含 Red/System 代码的 Red 程序必须以发布模式编译为 `red -r use-fact.red`；包含的 Red/System 脚本不需要预先编译。

`do` 词在这里不起作用，因为它只解释代码。

\=> 现在解答 *问题* 部分的第 3 和第 4 题。

只是为了好玩，你可以在编译命令中添加一个 `\-v` 标志来查看编译 Red 源时生成的 Red/System 代码，如 `red -r \-v 3 hello-world.red > output`（在任何文本编辑器中打开 `output` 文件）。

# 与操作系统交互

在编写应用时，你必须能够做的事情包括在启动时获取信息、从应用内部启动不同的程序以及使用操作系统剪贴板。在这里，我们将向你展示如何在 Red 中完成此操作。

## 从命令行参数获取输入

命令行参数被捕获在 `system/script/args` 词中。它是一个字符串，其中所有参数都存储在其中，以空格分隔：

```
;-- see Chapter10/args.red:
print system/script/args           ;== "78 A Red"
type? system/script/args           ;== string!
args: split system/script/args " " ;== ["78" "A" "Red"]
print ["Number of command-line arguments:" length? args]
                                   ;== Number of command-line arguments: 3
foreach arg args [
    print arg
]
; 78
; A
; Red
```

我们可以使用 `split` 在一个 `args` 系列中获得分隔的参数，并开始使用它们。

要查看输出，请使用 `red args.red 78 A Red` 启动程序，或在编译后使用 `red -r`，使用以下命令启动：`./args 78 A Red`。

## 调用外部程序 – call

从 Red 应用内部运行外部进程、程序或 shell 脚本很容易——只需使用 `call` 词。以下是一些你可以在 Red 控制台中尝试的示例：

```
;-- see Chapter10/interacting-os.red:
call "explorer.exe" ;== 9668 (1)
call/shell "notepad.exe" ;== 2292 (2)
call/shell "notepad.exe info.txt" ;== 11632 (3)
cmd: "notepad.exe info.txt"
call/shell cmd ;== 7244 (4)
call/shell/wait "notepad.exe" ;== 0 (5)
call/output "ls" %listing ;== 0 (6)
call "start copy nul > myfile.txt" ;== 13760 (7)
```

让我们逐行分解正在发生的事情：

1. 在这里，我们启动 Windows 资源管理器。
2. 启动记事本（在 Windows 上）。



3. 这里，文件 `info.txt` 将在 `Notepad` 中打开。

4. 你可以将 `shell` 命令存储在一个 `word` 中。

5. 在之前的示例中，`Red` 应用会继续运行。如果希望它等待已启动的进程完成，请使用 `/wait`。

6. 将命令的输出写入给定的文件（这里是在 Linux/OS X 上）。输出也可以是一个变量。

7. 任何有效的 `shell` 命令都可以传递给 `call`。这里，我们在 Windows 上使用 `start` 来创建空文件。

返回的 `call` 值要么是已启动程序的进程 ID，要么在启动成功时为 `0`，出错时为 `-1`。

# 使用操作系统剪贴板 – read-clipboard

以下代码会暂时将一些信息存储到剪贴板：

```
write-clipboard "Some important info" ;== true
; close Red console or Red program
print read-clipboard ;== Some important info
```

这可用于在两个 `Red` 应用之间进行通信。信息会保留在剪贴板上，直到操作系统重启或有其他程序写入剪贴板为止。

# 用于性能的数据类型

我们都喜欢并熟悉我们信赖的 `series!`，但某些应用需要尽可能高的性能，尤其是在处理大型数据集时。正因如此，才构建了 `vector!`、`map!` 和 `hash!` 这些数据类型。

## vector! 数据类型

当你的 `series!` 仅包含 `integer!`、`float!`、`char!` 或 `percent!` 类型中的一种值，并且你需要快速的数学运算时，就应该使用 `vector!` 类型，如下所示：

```
;-- see Chapter10/perf-datatypes.red:
v1: make vector! [7 13 42 108]
vector? v1       ;== true
```

在 `vector!` 中，所有值都必须是同一类型。你可以像这样对向量执行简单的算术运算：

```
v1 + 2       ;== make vector! [9 15 44 110]
print v1     ;== 9 15 44 110
v1 * 4       ;== make vector! [36 60 176 440]
v2: make vector! [1 2 3 4]
v1 + v2      ;== make vector! [37 62 179 444]
```

请注意，对一个向量的操作会原地修改它，这意味着我们示例中的向量 `v1` 会被操作改变。我们从 `series!` 中知道的大多数操作都可以用在向量上。以下是一个示例：

```
v1: make vector! [7 13 42 108]
length? v1 ;== 4
append v1 666 ;== make vector! [7 13 42 108 666]
find v1 42 ;== make vector! [42 108 666]
; looping through a vector:
foreach n v1 [prin [n "-"]] ;==7 - 13 - 42 - 108 - 666 -
v1/3 ;== 42 
```

## map! 和 hash! 数据类型

许多语言都有字典类型，用于高效存储与键关联的值，并为这些键提供快速查找——例如，`key1` -> `value1`、`key2` -> `value2`，等等。`Red` 为此提供了两种数据类型，`hash!` 和 `map!`。

使用 `hash!`，你得到的类型具有 `series!` 的所有导航功能。当键值是简单类型时，它们会被哈希化，从而实现 *快速值查找*。首先定义键，然后是其值，然后是新的键及其值，以此类推：

```
h1: make hash! [n 13 m 42 o 108]
h1/m           ;== 42
select h1 [m]  ;== 42 
select h1 'm   ;== 42

hash? h1       ;== true
hash? [n 13 m 42 o 108]   ;== false
```

值查找可通过路径表示法 `hash/` `key` 或通过 `select` 进行，键可以以块或字面单词的形式表示。这种数据类型特别适合处理需要不断搜索的大型数据集。`hash!` 也可以在没有键值对的情况下使用，只是为了为 `series!` 提供快速查找，如下所示：

```
fruits: ["apple" "ananas" "tomato" "strawberry" "grap"]
to-hash fruits  ;== make hash! ["apple" "ananas" "tomato" "strawberry" "grap"]
pick fruits 3   ;== "tomato"
```

`map!` 类型是使用哈希表的高性能字典的首选类型，但与 `hash!` 不同，它不是 `series!` 类型，因此例如它不知道索引位置或偏移量：

```
m1: make map! ["Red" 1 "Crystal" 7 "Python" 13 "Ruby" 42]
== #(
 "Red" 1
 "Crystal" 7
 "Python" 13
 "Ruby" 42
)
```

然而，我们可以看到它有一个方便的特殊语法，也可以用来创建映射：

```
m5: #(
    "Red" 1
    "Crystal" 7
)
type? m5 ;== map!
```

要从映射中获取值，请使用 `select`。要插入新值或修改现有值，请使用 `put`。要从映射中删除键值对，请使用 `put` `key` `none`：

```
select m1 "Crystal"     ;== 7
put m1 "Crystal" 5      ;== 5
put m1 "Java" 132       ;== 132
put m1 "Ruby" none      ;== none ; delete key "Ruby" and its value 
print m1
; output:
"Red" 1
"Crystal" 5
"Python" 13
"Java" 132
```

使用 `find` 检查映射是否包含某个键。如果包含，则返回 `true`；否则返回 `none`。`length?` 词返回键值对的数量。使用 `keys-of` 和 `values-of` 分别以块形式获取映射的键和值：

```
find m1 "Crystal" ;== true
find m1 "C#"      ;== none
length? m1        ;== 4
keys-of m1        ;== ["Red" "Crystal" "Python" "Java"]
values-of m1      ;== [1 5 13 132]
body-of m1        ;== ["Red" 1 "Crystal" 5 "Python" 13 "Java" 132]
```

所有这些功能使得 `map!` 成为构建字典数据结构的理想类型。

键可以是 `any-word!`、`any-string!`、`scalar!` 和 `binary!` 数据类型。

当键是单词时，熟悉的 `/` 路径表示法可用于检索和设置值：

```
m4: make map! [n 13 m 42 o 108]
m4/m ;== 42
m4/m: 43
print m4
; output:
n: 13
m: 43
o: 108
```

当键是其他类型时，键值必须用 `( )` 括起来，路径表示法才能工作。也可以使用 `extend` 词将一系列键值对合并到现有映射中：

```
extend m4 [p 113 q 666]
print m4
;== #(
    n: 13
    m: 43
    o: 108
    p: 113
    q: 666
)
```

同一个词也可以用来为对象扩展新的属性和值。

默认情况下，查找和修改不区分大小写——也就是说，键 `Red`、`red` 和 `RED` 被视为相同。如果想区分大小写工作，请使用 `put/case` 和 `select/case`。

预定义映射之一是 `list-env`，它为你提供操作系统环境变量（针对当前进程）的映射。你可以按如下方式读取这些变量的值：

```
select list-env "windir"   ;== "C:\WINDOWS" ; on Windows
select list-env "SHELL"    ;== "/bin/bash"
```

=> 在 *Questions* 部分的第 5 题中尝试一下。

# 嵌入式 Red 和宏

在本简短章节中，我们将介绍一些你应该了解的 `Red` 高级主题。我们会提供更详细描述的参考，以防你在项目中需要它们。

## 嵌入式 Red

用其他语言编写的软件可以通过 `libRed` 与 `Red` 交互，`libRed` 是 `Red` 解释器和运行时库的特殊版本。这样，例如 `C` 程序就可以与 `Red` 环境交互。还存在 `Visual Basic`、`Ruby` 和 `Julia` 的绑定（[`github.com/joa-quim/Red.jl`](https://github.com/joa-quim/Red.jl)）。详细文档位于 [`doc.red-lang.org/en/libred.html`](https://doc.red-lang.org/en/libred.html) 和 [`github.com/red/red/wiki/libRed`](https://github.com/red/red/wiki/libRed) [更多示例。](https://github.com/red/red/wiki/libRed)

## 宏

许多现代编程语言都有的另一个特性是宏。宏是关于在运行时转换代码。到现在为止，你应该已经确信 `Red` 已经能够以最小的努力做到这一点；想想你用 `do`、`reduce` 和 `load` 能做什么。毕竟，代码即数据，数据即代码！因为 `Red` 首先是一种编译语言，所以 `Red` 中的宏更倾向于在编译时转换代码，但在解释时同样有效。以下是一个宏示例，供你一窥究竟：

```
;-- see Chapter10/macros.red:
#macro as-KB: func [n][n * 1024]

print as-KB 64 ;== 65536   
```



When the macro is expanded by the preprocessor, the code will result in `print 65536`. This example is a *named macro*, and it acts like a function.

Macros in `Red/System` are quite different. The preprocessor directives we mentioned briefly in the section on `Red/System` also allow you to process code while compiling.

You can find more information on macros at [`www.red-lang.org/2016/12/entering-world-of-macros.html`](https://www.red-lang.org/2016/12/entering-world-of-macros.html).

# Red 用于智能合约编程

Although it is a very hyped nowadays, it is widely accepted that the introduction of *blockchain technologies* will have a dramatic impact on many industries and commercial activities, mainly because they are based on a trustworthy, decentralized model of storing transactions. That storage is programmable with *smart contracts*, making it truly adaptable to a multitude of needs. Also running on this blockchain infrastructure are opensource **decentralized applications** (or **dapps**), currently developed in a web stack model. Most of the current technology for developing smart contracts and dapps is web based, and overly complex and bloated. The main language for development on the Ethereum blockchain is Solidity, a language derived from JavaScript, which is known for its design flaws and security shortcomings.

As we mentioned in 第一章, *Red 的使命，* in Jan 2018, the Red team embarked on a mission to provide a superior solution to smart contract and dapp development based on the innovative Red full stack language and toolchain.

## Red/C3

Throughout this book, we have seen that Red, thanks to its metaprogramming capabilities, is especially well suited for creating **domain-specific languages** (`DSLs`). Red already uses a number of dialects, such as `parse`, `vid`, `draw`, and `Red/System`. The Red team is building a DSL for smart contract and dapp programming, called `Red Cross Chain Code` (`Red/C3`). This dialect will compile directly to the bytecode of the different **virtual machines** (`VM`) of the blockchains it will target; it will first run on the Ethereum framework. The dapps' user interfaces will be built with Red's cross-platform GUI capabilities we discussed in the previous chapter.

`Red/C3`, written in Red, will consist of two layers:

*   *一个非常高层的层* of specialized, almost natural language that is a restricted subset to provide higher safety. This layer can call into the lower, general purpose layer.
*   *一个下层的通用层*, a statically typed subset of Red for general computational work.

They will be able to call contracts written in other languages for external compatibility. The first alpha of the `Red/C3` compiler is planned for release in June 2018.

## 使用 Red 编写 dapps

The `Red/C3` toolchain will be fully integrated, and consist of the following:

*   *编译器* for each blockchain VM bytecode, starting with an Ethereum compiler
*   An automated *测试模块*, to guarantee better contract testing and, thus, higher safety
*   *高性能区块链 VM 模拟器* for extensive and exhaustive testing
*   A wrapper to a minimal local blockchain node infrastructure to simplify deployment
*   A sandboxed I/O mode to enforce security on devices running dapps

The complete `Red/C3` stack will be about 100x smaller than the web stack currently used for dapps (+- 1 MB compared to 120 MB). This compactness will make deployment of dapps much easier than it currently is with existing tools.

Among the first dapps to be built will be a Red wallet and a community chat.

## Red 的路线图

What does the future bring for Red? Its roadmap can be found at [`www.red-lang.org/p/roadmap.html`](https://www.red-lang.org/p/roadmap.html) and in more detail at [`trello.com/b/FlQ6pzdB/red-tasks-overview.`](https://trello.com/b/FlQ6pzdB/red-tasks-overview)

After the 0.6.4. GUI console release, the main focus will be on the 0.7.0 release, which will provide full I/O and networking support, some features of which will certainly be needed in the `Red/C3` toolchain. Android support is coming along well (for a demo, see [`www.red-lang.org/2018/04/sneak-peek-at-red-on-android.html`](https://www.red-lang.org/2018/04/sneak-peek-at-red-on-android.html)), and is planned for the 0.7.1 release.

The next big milestones before the 1.0 release are as follows:

*   **0.8**: Support for modules, to facilitate working on bigger and more complex Red projects
*   **0.8.5**: Integrating an advanced garbage collector in the Red runtime
*   **0.9**: Concurrency support, initially like coroutines, before evolving into a full-concurrent actor model

Another option is to declare 0.7.1 the last release before 1.0, moving major development portions to come after that release.

Red's toolchain is now still partly written in REBOL 2. Red will become truly self hosted when Red and its toolchain are completely rewritten in Red, which will also lead to faster compilation.

## 一些有用的参考

Red is still a young project, so its documentation lacks depth and examples. However, Red's popularity is growing rapidly, so this situation is expected to improve in the near future.

A number of documentation links can be found at [`www.red-lang.org/p/documentation.html`](https://www.red-lang.org/p/documentation.html), among which is the official Red documentation, also found at [`doc.red-lang.org/`](https://doc.red-lang.org/) (which is still under construction).

The following websites can help you out when you need more examples or explanations:

*   The Red By Example site ([`www.red-by-example.org/index.html`](http://www.red-by-example.org/index.html)), maintained by Arie Van Wingerden and Mike Parr
*   The Red Beginners Reference Guide at [`www.mycode4fun.co.uk`](http://www.mycode4fun.co.uk), maintained by Alan Brack
*   Red documentation in the form of a Notebook at [`helpin.red/`](http://helpin.red/), by André Ungaretti
*   The Red Programming site at [`redprogramming.com/Home.html`](http://redprogramming.com/Home.html) by Nick Antonaccio

An ever-growing collection of links to articles on specific topics and discussions can be found on GitHub at [`github.com/red/red/wiki`](https://github.com/red/red/wiki). I particularly recommend Gregg Irwin's Primer, which is found at [`github.com/red/red/wiki/Primer`](https://github.com/red/red/wiki/Primer).[](https://github.com/red/red/wiki/Primer)

A growing collection of Red tools can be found at [`github.com/rebolek/red-tools`](https://github.com/rebolek/red-tools). To explore concrete projects made in Red, visit [`github.com/red/code/`](https://github.com/red/code/tree/master/Library).[](https://github.com/red/code/tree/master/Library)

If you have questions, ask them on Stack Overflow at [`stackoverflow.com/questions/tagged/red`](https://stackoverflow.com/questions/tagged/red), or, for an even faster response, try Red's many Gitter channels at [`gitter.im/red/home`](https://gitter.im/red/home).

## 摘要

In this chapter, you got a glimpse of some advanced features that Red has to offer. We saw how powerful reactive programming techniques make it possible to link two or more objects, so that if one changes, the others change with it. You got an introduction to `Red/System`, the lower layer of the Red platform, and learned how to incorporate its power to improve performance or to do low-level coding from within your Red apps. We captured command-line arguments for our script, and used `call` to start other programs. We learned how to use the `vector!`, `hash!`, and `map!` datatypes for performance when working with number series and dictionaries. At the end, you got a glimpse of the (near) future of Red's development, and you acquired a collection of the best links to turn to if you need more help.



我们现在已经走到了 Red 之旅的终点。我希望这次快速概览能让你明白 Red 为何在软件世界中成为冉冉升起的明星，也希望你能在自己的项目中使用它。祝你在 Red 宇宙中再见！

# 问题

1. 绘制一个 10 x 10 大小的 `base` 面，并使用 `loose` 细节使其可拖动。添加一个文本字段，使用 `react` 显示该框的 `offset`，以便在拖动 `base` 时文本发生变化。

2. 面的颜色可以指定为 `face/color: as-color r g b`，其中 `r`、`g` 和 `b` 分别是颜色的红色、绿色和蓝色分量（它们是 `0` 到 `255` 之间的整数）。另外，你也可以使用 `face/color/1` 表示红色，`/2` 和 `/3` 表示绿色和蓝色。
编写一个程序来调整白色 base 面的颜色，通过改变三个滑块来改变 `r`、`g`、`b` 的每个值。如果这可行，请再编写一个响应式版本。注意响应式代码要简单得多。

3. 检查 `ch10/hello.reds` 源文件中的 Red/System 代码。编译并执行它。

4. 编写一个在 `Red/System` 中打印的 Red/System 函数。从一个 Red 脚本调用它，该脚本在包含 Red/System 代码之前和之后都写上 `in Red`。解释输出结果。如果使用 `#system` 块而不是 `#system-global`，你可以直接在其中调用 Red/System 函数。试试看。

5. 创建一个 `map!`，其字符串键仅在大小写上不同。尝试使用 `select` 和 `put`，有无 `/case` 修饰符的情况。

# 进一步阅读

官方关于响应式编程的文档可以在 [`doc.red-lang.org/en/reactivity.html`](https://doc.red-lang.org/en/reactivity.html) 找到。[](https://doc.red-lang.org/en/reactivity.html)

你可以在 [`static.red-lang.org/red-system-specs-light.html`](https://static.red-lang.org/red-system-specs-light.html) 找到 Red/System 的详细规范。

# 评估

## 第一章

1. `Red/System` 是 Red 中用于进行类似 C 的底层编程的部分。

2. 全栈语言是指可以在编程的整个光谱上使用的语言，从系统级到高级语言构造。`Red` 与 `Red/System` 能够做到这一点。

3. `Red` 和 `LISP` 都以块（或列表）作为其基本结构，用于代码和数据。

4. 1) `Red` 具有非常小的工具链，无需安装和配置
2) 其可执行文件的大小约为 1 Mb
3) 其源文件大小减少。

5. 方言是用 `Red` 编写的专用高级语言。示例：`parse`、`view`、`Red/C3`

6. `Red` 编译为原生代码，而 `REBOL` 始终是解释执行的。

7. `Red` 在 Windows、Linux、Android、OS X 和 FreeBSD 环境中以 32 位版本运行，也可在 ARMv5 处理器上运行。

## 第二章

1. 参考 `exercise1.red`

2. `red.r` 是当前用于将 Red 编译为 Red/System，以及将 Red/System 编译为原生代码的 REBOL 脚本

3. 自托管语言的编译器是用自身编写的（在 `Red` 的情况下，用 `Red` 编写）

4. `do` 函数

5. 在发布模式下不需要 `libRedRT`

6. `libRedRT` 的编译代码包含在程序的可执行文件中

7. 带有 `Red` 插件的 Visual Studio Code 为你提供代码补全

## 第三章

1. 它会给出 *** Script Error: John has no value.。修正为：`name: "John"`

2. 参见正文中的解释

3. `i1 ;== 1 i2 ;=`
重点是 `(i1: 1)` 本身的值为 1，并且其效果是 `i1` 从此以后引用 1。

4. `print \[10 \* 5\] ;== 50`
`probe \[10 \* 5\] ;== \[10 \* 5\]`

5. `to\_float 42` *** Script Error: to\_float has no value.
`to\_float` 不是一个已知的词。

6. `Chapter03/evaluation.red`:
```
print (6 < 13) and (42 < 33)   ;== false
print (43 < 42) xor (44 < 43)  ;== false
print complement 3             ;== -4
```

7. `a: "red" ;== "red"`
`b: copy a ;== "red"`
`a = b ;== true ;same value`
`a == b ;== true ;same value and datatype`
`a =? b ;== false ;not the same object!`

8. `print \[3 = 2 tab 5 = 5 tab 1 = 1.0\] ; == false true true`

9. `on = (5 = 5) ;== true`

10. `? now`
```
REFINEMENTS:
/year => Returns year only.
/month => Returns month only.
/day => Returns day of the month only.
/time => Returns time only.
/zone => Returns time zone offset from UCT (GMT) only.
/date => Returns date only.
/weekday => Returns day of the week as integer (Monday is day 1).
/yearday => Returns day of the year (Julian).
/precise => High precision time.
/utc => Universal time (no zone).
```

11. `square-root 9 + square-root 9`
与以下相同：`square-root (9 + square-root 9)`
与以下相同：`square-root (9 + 3)`
与以下相同：`square-root (12) ;== 3.464101615137754`
`(square-root 9) + square-root 9 ;== 6.0`

12. `minimal.red`
```
–c compilation:  66kB
–r compilation:  583 kB
```

## 第四章

1. 参见 `testing-conditions.red` QA 1

2. `out ;== "bad luck"`

3. 参见 `raining.red`

4. 参见 `testing\_conditions.red` ; QA 4

5. 参见 `testing\_conditions.red` ; QA 5

6. 参见 `repetitions.red` ; QA 6

7. 参见 `count-down.red`

## 第五章

1. `data: \["A" "B" "C" "D"\]` 是一个字符串序列，以下是一个字符序列：`data: \[#"A" #"B" #"C" #"D"\]`

2. 参见 `navigating-and-looping.red` 部分；移动到尾部。

3. 参见 `navigating-and-looping.red` 部分；返回。

4. 参见 `navigating-and-looping.red` 部分；跳过。

5. 参见 `navigating-and-looping.red`；QA 5。

6. 参见 `getting-info.red` 部分；选择一个项目

7. 参见 `getting-info.red` 部分；QA 7

8. 参见 `getting-info.red` 部分；QA 8

9. 参见 `getting-info.red` 部分；QA 9

10. 参见 `getting-info.red`；QA 10

11. 参见 `changing-series.red`；QA 11

12. 参见 `strings-as-series.red`；QA 12

## 第六章

1. 参见 `do-error-runtime.red` 以了解错误说明以及何时会出现它。

2. 参见 `do-does-has-func.red` 部分；QA 1

3. 参见 `do-does-has-func.red` 部分；QA 2？
在 `has1` 中定义的变量 `num` 成为全局变量，而 `has2` 将其 `num2` 参数转换为局部变量。

4. 参见 `do-does-has-func.red` 部分；QA 4

5. 参见 `function-atrributes.red` 部分；QA 5

6. 参见 `function-atrributes.red` 部分；QA 6

7. 参见 `working-with-functions.red` 部分；QA 7

8. 参见 `working-with-functions.red` 部分；QA 8
使用 `n \[integer!\]` 限制 `n` 的类型也是可以的，但会引发异常。
要测试 `if n = 0`，你也可以写成 `if zero? n`

9. 参见 `working-with-functions.red` 部分；QA 9

10. 参见 `objects.red`；QA 10

11. 参见 `objects.red` QA 11

## 第七章

1. 参见 `reading-and-writing-files.red`，部分 QA 1：每个项目都写在不同的行上。

2. 同 QA 2

3. 同 QA 3

4. 参见 `saving-and-loading.red` QA 4：加载后块结构得以恢复。

5. 参见 `saving-and-loading.red` QA 5

6. 参见 `currency-rates3.red`

## 第八章

1. 参见 `bitsets.red` 部分 QA 1

2. 参见 `bitsets.red` 部分 QA 2

3. 参见 `bitsets.red` 部分 QA 3

4. 参见 `how-parse-works.red` 部分 QA 4

5. 参见 `how-parse-works.red` 部分 QA 5

6. 参见 `searching-and-changing.red` QA 6

7. 参见 `searching-and-changing.red` QA 7

8. 参见 `searching-and-changing.red` QA 8

9. 参见 `searching-and-changing.red` QA 9

10. 参见 `more-features.red` QA 10

## 第九章

1. 参见 `currency-rates-gui2.red`

2. 参见 `faces-are-objects.red`

3. 参见 `close-window-quit.red` 和 `close-window-unview.red`

4. 参见 `container-layout.red` 部分 QA 4

5. 参见 `button-check.red`

6. 参见 `sorted-text-list.red`

7. 参见 `extracted-text-list.red`

8. 参见 `text-hover-wheel.red`

9. 参见 `drawing-lines.red`

## 第十章

1. 参见 `reactive-drag.red`

2. 非响应式版本是 `box-color-non-reactive.red`，响应式版本是 `box-color-reactive.red`




3. 参见 `hello.reds`

```
      Compile it with: red –r hello.reds
      To execute:     ./hello
```

4. `print-test.red` 调用 `print-test.reds`

5. 参见 `perf-datatypes.red`，第 QA 5 节

# 你可能会喜欢的其他书籍

如果你喜欢这本书，你可能也会对 Packt 出版的其他这些书籍感兴趣：

![](https://www.packtpub.com/application-development/rust-programming-example)

**通过示例学习 Rust 编程**

Guillaume Gomez, Antoni Boucher

ISBN: 978-1-78839-063-7

* 使用 Cargo-Rust 包管理器编译和运行 Rust 项目
* 使用 Rust-SDL 功能，例如事件循环、窗口、无限循环、模式匹配等
* 使用 Gtk-rs 和 Rust-SDL 创建图形界面
* 结合并发机制和多线程，同时包含线程安全和锁
* 使用 Tokio 库中的异步 I/O 栈实现 FTP 协议

![](https://www.packtpub.com/application-development/rust-standard-library-cookbook)

**Rust 标准库食谱**

Jan Nils Ferner, Daniel Durante

ISBN: 978-1-78862-392-6

* 如何使用库的基本模块：字符串、命令行访问等
* 使用向量、Deque、链表等实现集合和集合折叠
* 处理各种文件类型，压缩和解压缩数据
* 使用 glob 模式搜索文件
* 实现通过 CSV、TOML 和 JSON 等各种格式的解析
* 使用 drop trait，即 Rust 版本的析构函数
* 使用 Bilocks 进行资源锁定

# 留下评论 - 让其他读者知道你的想法

请通过在你购买该书的网站上留下评论，与他人分享你对这本书的想法。如果你是在 Amazon 购买的书，请在此书的 Amazon 页面上为我们留下诚实的评论。这一点至关重要，这样其他潜在读者才能看到并利用你的公正意见来做出购买决定，我们可以了解客户对我们产品的看法，而我们的作者也能看到你对他们与 Packt 合作创作的书名的反馈。这只会占用你几分钟的时间，但对其他潜在客户、我们的作者和 Packt 来说都很有价值。谢谢！
