# 代码控制结构

在本章中，你将学习如何通过添加条件、选择和循环结构来创建更复杂的代码。我们还将向你展示如何在 Red 中进行错误处理。本章通过开发一个猜数字游戏来说明这一点，用户必须猜出 1 到 99 之间的秘密数字。该秘密数字在程序中随机生成。

为此，我们需要学习如何将数据输入 Red 程序以及如何测试此输入。我们需要多次执行此操作，直到猜出正确的数字。总的来说，我们需要确保我们的程序能够处理错误类型的输入。

在本章中，我们将涵盖以下主题：

* 从用户获取输入
* 测试条件
* 结构化重复
* 猜数字游戏
* 处理错误
* 创建和捕获错误

## 技术要求

你可以在 [`github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter04`](https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter04) 找到本章的代码。如果你已按照 第 2 章 中所述安装了 Red，`Setting Up for Development`，你就可以开始了。你可以在 Windows、macOS X 或 Linux 上工作。你可以在 Red 控制台中输入或粘贴任何代码以查看其结果，或者你可以使用编辑器，例如 Visual Studio Code。

## 从用户获取输入 – input 和 ask

获取输入最简单的方法是使用 `input` 词，如以下代码片段所示：

```
;-- see Chapter04/getting-input.red:
print "Enter a number: "
num: input
print ["You entered the number" num "with" length? num "digits"]
```

当通过 `red getting-input.red` 在控制台中解释或从控制台内部使用 `do %getting-input.red` 时，输入数字 `89` 时我们得到以下结果：

```
Enter a number:
89
You entered the number 89 with 2 digits
```



`num` 的数据类型是什么？如果我们在控制台尝试 `length? 89`，我们会得到一个 `\*\*\* Script Error: length? does not allow integer! for its series` 参数。但 `length? "89"` 返回 `2`，所以 `num` 必须是 `string!` 类型。的确，控制台中输入的任何内容都会被 Red 作为字符串接收。对 `input` 词如此，对我们稍后会使用的 `ask` 词也如此。

如果我们想用 `red -r` 或 `red -c` 编译这个小程序怎么办？这会给你一个 `\*\*\* Compilation Error: undefined word` `input`。原因在于 `input` 词尚未编译到标准库中（这在未来可能会改变）。目前，为了编译它，你必须包含 `input` 的源代码。这可以通过在 Red 头部之后使用以下行来完成：

```
#include %../../../red-source/environment/console/CLI/input.red
```

我们需要的[content]
`num` 的数据类型是什么？如果我们在控制台尝试 `length? 89`，我们会得到一个 `\*\*\* Script Error: length? does not allow integer! for its series` 参数。但 `length? "89"` 返回 `2`，所以 `num` 必须是 `string!` 类型。的确，控制台中输入的任何内容都会被 Red 作为字符串接收。对 `input` 词如此，对我们稍后会使用的 `ask` 词也如此。

如果我们想用 `red -r` 或 `red -c` *编译* 这个小程序怎么办？这会给你一个 `\*\*\* Compilation Error: undefined word` `input`。原因在于 `input` 词尚未编译到标准库中（这在未来可能会改变）。目前，为了编译它，你必须包含 `input` 的源代码。这可以通过在 Red 头部之后使用以下行来完成：

```
#include %../../../red-source/environment/console/CLI/input.red
```

我们需要的文件是 `input.red`，其中定义了 `input` 和其他词。该文件包含在 Red 的源代码的 `environment/console/CLI` 文件夹中。如果您尚未下载源代码，请现在下载并将其放在名为 `red-source` 的文件夹中（参见 第 2 章 中的 *A look at the source code* 部分，*Setting Up for Development*）。从 v 0.6.4 起，此变通方法将不再必要。

要在另一个 Red 文件中包含源文件，请使用 `#include` 命令。因为它需要一个文件，其参数以 `%` 符号开头。

`#include` 词用于在您想要导入 Red 脚本时使用。它使包含文件中的函数可用。实际上，它不过是对代码的复制和粘贴——它不会执行该代码。这与 `do %file.red` 形成对比，后者与 `#include` 相同，然后执行代码。

