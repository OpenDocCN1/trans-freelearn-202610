
将字符串转换为文件使用 `to-file` 完成，但仅语法发生变化，并未进行其他检查。

如果需要将文件路径转换为特定平台的表示形式（如 Windows），请使用 `to-local-file` 函数，例如如下所示：

```
;-- see Chapter07/working-with-files-directories.red:
to-local-file %/E/Red/red.bat    ;== "E:\Red\red.bat"
```

相反，要将特定路径表示法转换为平台无关格式，请使用 `to-red-file`，如下代码所示：

```
to-red-file "E:\Red\red.bat"    ;== %/E/Red/red.bat
```

前述两个函数都有 `/full` 修饰符，可在需要时将文件路径转换为绝对路径。

```
to-local-file/full %red.bat     ;== "E:\Red\red.bat"
```

请注意，如果将路径用 `""` 包围，Red 会自动将使用 `\\` 的平台特定路径转换为标准表示法：

```
write %"C:\ProgramData\Red\test.txt" "test"
```

以下是一些方便函数的简要概述，供您参考：
* `pwd` 和 `what-dir` 都返回当前工作文件夹，初始时为当前程序执行的所在文件夹——例如 REPL——`pwd ;== %/C/ProgramData/Red/`，`get-current-dir` 以字符串格式返回相同的值——`"C:\\ProgramData\\Red"`。
* `cd` 和 `change-dir` 都会更改当前目录，并随之更改上一条要点中函数的返回值——`cd %/E/test`，`;== %/E/test/`，且 `pwd ;== %/E/test/`。
* `dir`、`ls` 和 `list-dir` 这些词都会给出当前或指定文件夹的内容。
* 字符 `file?` 和 `dir?` 分别测试其参数是否为文件或文件夹：
```
file? %/E/Red/red.bat ;== true
dir? %/E/Red/         ;== true (trailing / is needed)
```
* 短语 `make-dir` 创建一个新文件夹，如果已存在则不会报错：`make-dir %scripts`。
* 要测试文件或文件夹是否真正存在，请使用 `exists?`：
```
exists? %/E/Red/red.bat    ;== true
exists? %/E/Red/red2.bat   ;== false
exists? %/E/NotExist/      ;== false
```
* `suffix?` 短语返回文件的扩展名——`suffix? %/E/Red/red.bat ;== %.bat`。
* `size?` 短语返回文件的大小（字节），如果不存在则返回 `none`——`size? %red.bat ;== 22`。
* 要删除文件，只需使用 `delete %file`。成功时返回 `true`。

# 读写数据文件

`read` 和 `write` 词是 Red 词通用工作方式的良好示例。它们用于读写各种文件（文本和二进制数据），但也可以与网络端口或 URL 一起使用（请参阅本章后面的股票报价示例）。如果你在其他语言中使用过文件，会欣赏它们易于使用；在使用 Red 时，无需以特定模式打开文件然后再关闭！

