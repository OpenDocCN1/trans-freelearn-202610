
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可以在 `blocks` 中组合，`blocks` 由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 上的图形化 REPL 中，可以按此布局复制粘贴。在 Linux 上，目前只有控制台 REPL，因此格式不会那么美观）。

你可以使用 `to-block` 将任何参数转换为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可以包含 `code instructions`，如下所示：

```
loop 10 [prin "hello" print " world"]
```

在下面的代码片段中，第一个块 `\[site action\]` 包含单词，这些单词必须具有值，而第二个块包含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

如你所见，代码和数据几乎没有区别；这将在后面的章节中变得更清晰。块本身不会求值，只有在用 `do` 调用时（参见 `Evaluation with do and reduce` 部分）或在代码上下文中才会这样做。

在大多数情况下，块中值或单词的 `order` 很重要——块是 `series` 的一个例子。

当然，块可以包含其他块，例如：`\[ \[125 12\] \[256 26\] \]`

最小的 Red 脚本是 `Red \[\]`，或如下所示：

```
Red [
]
```

它什么都不做，但包含一个必需的头部块，正如我们稍后会看到的那样，它可以包含有关脚本的信息。事实上，每个脚本文件本身也是一个块——它不包含括号，但隐含了一个包含整个源代码的块。

我们尚未提及的另一种数据类型是 `paren!`。与 `block!` 是方括号 `\[\]` 内的任何系列一样，`paren!` 是圆括号 `()` 内的任何系列，例如 `(1 + 2)`。与块不同，paren 总是自行求值——`(1 + 2) ;== 3`。

Red 有一个名为 `quote` 的函数，其特殊之处在于它不会对其参数求值。使用它，我们可以通过询问 `type?` 来显示 `paren!` 类型（我们将在 `Word bindings` 部分进一步讨论）：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

在程序中穿插着注释，注释是用于向人类解释代码的文本，Red 会忽略它。注释在行首或行中分号 (`;`) 之后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释形成如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节开始，我们将在以 `;==` 开头的注释中在同一行显示单词的返回值，如此——`;== value`。然而，`;==` 也是 REPL 显示返回值的[content]
```
integer? -42        ;== true
logic? off          ;== true
date? 20-4-2018     ;== true
date? 2018-44-18    ;*** Script Error: cannot MAKE/TO date! from: [day month year]
```

# 使用块和括号

单词和任何类型的值都可组合在 `blocks` 中，由方括号 `\[ \]` 限定。块的类型为 `block!`。下面是一个包含 `data` 的块：

```
;-- Chapter03/blocks.red:
[
    "Jeff" Jeff@amz.com #213-555-1010
    "Bill" billg@ms.com #315-555-1234
    "Steve" jobs@apl.com #408-555-4321
]
```

（在 Windows 和 macOS 的图形 REPL 中可复制粘贴此布局。Linux 目前只有控制台 REPL，格式不佳）。

可用 `to-block` 将任意参数转为块——`to-block {1 2 3} ;== \[1 2 3\]`。

块也可包含 `code instructions`，例如：

```
loop 10 [prin "hello" print " world"]
```

下面代码中，第一个块 `\[site action\]` 含需赋值的单词，第二个块含代码：

```
foreach [site action] sites [
    data: read site
    do action
]
```

代码与数据几乎无差别，后续章节会更清晰。块本身不求值，仅在 `do` 调用或代码上下文中求值，见 `Evaluation with do and reduce`。

通常块中值或单词的 `order` 很重要——块是 `series` 的一种。

块可嵌套，例如：`\[ \[125 12\] \[256 26\] \]`

最小 Red 脚本为 `Red \[\]`，或：

```
Red [
]
```

它不执行任何操作，但包含必需的头部块，可存放脚本信息。实际上每个脚本文件本身也是块，只是隐含了包含全部源码的块。

未提及的数据类型还有 `paren!`。`block!` 是 `\[\]` 内的系列，`paren!` 是 `()` 内的系列，如 `(1 + 2)`。与块不同，paren 总是自求值——`(1 + 2) ;== 3`。

Red 有 `quote` 函数，其特殊之处是不求值其参数。用它可显示 `paren!` 类型，通过 `type?` 查询，后续在 `Word bindings` 讨论：

```
quote (1 + 2)        ;== (1 + 2)
type? quote (1 + 2)  ;== paren!
```