`../` 有什么用？这只是为了指示在目录结构中向上跳一个文件夹，我们的 `getting-input.red` 文件就位于那里；它是一个相对路径。在我的例子中，我必须向上跳三级文件夹；对您来说，这可能不同。此 `#include` 反过来会从 `/environment/console` 目录包含多个其他文件（请查看 `input.red` 的源代码）。

有了这个，`red -r` 编译将为您提供一个可执行文件，在使用 `./getting-input` 运行它时具有上述行为。

`ask` 词允许我们在 *询问* 时显示一个字符串，有效地替换了 `print` 和 `input`：

```
;-- see Chapter04/ask-input.red:
num: ask "Enter a number: "
print ["You entered the number" num "with" length? num "digits"]
```

编译时，您再次需要使用 `#include` 词，因为 `ask` 尚未包含在标准编译过程中。您还应使用 `red -r` 命令。

要让 Red 程序保持打开并等待输入，请在脚本的最后一行使用 `ask ""`，如下所示：
`ask "Enter some text: "`
`ask ""`

另一种选择是以以下选项启动 Red 脚本：
`red --cli --catch`

我们从 `ask` 和 `input` 得到的是一个字符串，其中去除了换行符；`length?` 词给出了字符数。为了获得整数等价物，我们可以使用上一章中的 `to-integer` 或 `load`。所以现在我们已经学会了如何向用户询问数字猜测。

## 测试条件
如何根据条件执行不同的代码分支？例如，在我们的游戏中，我们必须测试输入的数字是否大于或小于要猜测的数字。Red 为此提供了不同的方法。

### if
以下是 `if` 的工作原理：
* 当条件为真时，则计算随后跟随的代码块
* 如果条件为假，则不执行任何操作；更精确地说，返回 `none` 值：

```
;-- see Chapter04/conditions.red:
account: -250
if account < 0 [print "Your account is overdrawn."] ;== Your account is overdrawn.
if now/time < 12:00 [print "before noon"]           ;== before noon, if e.g. 10:53
```

条件（这里是 `account < 0` 或 `now/time < 12:00`）*不*必用 `( )` 括起来，但其后的代码必须放在块 `[]` 中。

诸如 `and`、`or` 和 `not` 之类的逻辑运算可用于创建复合条件。例如，这里我们使用 `not` 来测试文件是否存在，使用 `exists?` 函数：

```
if not exists? %missing.txt [print "File missing.txt does not exist"] 
```

如果您文件夹中没有此文件，则会打印消息。

我们也可以使用 `any` 或 `all`：

```
n: 12    m: 42
if any [n = 13 m = 42] [print "true!"]      ;== true!
if all [n = 13 m = 42] [print "true!"]      ;== none 
; nothing is printed and none is returned because not all expressions are true
```

需要注意的是，Red 将除 `false` 或 `none` 以外的所有内容都视为真：

```
if 42   [print "true!"]   ;== true!
if "ok" [print "true!"]   ;== true!
if 0    [print "true!"]   ;== true!
if []   [print "true!"]   ;== true!
if none  [print "true!"]  ;== none
if false [print "true!"]  ;== none
```

\=> 现在回答 *Questions* 部分中的问题 1。

### unless
Red 还有一个 `unless` 词，它与 `if not` 相同。

只有当条件为 `false` 时，`unless` 之后的代码块才会被执行。因此，我们可以用以下方式重写前面关于文件存在性测试的那一行：

```
unless exists? %missing.txt [print "File missing.txt does not exist"]
```

### either
以下行的结果是什么？

```
if 13 < 42 [print "13 < 42"] [print "13 > 42"]     
; 13 < 42  ;== [print "13 > 42"]
```

正如您所见，它正确地打印出 `13 < 42`，但返回值是以下内容：

```
[print "13 > 42"]   
```

这是正常行为——该块位于 `if` 之后，因此作为块返回。

与大多数其他编程语言不同，Red 中的 `if` *没有* `else` 分支，请注意这种行为！

如果您需要 `if…else` 结构，请使用 `either`。因此，对于前面的语句，请使用以下内容：

```
either 13 < 42 [print "13 < 42"][print "13 > 42"]    ;== 13 < 42
```

这会输出预期的结果 `13 < 42`。当我们使用 `>` 代替 `<` 时，使用另一分支：

```
either 13 > 42 [print "13 > 42"][print "13 < 42"]   ;== 13 < 42
```