让我们使用与我们在 [第5章](#35fe4037-dd49-406f-b90b-217052b1d33f.xhtml) 中的 `contacts` 序列示例完全相同的数据，*使用序列和块*：

```
;-- see Chapter07/reading-and-writing-files.red:
contacts: [
     "John Smith" "123 Tomline Lane Forest Hills, NJ" "555-1234"
     "Paul Thompson" "234 Georgetown Pl. Grove, AL" "555-2345"
     "Jim Persee" "345 Pickles Pike Orange Grove, FL" "555-3456"
     "George Jones" "456 Topforge Court Mountain Creek, CO" ""
     "Tim Paulson" "" "555-5678"
]
```

我们第一个联系人的数据如下：

```
name: "John Smith"
address: "123 Tomline Lane Forest Hills, NJ"
phone: "555-1234"
```

让我们将其写入名为 `contacts1` 的文件。一般语法是 `write %filename data`，其中 `data` 可以是任何类型。但是，每次写入同一文件时，都会覆盖之前的内容。如果使用 `/append`，数据会追加到文件末尾：

```
write %contacts1 name
write/append %contacts1 address
write/append %contacts1 phone
```

文件写入 `pwd` 指定的文件夹。如果需要将其存储在不同文件夹中，请在文件名中使用完整的文件路径（相对或绝对）——例如 `%/E/Red/Tests/contacts1`。诸如 `.txt` 的后缀不是必需的，但如果需要可以添加。

如果在编辑器中打开 `contacts1`，你会看到 `John Smith123 Tomline Lane Forest Hills, NJ555-1234`。使用 Red 读取此文件会将内容作为一个字符串返回：

```
read %contacts1   ;== {John Smith123 Tomline Lane Forest Hills, NJ555-1234}
```

如果尝试读取不存在的文件，例如 `read %contacts99`，你会得到 `*** Access Error: cannot open: %contacts99` 错误。

不同字段现在无法再分隔。让我们将所有数据写入一行，用分号（`;`）分隔每个字段（我们也可以选择任何想要的分隔符）：

```
write %contacts1 rejoin [name ";" address ";" phone newline]
```

`contacts1` 文件现在包含 `John Smith;123 Tomline Lane Forest Hills, NJ;555-1234`。

现在我们想将所有 `contacts` 数据存储在文本文件 `contacts2` 中，每个联系人占一行。我们可以通过遍历序列、模式匹配字段并将每个联系人写入一行来实现，如下所示：

```
foreach [name address phone] contacts [
    write/append %contacts2 rejoin [name ";" address ";" phone newline]
]
```

要将此数据读回，我们可以使用 `contents: read %contacts2`，但此时 `contents` 字符串包含整个文件。我们必须先对换行符使用 `split`（请参阅 [第5章](#35fe4037-dd49-406f-b90b-217052b1d33f.xhtml)，*使用序列和块*）来分隔联系人，然后对 `;` 使用 `split` 来获取每个联系人的单独字段。

但我们可以使用以下方法将每个联系人作为单独的字符串取回，所有内容都收集在一个块中：

```
contents: read/lines %contacts2
```

这给出了以下结果：

```
;== [{John Smith;123 Tomline Lane Forest Hills, NJ;555-1234} {Paul Thompson;234 Georgetown Pl. Grove, AL;555-2345} {Jim Persee;345 Pickles Pike Orange Grove, FL;555-3456} {George ...   ]
```

如果你只想将数据块写入文件，只需一行——即 `write %contactsw contacts`——此时 `contactsw` 文件包含以下内容：

```
[
     "John Smith" "123 Tomline Lane Forest Hills, NJ" "555-1234"
     "Paul Thompson" "234 Georgetown Pl. Grove, AL" "555-2345"
     "Jim Persee" "345 Pickles Pike Orange Grove, FL" "555-3456"
     "George Jones" "456 Topforge Court Mountain Creek, CO" ""
     "Tim Paulson" "" "555-5678"
 ]
```

要将序列中的每个项目写入文件的不同行，请使用 `write/lines`，如果需要向现有文件添加内容，请使用 `write/lines/append`。请在 *问题* 部分的第 1 题中尝试此操作。顺便也做第 2 题和第 3 题。

使用 `read %.` 将当前文件夹中的所有文件和文件夹名称存储在一个块中，返回值类似于 `[%apps/ %articles/ %benchmarks/ %blockchain/ %books/ %build/ %cinfo %contactss ...]`。

# 处理二进制文件

如果处理的文件仅包含字节（不可读文本），如多媒体文件（图片、音频、电影），请添加 `/binary` 修饰符，如下例所示，其中 `png` 图像加载到临时变量 `tmp` 中：

```
tmp: read/binary %red.png

;== #{
89504E470D0A1A0A0000000D49484452000002EE000002ED08060000007789E1
64000078C34944415478DAECDD079C645599F7F1E739F75655E7389DD3040686
0C8A202220028AB8200883AEBB20418220023A8489...}
```

现在我们将 `tmp` 写入另一个文件 `red-copy.png`：

```
write/binary %red-copy.png tmp
```

为了更好地遵循 Red 的惯用写法，消除 `tmp` 并直接写入——`write/binary %red-copy2.png read/binary %red.png`。

# 图像! 数据类型

事实上，Red 为图片文件提供了一个特殊的 `image!` 数据类型。假设我们有一个包含 Red 标志图像的二进制文件，名为 `logo.png`。那么我们可以按如下方式将其读入程序：