# 注释

程序中可插入注释，用来向人解释代码，Red 会忽略。注释在行首或行中分号 (`;`) 后开始：

```
;-- see Chapter03/comments.red:
; single-line comment
print "start" ; comment on a code line
```

多行注释如下：

```
comment {
    This is a multiline comment.
    This explains how this program works.
}
```

从下一节起，我们在以 `;==` 开头的注释同一行显示单词返回值，如 `;== value`。` ;==` 也是 REPL 显示返回值的方式，应熟悉。

可用多个单行注释组成多行注释，Visual Studio Code 有菜单 Edit / Toggle Line Comment，也可用 `;--` 增强视觉提示。尽量用描述性名称避免注释，并用文档字符串代替注释，后续在 [第6章](#5c76066d-61d4-4dbd-b5db-902ae9e9991c.xhtml) `Using Functions and Objects` 介绍。

# Word bindings

Red 程序中可存在多个 `contexts`。上下文是单词可定义的作用域，即 `a word is bound to a context`。例如函数上下文中局部单词 `price`，另一函数中同名 `price` 含义不同。

上下文还可在块内或整个脚本/环境内，即 `global context`。例如在 Red 控制台输入 `type? chocolate` 会打印：

```
No matching values were found in the global context.
```

在上下文中 `word refers to a value,` 如：

```
;-- see Chapter03/bindings.red:
age: 62
is-integer?: false
lunch-time: 12:32
birth-day: 17-Jan-1997
friends: ["Jeff" "Paula" "Viviane"]
```

上述单词绑定到全局上下文并引用对应值。例如 `age` 引用整数 `62`，`friends` 引用块值 `\[ "Jeff" "Paula" "Viviane" \]`。

`a word` `var` 引用值 `val` 的通用语法为：

```
var: val
```

末尾无需终止符，但 `:` 与值间需空格。

注意 `:` 不是赋值运算符，只将前面的单词绑定到 `:` 后表达式的结果。Red 中 `var:` 类型为 `set-word!`。

`var` 返回值，`:var` 即 `getting the value of the word` `var`，效果相同，如 `age`：

```
age     ;== 62  <-- remember: this comment shows the value of age
:age    ;== 62
```

`:var` 类型为 `get-word!`。

可读性允许时可一行绑定：

```
n: 12   m: 42
```

一行可设多个变量为同一值：

```
age: number: size: 62
```

也可 `set` 结合块使多个单词同值：

```
set [log-time start-time run-time] 10:57  ;== 10:57:00
start-time                                ;== 10:57:00
log-time                                  ;== 10:57:00
run-time                                  ;== 10:57:00
```

`age`、`is-integer?`、`lunch-time`、`birth-day`、`friends` 都是 `words.` 它们是 Red 中最接近其他语言变量的东西，故有时称变量。但需记住它们是 `words that refer to values`。

检查单词是否有值用 `value?`：

```
value? age  ;== true
```

⇒ 现在回答 `Questions` 部分的第 3 个问题。

用 `print var` 或 `prin var` 输出变量 `var` 的格式化值，有无换行均可。用 `probe var` 查看原始类型表示，调试有用，`?` 和 `??` 亦然：

```
print friends ;== Jeff Paula Viviane
probe friends ;== ["Jeff" "Paula" "Viviane"]
? friends     ;== FRIENDS is a block! value. length: 3 ["Jeff" "Paula" "Viviane"]
?? friends    ;== friends: ["Jeff" "Paula" "Viviane"]
```

⇒ 现在回答 `Questions` 部分的第 4 个问题。

另提两个相关函数：

*   `form` 返回用户友好的字符串表示，如：

```
form friends        ;== "Jeff Paula Viviane"
```

它从块生成 `regular text`，多用于字符串处理。

*   `mold` 返回源格式多行字符串表示：

```
mold friends        ;== {["Jeff" "Paula" "Viviane"]}
```

基本用途是生成可保存并重载以复原原值的 Red 文本。

输出多部分时常与块一起用 `print`：

```
name: "John"
print ["My name is:" name]     ;== My name is: John
```

各部分自动加空格。某些字符有专用名，如 `tab`、`newline` 也可用字符 `#"^/"`、`lf` `linefeed` 等。

要知变量类型，用 `type?`：