记住，`if` 只接受一个块，`either` 必须有两个块！

这里是另一个例子：

```
print either now/time < 12:00 ["AM"]["PM"]   ;== PM
```

`now/time` 短语给出了当前时间，例如当时间为 16:23 时会打印出 `PM`。

当然，块也可以包含绑定语句：

```
n: 42
either n < 0 [msg: "negative"][msg: "zero or positive"]
print ["n is" msg]    ;== n is zero or positive
```

当两个分支返回相同类型的值时，如这里的情况，`either` 的结果可以绑定到一个词，如以下代码所示：

```
n: 42
msg: either n < 0 ["negative"]["zero or positive"] ;== "zero or positive"
msg    ;== "zero or positive"
```

当块包含多个语句时，您应使用四空格缩进的以下样式。这在以下代码片段中有所说明，该片段模拟用户登录操作：

```
user: ask "Username (jvh38): "
pass: ask "Password (avi108): "
either all [user = "jvh38" pass = "avi108"][
    print ["Hi" user]
    print "Welcome back"
][
    print "Incorrect Username/Password"
]
;-- if input entered as shown: 
;== Hi jvh38
;== Welcome back
```

将此作为多行编写块的标准方式。`either` 块也可以彼此嵌套，但这很快就会变得难以阅读，因此不要过度使用。

\=> 现在回答 *Questions* 部分中的问题 2 和 3。

## 停止程序 – halt 或 quit
当满足某个条件时如何停止程序？例如，在我们的猜测程序中，当用户输入字母 `S` 时，我们希望停止运行脚本。这里是解决方案——只需在控制台中使用 `quit`，如下面的代码所示：



```
answer: ask "Type a number (from 1 till 99) or stop(S): "
if answer = "S" [
    print "OK, you want to stop."
    quit
]
print "This line is not executed when S is entered"
```

这可以写成 `either` 语句，但我觉得这种格式更易读。

`quit` 词也可以在出现错误条件时用来退出程序。

使用 `/return` 修饰符，你可以通过写成 `quit/return 5` 的形式向操作系统发出返回状态。你也可以使用替代写法 `\-return 5`。当 Red 程序从 shell 脚本调用时，这会很有用。

如果你的程序是解释执行而不是编译执行，你可以使用 `halt` 代替 `quit`。这样会将控制权返回到 Red 控制台提示符而不是操作系统层，并且会显示 `(halted)`。例如：

```
if now/time > 19:00 [prin "Stop working" halt]     ;== Stop working(halted)
```

如果你需要暂停正在运行的脚本一段时间，比如五秒，使用 `wait 5`.

## switch 和 case

假设你需要测试一个词的不同 *值*，并且每个值都需要不同的操作。你会怎么做？你可以使用多个 `if` 测试，写成 `if var = value1` 的形式，但那样你就必须为每个要测试的新值添加一个新的 `if`，并且所有 `if` 测试都会被执行。Red 有一个多功能的 `switch` 词，可以让你在一句话中做到这一点。例如：

```
person: "John"
switch person [
    "Bob"   [print "Bob is at the gym"]
    "John"  [print "John is at work"]
    "Laila" [print "Laila is shopping"]
] ;== John is at work
```

`switch` 词会评估 `person` 词，并只执行与相同值对应的分支。

如果找不到该值，就不会执行任何分支。在这种情况下，提供一个带有 `/default` 修饰符的默认分支可能会很有用，如以下代码所示：

```
person: "John"
switch/default person [
    "Bob"      [print "Bob is at the gym"]
    "Lawrence" [print "Lawrence is at work"]
    "Laila"    [print "Laila is shopping"]
][
    print "person not found"
] ;== person not found
```

要测试的词可以是任何数据类型，包括数字、字符串、词、日期、时间、URL、文件和块。你甚至可以测试一个词的类型，如以下代码所示：

```
var: 108
switch type?/word var [
    string!  [print "found a string"]
    binary!  [print "found a binary"]
    integer! [print "found an integer number"]
    decimal! [print "found a decimal number"]
]   ;== found an integer number
```

`switch` 值也可以是不同的数据类型。

\=> 现在在 *问题* 部分回答第 4 题。

