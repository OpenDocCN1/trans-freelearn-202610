
Both XML and JSON, as well as other formats, can be handled quite easily with the Red `parse` dialect. In the next chapter, we'll analyse this data to extract the relevant information.

# 摘要

在本章中，我们学习了如何从 REPL 以及 Red 代码内部处理文件和文件夹。我们看到了如何交互式地向用户请求文件或文件夹，以及 `read` 和 `write` 文件是多么容易，即使是二进制格式或来自互联网。在处理代码时，更推荐使用 `save` 和 `load`。我们以下载货币汇率数据结束本章，该数据将在下一章中进行处理。

# 问题

1. 使用 `/lines` 将 `contacts` 系列的内容写入 `contacts3.txt` 文件。有什么不同？
2. 尝试使用 `20` 作为参数的 `write/part`。它做了什么？
3. `write/seek %file new-text n`（其中 `n` 为数字）用于从位置 `n`+1 开始替换 `%file` 中的文本。针对 `contacts` 系列尝试此操作，并将第一项中的 `John` 替换为 `Adam`。
4. 使用 `save` 和 `load` 将我们的 `contacts` 系列存储到文件中并读取。
5. 使用 `copy ""` 重写 `appin` 函数，并多次调用它以查看差异。
6. 编写一个脚本，询问用户感兴趣的货币和格式类型（XML 或 JSON）。这里，你必须使用 [`http://www.floatrates.com/daily/USD.xml`](http://www.floatrates.com/daily/USD.xml) 格式，该格式不使用查询字符串参数。尝试通过获取 JSON 格式的数据来测试。提示：使用 `probe` 来查看你的 URL 是否正确！

# 解析数据

当今软件开发的一个关键重点是处理和分析（大型）数据流。每种编程语言都需要对这类任务提供某种支持。大多数语言使用 `regular expressions`（简写为 `regex`（[https://www.regular-expressions.info/examples.html](https://www.regular-expressions.info/examples.html)））来执行此类任务。这些表达式使用专门的正式语法匹配字符串数据中的模式，这通常显得像是代码中笨拙的附加部分。

Red 采用不同的方法。虽然 Red 提供了处理数据所需的所有系列操作，但它还拥有自己的 `domain-specific language`（`DSL`）或称为 `parse` 的 `dialect`，专门用于文本和数据处理。解析引擎在 `Red/System` 中实现。它有自己特定的单词和语法，但在其他 Red 代码中嵌入时感觉非常自然。Parse 代码本身比 regex 更易于人类阅读和维护。它更强大，不仅限于字符串解析，而且性能更好。

在本章中，我们将探讨如何使用 `parse`，它是任何 Red 开发人员不可或缺的工具。

以下主题将被涵盖：
* `bitset!` 数据类型
* `parse` 的工作原理
* 搜索、提取和更改
* 更多功能和示例

## 技术要求

你可以在 [`https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter08`](https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter08) 找到本章的代码。如果已按照 [第 2 章](#e7b0526e-a6dd-4c51-abc8-81b1dfdc8460.xhtml) 中所述安装了 Red，*Setting Up for Development*，你就可以开始了。你可以在 Windows、macOS 或 Linux 上工作。你可以在 Red 控制台中输入或粘贴任何代码以查看其结果，或使用 Visual Studio Code 等编辑器开始使用。

## `bitset!` 数据类型

该数据类型专门设计，使 `parse` 能够高效地处理由 Unicode 值组成的字符串。它用于将任意字符集存储为布尔值，即作为一组位。

`bitset!` 类型的值使用 `make bitset!` 创建，接受字符、字符串、整数（在 Unicode 代码点范围内）或这些值的系列，例如：

```
;-- see Chapter08/bitsets.red:
make bitset! #"R"              ;== make bitset! #{0000000000000000000020}
make bitset! "Red"             ;== make bitset! #{0000000000000000000020000C}
make bitset! [108 "Red" #"R"]  ;== make bitset! #{0000000000000000000020000C08}
```

请注意，bitset 值以十六进制格式表示。一个方便的快捷函数 `charset` 执行相同的操作。因此，例如，最后一条语句也可以写成 `charset [108 "Red" #"R"]`（比较 bitset 二进制值可知它们相等）。

现在我们可以创建一个 bitset 来存储所有元音：

```
vowel: charset "aeiou" ;== make bitset! #{000000000000000000000000444104}
```

我们可以利用它高效地搜索字符串中的第一个元音，如下所示：

```
str: "dog"
find str vowel     ;== "og"
```

（请记住，这里 `find` 返回从匹配值开始的系列。）

以下是存储所有数字的 charset：

```
digit: charset "0123456789"
```

另一种表示方法是使用 `range`，在两个字符或两个整数值之间使用 `\-`：

```
digit: charset [#"0" - #"9"]
```

=> 现在回答 `Questions` 部分的问题 1。

如果你找到了这个问题的答案，这里是如何存储所有字母。我们可以以两种方式完成。第一种是：

```
letter: charset [#"a" - #"z" #"A" - #"Z"]
```

我们也可以使用 `union` 词：

```
letter: union lower upper
```

其中 `lower` 和 `upper` 分别定义为：

```
lower: charset [#"a" - #"z"]
upper: charset [#"A" - #"Z"]
```

=> 现在回答 `Questions` 部分的问题 2 和 3。

其他集合函数也可以使用。

以下 `ws` bitset 存储空白字符——`ws: charset reduce [space tab cr lf]`。使用 `not` 来创建补集 bitset——`non-digit: charset [not "0123456789"]`。

要创建一个内存分配为 100 位的空 bitset，请执行以下操作：

```
make bitset! 100 ;== make bitset! #{00000000000000000000000000}
```

这是因为大小会四舍五入到 8 的倍数，并且 104 个字符有 26 个十六进制数字。

bitset 可以通过使用 `append` 来扩展，如以下示例所示：

```
bs1: make bitset! "A"   ;== make bitset! #{000000000000000040}
append bs1 "Z"          ;== make bitset! #{000000000000000040000020}
```

`bs1` 的内存分配会自动扩展到其新大小。

## 理解 parse 的工作原理

`parse` 的工作原理相当直接。它是一个函数，接受要解析的输入，以及用于解析输入的规则集——`parse input [rules]`

输入是需要处理的数据；它可以是 `any series` 值。在大多数情况下它是 `a string`，可能来自网页、XML 或 JSON 或其他类型的文件，或电子表格，但它也可以是包含值的块、二进制值，甚至是代码。

Parse 搜索输入并尝试从头到尾使用规则对其进行模式匹配。如果完整的输入直到末尾都匹配规则，则返回 `true`；如果不匹配，则返回 `false`。事实上，规则块可以是一个完整的子程序，从输入中提取信息并进行处理：

![](assets/737f007b-8aa2-4cde-946c-06c94f4b9fe8.png)

### 第一个 parse 示例

让我们从一些简单示例开始，其中我们有一个产品代码，以字母组合 `XY` 开头，后跟一个数字，我们在上一节中将其定义为 bitset `digit`，例如 `"XY6"`。我们可以使用规则块 `["XY" digit]` 解析此输入：

```
;-- see Chapter08/how-parse-works.red:
parse "XY6" ["XY" digit]      ;== true
```

其工作原理如下。从左到右查看 `"XY6"`，找到 `"XY"`，如果找到，则找到一个数字。如果找到并且我们在输入的末尾，则返回 `true`。但在代码 `"XY67"` 中有两个数字。未到达输入的末尾，parse 返回 `false`：

```
parse "XY67" ["XY" digit]    ;== false
```

我们可以通过指定数字的数量使其匹配，如下所示：

```
parse "XY67" ["XY" 2 digit] ;== true
```
