
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

用其他语言编写的软件可以通过 `libRed` 与 `Red` 交互，`libRed` 是 `Red` 解释器和运行时库的特殊版本。这样，例如 `C` 程序就可以与 `Red` 环境交互。还存在 `Visual Basic`、`Ruby` 和 `Julia` 的绑定（[https://github.com/joa-quim/Red.jl](https://github.com/joa-quim/Red.jl)）。详细文档位于 [https://doc.red-lang.org/en/libred.html](https://doc.red-lang.org/en/libred.html) 和 [https://github.com/red/red/wiki/libRed](https://github.com/red/red/wiki/libRed) [更多示例。](https://github.com/red/red/wiki/libRed)

## 宏

许多现代编程语言都有的另一个特性是宏。宏是关于在运行时转换代码。到现在为止，你应该已经确信 `Red` 已经能够以最小的努力做到这一点；想想你用 `do`、`reduce` 和 `load` 能做什么。毕竟，代码即数据，数据即代码！因为 `Red` 首先是一种编译语言，所以 `Red` 中的宏更倾向于在编译时转换代码，但在解释时同样有效。以下是一个宏示例，供你一窥究竟：

```
;-- see Chapter10/macros.red:
#macro as-KB: func [n][n * 1024]

print as-KB 64 ;== 65536   
```