除了测试值，我们也可以测试 *条件* 吗？同样，你可以用多个 `if` 语句来做，但更好的方法是使用 `case`，它会评估多个条件。第一个为 `true` 的条件的块会被评估，如下所示：

```
num: 42
case [
    num > 42 [print "num is more than 42"]
    num < 42 [print "num is less than 42"]
    num > 33 [print "num is more than 33"]
    num = 42 [print "num is 42"]
    true     [print "yes its true"]
] ;== num is more than 33
```

只有一个块会被执行。如果你希望所有条件为真的块都被评估，使用 `/all` 修饰符。如果我们把前面的例子中的 `case` 替换为 `case/all`，输出如下：

```
num is more than 33
num is 42
yes its true
```

`true` 分支是默认分支，但它是可选的。当你需要根据情况在多种操作中进行选择时，使用 `case`。

\=> 现在在 *问题* 部分回答第 5 题。

## catch 和 throw

另一种允许代码控制的机制是 `catch` - `throw`。请看下面的例子：

```
catch [
    print "before throw" 
    throw "error" 
    print "after throw"
]
```

这会打印 `before throw` 并返回 `error`。`throw` 词会中断代码流程并返回到下一个更高层的 `catch`，返回 `throw` 给出的值。`catch` 必须始终跟随一个包含语句或函数调用的块 `[...]`。

虽然 `case` 只能在所有可能条件都指定的局部块中使用，`catch` 可以在调用包含 `throw` 的函数时使用，如下所示：

```
catch [ function-name arguments]
```

`catch` / `throw` 跨越函数边界工作——嵌套函数中发生的 `throw` 可以在更高层的函数中被捕获。

你不能在没有 `catch` 的情况下使用 `throw`，正如 `if n = 0 [throw 5]` 所示，这会给出错误 `*** Throw Error: no catch for throw: 5`。

为了获得更多可能性，`throw` 可以有一个 `/name` 修饰符，写成 `throw/name rvalue tname`，其中 `tname` 是 `throw` 名称，`rvalue` 是返回值。这些 `throw` 词仍然会被 `catch` 捕获，但你可以通过在 `catch` 中指定一个或多个名称来细化捕获：

```
catch/name [some function call with a throw] 'tname
```

这里，`tname` 也可以是一个块，例如 `[tname1 tname2]`，以捕获多个命名的 `throw` 实例。

使用函数和名称的 catch 和 throw 的详细示例可以在这里找到：[`www.red-by-example.org/#catch`](http://www.red-by-example.org/#catch)。

虽然 `catch` `throw` 看起来很像其他语言中使用的错误处理结构，但实际上并非如此！错误是在执行期间（运行时）发生的，并非程序员有意为之。`catch` 和 `throw` 词是刻意编程的。对于错误处理，请使用 *处理错误* 部分描述的技术。

## 结构化重复

通常，你需要重复一段代码行，要么重复一定次数，要么在某个条件为真时重复。`forever` 词允许无限重复。使用时应谨慎——确保使用 `break` 跳出无限循环并继续执行程序的其余部分。以下片段展示了一个热衷读书者的生活：

```
;-- see Chapter04/repetitions.red:
count: 99
forever [
    print append form count " books to read"
    ; print [form count " books to read"]   ;-- same as append
    count: count - 1
    if count = 0 [break]
]
;== 99 books to read
;...
;== 2 books to read
;== 1 books to read
```

当 `count` 递减到 `0` 时，我们跳出循环。

我们使用 `append` 将两个字符串连接起来——第一个是 `form count`（它将 `count` 转换为字符串），第二个字符串是句子的其余部分。代替 `append`，我们也可以直接使用 `print [form count " books to read"]`。

## 重复一定次数 – loop 或 repeat

如果你需要执行一个块固定次数，最简单的方法是使用 `loop`：

```
n: 3 ;== 3
loop n [prin "looping "] ;= looping looping looping ; recall: prin is no newline
```

在 `loop` 中，你不能在代码块中使用计数器。如果需要这样做，使用 `repeat`：

```
repeat i 5 [prin i]          ;== 12345
```

`repeat` 词有一个索引（这里命名为 `i`），每次循环都会自动递增。索引从 `1` 数到第二个参数，即循环次数。你可以在代码块本身中使用并更改 `i`，但 `i` 会在每次循环开始时重新初始化，如以下代码所示：

```
repeat i 5 [i: i + 2 prin i]   ;== 34567
```

\=> 现在从 *问题* 部分回答第 6 题。

## 条件重复 – while 或 until

在其他情况下，你可能希望循环的继续取决于一个条件：

* 你可能想在某个条件为真时继续 `while`，当条件变为假时停止循环
* 你可能想做相反的操作——在某个条件为真时循环 `until`

这里有一个 `while` 词的例子，打印出小于 `10` 的奇整数：



## While、Until 与循环控制

```
 n: 1
 while [n <= 10 ][
     if odd? n [prin n prin " "]
     n: n + 1
 ] ;== 1 3 5 7 9
```

请注意条件（此处为 `n <= 10`）必须放在代码块 `\[ \]` 中。该块可包含多个语句和表达式；最后一个被视为条件。

我们可以通过模拟第二步得到相同的结果：

```
 n: 1
 while [n <= 10 ][
     prin n prin " "
     n: n + 2
 ] ;== 1 3 5 7 9
```

每个 `while` 都可以通过反转条件转换为 `until`。但要注意——应该在某一时刻变为真的条件，必须作为 *`until` 块中的最后一个表达式* 出现：

```
 n: 1
 until [
     prin n prin " "
     n: n + 2
     n > 10 ; the condition to end the loop
 ] ;== 1 3 5 7 9
```

这也意味着该块至少会执行一次。

`break` 词可用于跳出任何类型的循环。如果需要，可以使用 `break/return value` 返回一个值。如果想跳过当前循环的迭代，使用 `continue` 将控制权转移回循环开始处以开始下一次迭代。以下代码片段说明了这一点：

```
repeat n 8 [
    prin ["Before" n " - "]
    if n < 3 [continue]
    if n = 6 [break]
    prin ["After" n " - "]
]
```

这会输出以下内容：

```
Before 1 - Before 2 - Before 3 - After 3 - Before 4 - After 4 -
Before 5 - After 5 - Before 6 -
```

Red 中没有 `for` 词来执行类似 `for i = 2 to 10 step 2` 的操作，如其他语言中所做的那样。相反，使用 `repeat` 和 `while` 来实现你想要的功能。例如，`to value` 短语可与 `while` 中的 `<= value` 条件一起使用，而步进可在 `while` 代码块中通过 `i: i + step` 执行。

在块中遍历序列时，你最常使用 `foreach` 和 `forall`，我们将在下一章讨论它们。

\=> 现在回答 *问题* 部分的第 7 个问题。

## 猜数字游戏

现在我们已经拥有制作游戏所需的所有构建块。

从上一章我们已经知道如何挑选 1 到 99 之间的随机数，并如何对其进行种子设置，以便每次程序都会给出另一个 `secret-number`：

```
;-- see Chapter04/guess-number.red:
random/seed now/time
secret-number: random 99
```

从用户获取输入使用 `ask`；我们在 *停止程序* 部分见过此代码片段。用户被允许猜测一定次数，例如 10 次，这个数字存储在 `max-number-of-guesses` 中。因此，所有包含输入及其后续逻辑的代码必须在具有该特定迭代次数的循环中执行：

```
loop max-number-of-guesses [ code ]
```

我们还跟踪 `number-of-guesses`。当然，我们的程序在每一步都必须给出清晰的消息。

每次循环迭代必须经过哪些逻辑步骤？让我们记下来：

1. 取输入并检查用户是否想停止。
2. 增加 `number-of-guesses`，并检查是否尚未超过最大猜测次数；如果超过，则从循环中 `break`。
3. 将输入转换为数字以得到 `guessed-number`。
4. 如果 `guessed-number` 与 `secret-number` 相同，用户就赢得了游戏。在这种情况下，从循环中 `break`。
5. 将 `guessed-number` 与 `secret-number` 进行比较，并告诉用户他们的猜测比秘密数字大还是小。然后 `continue`。

在解释了一切之后，现在很容易理解完整代码（参见 `guess-number.red`）。为了简洁，我们省略了头部。步骤在代码中以例如 `;--2` 的形式标出：

```
 random/seed now/time
 secret-number: random 99
 max-number-of-guesses: 10
 number-of-guesses: 0
 
 loop max-number-of-guesses [
 ;-- 1
     answer: ask "Type a number (from 1 till 99) or stop(S): "
     if answer = "S" [
         print "Allright, you want to stop."
         break
     ]
 ;-- 2
     number-of-guesses: number-of-guesses + 1
     print ["--> Number of guesses:" number-of-guesses]
     if number-of-guesses > (max-number-of-guesses - 1) [
         print ["Sorry, too much guessing, the secret number was" secret-number]
         break
     ]
 
     guessed-number: load answer ;-- 3
 ;-- 4
     if secret-number = guessed-number [
         print ["Congratulations. You guessed the number" secret-number "in" number-of-guesses "times"]
         break
 ]
 ;-- 5
     either secret-number < guessed-number [
         print ["The secret number is smaller than" guessed-number]
         continue
     ][
         print ["The secret number is bigger than" guessed-number]
         continue
     ]
 ] ; end loop
 
 print "Thanks for guessing!"
```

使用 `red -r guess-number.red` 编译程序，并使用 `./guess-number` 享受游戏！也许你可以添加一些额外的逻辑，例如以下内容：

* 在五次尝试后通知用户剩余的猜测次数
* 当猜测与秘密数字之间的差值小于 10 时，告诉用户他们接近了

请向 GitHub 仓库发送 pull request！

## 处理错误

**错误** 可能由用户输入错误引起，也可能由系统产生。

如果你在 REPL 中输入 `4 / 0`，你会收到 `*** Math Error: attempt to divide by zero` 警告。Red 控制台在出现错误时不会崩溃，但编译后的程序会。

预定义错误的不同类型可通过 `? system/catalog/errors` 找到。

要查看特定错误，请使用类型作为细化——例如，

`? system/catalog/errors/syntax`，它会显示例如 `no-header`、`missing` 或 `invalid` 错误。

现在，让我们回到猜数字游戏。如果用户输入字符串 `"abc"` 而不是数字会怎样？那么猜测就在那里停止——程序会因错误而崩溃：

```
;-- see Chapter04/handling-errors.red:
*** Script Error: cannot compare 4 with "abc"
*** Where: <
```

这发生在比较秘密数字（在本例中为 `4`）与字符串 `4 < "abc"` 时。

Red 有不同的方法使你的程序对由危险输入引起的运行时错误具有弹性。

### attempt 和 try

首先，你可以使用 `attempt` 在块中评估危险代码：

```
attempt [4 < "abc"] ;== none
```

而不是因错误而崩溃，这只会简单返回 `none`，而行为良好的代码则会简单执行并返回其结果，例如 `attempt \[4 \* 7\] ;== 28`。

因为 `attempt` 有效地丢弃了错误，所以不适合生产代码。然而，在开发过程中，这可用于完善程序的逻辑。处理错误最好在第二阶段进行，但到那时你应该 definitely 使用比 `attempt` 更好的东西，即 `try`。

`try` 词尝试评估一个块，类似于 `do`：

```
try [4 * 7]        ;== 28 ; normal evaluation result
try [4 / 0]        ; *** Math Error: attempt to divide by zero
type? try [4 / 0]  ; == error!
```

我们看到在出现错误的情况下，`try` 返回 `error!` 类型的值。

`try` 词有一个 `/all` 细化，它充当终极屏障；它捕获所有可能的异常，包括 `break` 和 `continue` 的不当使用，以及 `return` 或 `exit`，我们将在 第 6 章 *使用函数和对象* 中遇到它们。

### 使用 error?

使用类型测试 `error?` 函数结合 `try` 来测试代码是否返回错误：

```
error? try [4 < "abc"] ;== true
```

`error? try` 短语在出现错误时返回 `true`，否则返回 `false`。

不要忘记 `try` 后面跟着必须被测试的代码块。如果你不使用它而使用 `error?`，危险代码不会被执行，因此不会生成 `error!`，返回 `error? \[4 < "abc"\] ;== false`。

我们可以使用这一点来查看条件测试是否存在运行时错误，表述为 `if error? try`：



```
if error? try [4 < "abc"][
    print "Wrong input, enter an integer!"
    quit ; or: continue
]
```

我们也可以使用 `either error? try`：

```
either error? try [4 < "abc"][
    print "Wrong input, enter an integer!"
    quit ; or: continue
 ][
    ; everything ok, now we can proceed
 ]
```

你可以使用 `probe` 词来了解更多关于发生的错误的详细信息：

```
if error? err: try [4 < "abc"] [probe err]
```

这会产生以下输出，在调试时很有用：

```
make error! [
    code: 339
    type: 'script
    id: 'invalid-compare
    arg1: 4
    arg2: "abc"
    arg3: none
    near: none
    where: '<
    stack: 77629472
]
*** Script Error: cannot compare 4 with "abc"
*** Where: <
*** Stack: probe
```

# 创建和捕获错误

在可能发生运行时错误的情况下，你可以使用 `cause-error` 自己引发特定的错误。例如：

```
m: 10
if n = 0 [ cause-error 'math 'zero-divide [] ]
print m / n
```

当 `n` 为 `0` 时，这一行会生成 `*** Math Error: attempt to divide by zero`。当进入一个 `n` 参数可能为 `0` 的函数，并在函数中想对 `n` 进行除法时，这可能很有用。

请注意它需要字面词 `'math` 和 `'zero-divide`，描述错误的类型和名称；第三个参数是一个参数块，可以为空。除了 `cause-error` 之外，你也可以遇到 `make error! [math zero-divide]` 或甚至 `make error! 400`。它们的作用都一样——使用该名称或代码生成错误。

只有当你想让程序停止并显示错误信息时才使用 `cause-error`。

总之，为了在 Red 中正确处理错误，请一起使用 `try`、`if error?`、`cause-error` 和 `error!` 对象。

`catch - throw` 短语用于控制代码流程，正如本章的 `Catch and throw` 部分所见，而不是用于错误处理。

# 检查类型

编写以运行时错误结束的代码是不专业的，因此在可能的情况下，使用不会产生错误的代码。在我们的游戏中，你可以使用以下方法测试 `guessed-number` 的类型：

```
if integer? guessed-number 
```

如果 `guessed-number` 不是整数，使用 `continue` 来要求下一次猜测：

```
;-- see Chapter04/guess-number2.red:
if not integer? guessed-number [
    print ["This is no integer! Try again."]
    continue
]
```

你也可以在这里使用更复杂的 `if (type? guessed-number) <> integer!`，但我们力求在 Red 中让事情更简单，记住吗？

要检查一个 `value` 是否为数字（整数或浮点数），请使用 `number? value`。

我们将在 第 6 章 中重返此主题，`Using Functions and Objects`，以讨论使用函数的错误处理。

# 摘要

本章我们学到了很多！我们现在知道如何从用户获取控制台输入。我们学习了多种方法让代码根据条件做出响应，使用 `if`、`either`、`switch`、`case` 或 `catch` 和 `throw`。我们还看到重复代码可以用 `forever`、`loop`、`repeat`、`while` 和 `until` 来实现。我们讨论了如何使用 `attempt`、`try`、`error?` 和 `cause-error` 进行错误处理。最后，我们构建了一个数字猜测游戏，利用了其中大部分技术。

到目前为止，你应该能够用简单的脚本写出程序的逻辑。在下一章，你将学习如何使用块和系列，这是 Red 中的基础数据结构。

# 问题

1. 给出以下语句的结果。首先在脑中完成，然后在控制台中验证：

```
      if 0 [print "0 is true"]
      if 13 = 13.0 [print "ok"]
      if 13 == 13.0 [print "ok"]
```

2. 执行以下语句后 `out` 的值是多少？

```
      p: 7
      out: either p == 7.0 ["p is 7"]["bad luck"]
```

3. 编写一个代码片段，询问用户是否正在下雨，并提供下雨与否的适当响应。使用多行块样式。

4. 编写一个 `switch` 片段，其中值的数据类型不同——例如，使用具有 `"red"` 值的 `item` 变量并测试不同的值。

5. 编写一个 `case` 片段，测试 `name` 词是否包含元音（a、e、i、o 或 u）。提示：条件形式为 `find name "a";`，等等。在 REPL 中使用 `? find` 查找解释。我们将在下一章讨论 `find`。

6. 编写一个产生输出 `1 2 3 4 5 6 7` 的循环。提示：使用 `append` 和 `form`，如 `forever` 循环中所示。

7. 编写一个火箭倒计时（从 10 到 1）。
