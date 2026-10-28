## 第 6 章 转换和定义时间变量

本章概述了转换和存储时间变量的各种方法。这是一个比较常规的主题，但数据中通常会包含各种形式的日期和时间信息。本章大致分为三个部分：

- **将字符串转换为时间数据类型**：本节介绍如何将文本格式的时间戳转换并存储为 R 的一种内部时间格式。
- **将时间数据类型转换为字符串**：本节介绍如何将时间数据类型转换为字符串，以便能以标准格式保存到文件中。
- **时间数据类型的运算**：本节概述了在时间数据类型上执行基本算术运算的方法和技术。

### 引言与假设

日期和时间信息通常作为数据集的一部分被保存。将这类信息转换为日期或时间变量是较为枯燥的工作之一，而且需要格外细心。在本章中，我们将讨论转换字符串与数据类型的方法，并演示如何执行基本的算术运算。有一点很重要：处理时间和日期数据会在多种不同的情境中出现，也有许多不同的库（如 `chron`、`lubridate` 和 `date`）可以帮助你处理时间和数据变量。

不过，我们这里的重点是 R 中用于处理时间和日期数据的内置函数。需要注意的是，这里讨论的命令可能对数据文件中的细微变化很敏感，因此你应该经常检查自己的工作，尤其是时间数据方面。这可能是一项乏味的任务，但确保数据正确非常重要。如果你经常处理时间数据，应该阅读 `help(DateTimeClasses)` 命令中给出的详细信息。

另一个复杂之处在于时区可能会发生变化，与时间数据相关的通用做法也可能会改变。例如，新的时区可能会被更改、创建或移除，或者某个区域可能会更改其时区。你应该始终确保自己处理时间数据的做法与生成数据时所采用的做法保持一致。

### 将字符串转换为时间数据类型

我们要探讨的第一个任务是将字符串转换并存储为各种内部时间格式。`strptime` 命令接受一个字符串并将其转换为 POSIXlt 时间变量。如果希望将字符串转换为 POSIXct 数据类型，可以使用 `as.POSIXct` 命令转换 `strptime` 的结果。我们首先关注如何将字符串转换为 POSIXlt 数据类型，并在本节末尾给出一个转换为 POSIXct 数据类型的示例。

要将字符串转换为时间数据类型，必须指定该字符串的格式，且格式选项必须符合 ISO C99/POSIX 标准。字符串包含一系列字面字符和一部分转换子字符串，*表 1* 中给出了部分列表。例如，`%Y-%m-%d %H:%M:%S` 字符串表示，当日期为 2014 年 5 月 14 日上午 9:54 时，其显示形式应为 *2014-05-14 09:54:10*。一个字符串可以包含任意数量的预定义字符串。除此之外的内容都被视为字面字符，必须与格式字符串中显示的形式完全一致。

一旦指定了格式字符串，就可以使用 `strptime` 命令将字符串转换为 POSIXlt 数据类型。该命令的参数为要转换的字符串，后跟格式字符串：

```
> theTime <- c("08:30:00 1867-07-01", "18:15:00 1864-10-27")
> converted <- strptime(theTime,"%H:%M:%S %Y-%m-%d")
> converted
[1] "1867-07-01 08:30:00" "1864-10-27 18:15:00"
> typeof(converted[1])
[1] "list"
> converted[1]-converted[2]
Time difference of 976.5938 days
```

该命令还接受一个可选的时区选项，如下所示：

```
> theTime <- c("08:30:00 1867-07-01", "18:15:00 1864-10-27")
> converted <- strptime(theTime,"%H:%M:%S %Y-%m-%d", + tz="Canada/Eastern")
> converted
[1] "1867-07-01 08:30:00 EST" "1864-10-27 18:15:00 EST"
> converted[1]-converted[2]
Time difference of 976.5938 days
```

`strptime` 命令返回的结果是 POSIXlt 数据类型。可以使用 `as.POSIXct` 命令对其进行转换：

```
> theTime <- c("08:30:00 1867-07-01", "18:15:00 1864-10-27")
> converted <- strptime(theTime,"%H:%M:%S %Y-%m-%d", tz="Canada/Eastern")
> converted
[1] "1867-07-01 08:30:00 EST" "1864-10-27 18:15:00 EST"
> typeof(converted[1])
[1] "list"
> otherTime <- as.POSIXct(converted)
> otherTime
[1] "1867-07-01 08:30:00 EST" "1864-10-27 18:15:00 EST"
> typeof(otherTime[1])
```





| 格式 | 描述 | 格式 | 描述 |
|---|---|---|---|
| `%a` | 星期几名称的缩写 | `%p` | “A.M.” 或 “P.M.” 的指示符 |
| `%A` | 星期几的完整名称 | `%S` | 秒数（00-61）；允许闰秒 |
| `%b` | 月份名称的缩写 | `%U` | 一年中的周数（00-53） |
| `%B` | 月份的完整名称 | `%w` | 星期几的数字（0=星期日，1=星期一，…，6=星期六） |
| `%c` | 日期和时间格式为 `%a %b %e %H:%M:%S %Y` | `%x` | 日期形式为 “%y/%m/%d” |
| `%d` | 月份中的天数（01 至 31） | `%X` | 时间格式为 “%H:%M:%S” |
| `%H` | 小时数（00-23）；注意 24 可作为 24:00:00 的例外使用 | `%y` | 两位数的年份（“00-99”），其中 00-68 指 2000 年代，而 69-99 指 1900 年代 |
| `%I` | 12 小时制小时数（01-12） | `%Y` | 四位数的年份（>=1582） |
| `%j` | 一年中的天数（001-366） | `%z` | 与 UTC 的偏移量（-0500 表示比 UTC 慢五小时） |
| `%m` | 月份数（01-12） | `%Z` | 时区字符串（仅在将时间转换为字符串时可用） |
| `%M` | 分钟数（00-59） |  |  |

*表 1 – 用于定义日期格式的字符*

更多选项可用，可在 R 环境中使用 `help(strftime)` 命令查看。

### 将时间数据类型转换为字符串

日期和时间变量可使用 `strftime` 命令转换为字符串。其格式与 `strptime` 命令相似，只是多了一个用于确定结果字符串是否包含时区信息的选项。该命令是一个便捷函数，根据时间变量的数据类型调用 `format.POSIXlt` 或 `format.POSIXct` 命令。我们重点关注 `strftime` 命令，因为它对来自更广泛编程经验的人来说很熟悉。

`strftime` 命令需要一个时间变量和一个格式。它返回给定格式的字符串（部分格式选项见 *表 1*）：

```r
> theTime <- c("08:30:00 1867-07-01", "18:15:00 1864-10-27")
> converted <- strptime(theTime,"%H:%M:%S %Y-%m-%d",
+ tz="Canada/Eastern")
> converted
[1] "1867-07-01 08:30:00 EST" "1864-10-27 18:15:00 EST"
> typeof(converted)
[1] "list"
> backAgain <- strftime(converted,"%j - %B")
> backAgain
[1] "182 - July"   "301 - October"
> typeof(backAgain[1])
[1] "character"
```

### 时间数据类型的运算

时间数据类型有多种算术运算可用，执行任何运算时应格外谨慎。返回的单位可能因上下文而异。很容易失去对单位的跟踪并做出错误的比较。在本节中，我将介绍一些基本运算，然后讨论 `difftime` 命令。可以在不使用 `difftime` 命令的情况下执行任何运算，但该命令具有重要优势：它允许你明确定义单位。

对时间数据类型执行简单算术运算时，其行为如你所预期：

```r
> earlier <- strptime("2014-01-01 00:00:00", "%Y-%m-%d %H:%M:%S")
> later <- strptime("2014-01-02 00:00:00", "%Y-%m-%d %H:%M:%S")
> later-earlier
Time difference of 1 days
> timeDiff <- later-earlier
> timeDiff
Time difference of 1 days
> as.double(timeDiff)
[1] 1
> earlier+timeDiff
[1] "2014-01-02 EST"
```

注意，在前面的示例中，使用的是天为单位。稍作改动，结果类型就会不同：

```r
> earlier <- strptime("2014-01-01 00:00:00", "%Y-%m-%d %H:%M:%S")
> later <- strptime("2014-01-01 12:00:00", "%Y-%m-%d %H:%M:%S")
> later-earlier
Time difference of 12 hours
> timeDiff <- later-earlier
> timeDiff
Time difference of 12 hours
> as.double(timeDiff)
[1] 12
```

R 环境会跟踪差异及其单位。如果查看前一个示例中的 `difftime` 变量，它包含有关其含义的信息：

```r
> attributes(timeDiff)
$units
[1] "hours"

$class
[1] "difftime"

> attr(timeDiff,"units")
[1] "hours"
```

你可以通过将结果强制转换为数值并提供要使用的单位来指定单位：

```r
> earlier <- strptime("2014-01-01 00:00:00", "%Y-%m-%d %H:%M:%S")
> later <- strptime("2014-01-01 12:00:00", "%Y-%m-%d %H:%M:%S")
> timeDiff <- later-earlier
> timeDiff
Time difference of 12 hours
> as.numeric(timeDiff,units="weeks")
[1] 0.07142857
> as.numeric(timeDiff,units="secs")
[1] 43200
```

由于可能存在歧义，通常最好使用 `difftime` 命令。`difftime` 命令提供广泛的选项，应仔细阅读其帮助页面 `help(difftime)` 以查看选项和细节。在最基本的使用中，你可以找到两个时间之间的差异，并指定要使用的单位：

```r
> earlier <- strptime("2014-01-01 00:00:00", "%Y-%m-%d %H:%M:%S")
> later <- strptime("2014-01-01 12:00:00", "%Y-%m-%d %H:%M:%S")
> timeDiff <- difftime(later,earlier,units="sec")
> timeDiff
Time difference of 43200 secs
> timeDiff <- difftime(later,earlier,units="day")
> timeDiff
Time difference of 0.5 days
```

可用的单位为 auto、secs、mins、hours、days 或 weeks。

最后一点说明；`difftime` 数据类型提供了一种方便的方式来执行一些时间算术运算。`as.difftime` 命令可用于指定时间间隔，并可指定单位，因此结果更符合你的预期：

```r
> later <- strptime("2014-01-01 12:00:00", "%Y-%m-%d %H:%M:%S")
> oneHour = as.difftime(1,units="hours")
> later+oneHour
[1] "2014-01-01 13:00:00 EST"
```

### 总结

本章对在字符串和两种时间数据类型之间转换的可用选项进行了广泛概述。`strptime` 命令用于将字符串转换为 POSIXlt 变量。`strftime` 命令用于将时间数据类型转换为字符串。最后，讨论了用于在两个时间变量之间执行算术运算的基本运算符，重点是使用 `difftime` 命令。我们还探讨了 `difftime` 数据类型的用法。

### 条件执行

首先检查的控制结构是 if 语句。if 语句会确定是否应执行代码块，并具有可选的 else 语句。额外的 if 语句可以链接到 else 语句以创建选项级联。

在最基本的形式中，if 语句具有以下形式：

```
if(condition)
    code block
```

条件可以是从下一张表中给出的运算符构造的逻辑语句，也可以是数值结果。如果条件为数值，则零被视为 FALSE，否则语句为 TRUE。代码块可以是单个语句，也可以是用大括号括起来的语句集：

```
> # check if 1 is less than 2
> if(1<2)
+     cat("one is smaller than two.\n")
one is smaller than two.
> if(2>1) {
+     cat("But two is bigger yet.\n")
+ }
But two is bigger yet.
```

注意，前面的代码中包含了一个注释。# 字符用于表示注释。# 字符之后的所有内容都会被忽略。

if 语句可使用 else 语句扩展。else 语句允许你在条件未求值为 TRUE 时指定要执行的代码块：

```
> if(1>2) {
+     cat("One is the most biggest number\n")   # Say something wrong
+ } else {
+     cat("One is the loneliest number\n")      # Say something less wrong
+ }
One is the loneliest number
```

这些示例首先需要注意的是，采用了 **Kernighan and Ritchie (K&R)** 缩进风格。我们采用 K&R 风格，因为 else 语句必须与闭合大括号在同一行。其次需要注意的是，另一个 if 语句可以附加到 else 语句以构造标准的 if-then-else 块：

```
> if(0) {
+     cat("Yes, that is FALSE.\n")
+ } else if (1) {
+     cat("Yes, that is TRUE\n")
+ } else {
+     cat("Whatever")
+ }
Yes, that is TRUE
```

一个潜在问题是 R 没有标量类型，并假设大多数数据类型都是


以向量形式排列。这可能会导致逻辑语句出现问题。向量中的第一个元素用于判断整个语句是 TRUE 还是 FALSE。幸运的是，如果条件计算结果为长度大于 1 的向量，R 会给出警告：

```
> x <- c(1,2)
> if(x < 2) {
+   cat("Oh yes it is\n")
+ }
Oh yes it is
Warning message:
In if (x < 2) { :
  the condition has length > 1 and only the first element will be used
```

在决定 if 语句中使用哪个逻辑运算符时，这一点需要牢记。例如，`|` 运算符会对向量的所有元素执行逻辑或运算，但 `||` 运算符只对向量的第一个元素执行逻辑或运算：

```
> x <- c(FALSE,FALSE,TRUE)
> y <- c(FALSE,TRUE,TRUE)
> x|y
[1] FALSE  TRUE  TRUE
> x||y
[1] FALSE
```

R 能识别多种逻辑运算符。有些运算符会对向量中的所有元素进行比较，有些则只对向量中的第一个元素进行比较。下表列出了这些运算符：

| 运算符 | 描述 | 运算符 | 描述 |
|---|---|---|---|
| < | 小于（向量） | \| | 或（向量） |
| > | 大于（向量） | \|\| | 或（仅第一个元素） |
| <= | 小于等于（向量） | ! | 非（向量） |
| >= | 大于等于（向量） | & | 与（向量） |
| == | 等于（向量） | && | 与（仅第一个元素） |
| != | 不等于（向量） | xor(a,b) | 异或（向量） |

表 1 – 逻辑运算符（包括比较运算符）

### 循环结构

另一个重要的编程任务是创建一组可以按结构方式重复执行的指令。R 中有三种循环结构：for 循环、while 循环和 repeat 循环。我们将逐一探讨这些循环结构，并讨论 break 和 next 命令，它们可以控制循环代码块内指令的执行方式。使用 `help(Control)` 命令可以查看更多详细信息。

#### for 循环

for 循环接受一个向量，并针对向量中的每个值重复执行一段代码。语法如下：

```
for(var in vector)
  code block
```

for 循环会按相应顺序对向量中的每个值重复执行一组指令。如果你需要多次重复该循环，而向量又尚未存储在工作区中，则可能会占用大量内存。下面是一个简单 for 循环的示例：

```
> for(lupe in seq(1,2,by=0.33))
+ {
+   cat("The value of lupe is ",lupe,"\n")
+ }
The value of lupe is  1
The value of lupe is  1.33
The value of lupe is  1.66
The value of lupe is  1.99
```

#### while 循环

while 循环会在给定表达式为真的情况下执行代码块。语法如下：

```
while(condition)
  code block
```

它们通常用于事先不知道迭代次数、循环会一直重复直到满足某个条件的情况。此外，while 循环相对于 for 循环有一些优势。它可能更高效，尤其是在内存方面，而且更灵活（RMemory，有关此主题的更多信息，请参阅 RMemory: Hadley Wickham, memory, 2014, http://adv-r.had.co.nz/memory.html）。例如，无需构造一个大型向量来遍历其元素，而是可以使用单个索引。缺点是可读性较差，编写代码时需要多加小心。下面给出一个示例：

```
> lupe <- 1.0;
> while(lupe <= 2.0)
+ {
+   cat("The value of lupe is ",lupe,"\n")
+   lupe <- lupe + 0.33
+ }
The value of lupe is  1
The value of lupe is  1.33
The value of lupe is  1.66
The value of lupe is  1.99
```

#### repeat 循环

repeat 循环用于表示一段代码块，该代码块将反复执行，直到显式跳出该代码块。repeat 循环的主要优点是代码块的起点总是会执行。一个缺点是可能难以阅读。语法相对简单：

```
repeat
    code block
```

你必须使用 break 命令来告诉 R 退出该代码块。break 命令将在下一小节中更详细地描述。下面的示例给出了一个 repeat 循环的示例，它是在复平面中模拟随机游走的简要示例：

```
positions  <- complex(0) # Initialize the history of positions
currentPos <- 0.0+0.0i    # Start at the origin
NUMBERSTEPS <- 50          # Number of steps to take
angleFacing <- 0.0         # direction it is facing
stdDev      <- 1.0         # std dev. of the change in the angle

step <- as.integer(0)
repeat
{
    ## Update the current time step
    step <- step + as.integer(1)

    ## Check to see if it is time to stop
    if(step>MAXNUMBER)
        break

    ## Add new people to the line and update the length
    angle <- angle + rnorm(1,0.0,stdDev)
    currentPos <- currentPos + exp(angle*1.0i)
    positions <- c(positions,currentPos)
}
plot(Re(positions),Im(positions),type="l")
```

#### break 与 next 语句

break 和 next 语句用于影响当前循环中哪部分代码将被执行。break 语句会跳转到当前代码块的末尾，并停止循环的执行。next 语句的作用则相当于已到达代码块末尾，然后回到代码块开头开始下一次迭代。

作为演示，我们在前面示例的随机游走模拟基础上进行构建。我们通过对位置施加限制来修改模型。如果某一步移动到平面的左侧部分，则该步会被忽略：

```
positions   <- complex(0) # Initialize the history of positions
currentPos  <- 0.0+0.0i   # Start at the origin
NUMBERSTEPS <- 50         # Number of steps to take
angleFacing <- 0.0        # direction it is facing
stdDev      <- 1.0        # std dev. of the change in the angle

step <- as.integer(0)
repeat
  {
    ## Add new people to the line and update the length
    newAngle     <- angle + rnorm(1,0.0,stdDev)
    proposedStep <- currentPos + exp(newAngle*1.0i)
    if(Re(proposedStep) < 0.0)
      next  # Ignore this step. It moves to neg. real parts

    ## update the position
    angle     <- newAngle
    currentPos <- proposedStep
    positions  <- c(positions,currentPos)

    ## Update the current time step
    step <- step + as.integer(1)

    ## Check to see if it is time to stop
    if(step>MAXNUMBER)
      break
  }
plot(Re(positions),Im(positions),type="l")
```

### 函数

另一个常见的编程任务是定义可以通过一次调用执行的函数或子例程。由于在不同上下文中可能存在的同名变量所涉及的技术细节，定义和使用函数可能比较复杂。另一个出现的问题是 R 中一切都是对象。到目前为止，我们一直悄然忽略这个问题，但这是一个我们现在必须考虑的技术问题。在本节中，我们首先演示如何定义函数。然后讨论参数如何传递给函数的细节。最后讨论 R 如何确定变量名含义的技术细节。

在深入这些细节之前，我们先说明一下 R 如何跟踪函数。当我们定义一个变量时，R 会将该变量视为一个对象，可以使用我们分配给该变量的名称来访问它。同样，当你定义一个新函数时，它会被分配一个变量名，并且该变量是一个对象。

#### 定义函数

如前所述，定义函数时，函数会被分配到一个对象上，并像任何其他变量一样被对待。函数定义的格式如下：

```
function (arg1, arg2, ... )
  code block
```

这将会创建一个对象，你必须给该对象分配一个变量名。如果你打印出该变量的值，它会打印出函数的定义。在下面的示例中，假设我们需要一个用于在复平面中模拟随机游走的函数。该函数接受当前位置，并在随机方向上增加一个单位步长：

```
> updatePosition <- function(currentPos)
+ {
+     newDirection <- exp(1i*runif(1,0.0,2.0*pi))
+     currentPos + newDirection
+ }
> 
> updatePosition(0.0)
[1] 0.9919473-0.1266517i
> updatePosition
function(currentPos)
{
    newDirection <- exp(1i*runif(1,0.0,2.0*pi))
    currentPos + newDirection
}
```

与函数相关的一个奇特之处在于，函数返回的值是代码块中最后求值的表达式。

有时你可能希望函数执行影响多个变量的操作。在这种情况下，你可能需要返回结果的组合。对于无法表示为向量的复杂结果，你可以将结果作为列表返回。在


在下面的示例中，我们扩展了前面的示例，希望同时返回新位置以及更新后的移动方向：

```
```r
> updatePosition <- function(currentPos,angle,stdDev)
+ {
+     angle <- angle + rnorm(1,0,stdDev)
+     list(newPos=currentPos + exp(angle*1.0i),
+          newAngle=angle)
+ }
>
> pos <- updatePosition(2.0,0.0,1.0)
> pos
$newPos
[1] 2.986467+0.163962i

$newAngle
[1] 0.164706
> pos$newPos
[1] 2.986467+0.163962i
```
```

可以使用 `return` 命令显式指定函数返回的值。`return` 命令最多接受一个参数。该命令将退出函数，如果参数存在，则返回参数中给定的值。在下面的示例中，我们基于复平面上的随机游走示例进行构建。这里，我们假设平面的左侧不可达，如果某一步的实部为负，则该步向相反方向移动：

```
```r
> updatePosition <- function(currentPos,angle,stdDev)
+ {
+     angle <- angle + rnorm(1,0.0,stdDev)
+     newStep <- exp(angle*1.0i)
+     if(Re(currentPos + newStep)<0.0)
+     {
+             # 这将是向平面左侧的移动。
+             # 向相反方向移动。
+             return(list(newPos=currentPos - newStep,
+                         newAngle=angle+pi))
+     }
+     # 一切正常。接受这次移动。
+     return(list(newPos=currentPos + newStep,
+                 angle=angle))
+ }
>
> pos <- updatePosition(-0.1+2i,0.0,1.0)
> pos$newPos
[1] 0.459425+2.828881i
```
```

#### 函数的参数

我们已经讨论了如何定义一个新函数，并简要讨论了如何向函数传递参数。现在我们重点讨论向函数传递参数的一些细节。首先，我们注意到，传递给函数的参数是按值传递而非按引用传递。你对参数所做的任何更改都不会影响函数外部的变量。在下面的示例中，我们回到用于更新复平面随机游走位置的函数。我们将角度传递给函数，该角度在函数内部被改变，但在函数外部保持不变：

```
```r
> updatePosition <- function(currentPos,angle,stdDev)
+ {
+     angle <- angle + rnorm(1,0.0,stdDev)
+     currentPos + exp(1i*angle)
+ }
>
> angle <- 0.0
> updatePosition(1+2i,angle,1.0)
[1] 0.250178+2.661639i
> angle
[1] 0
```
```

另一个重要的一点是，你可以为某些参数提供默认值。如果为某个变量提供了默认值，那么在调用函数时就不需要再传入该参数：

```
```r
> updatePosition <- function(currentPos,angle=0.0)
+ {
+     print(noquote(paste("Angle is ",angle)))
+     angle <- angle + runif(1,0.0,2.0*pi)
+     currentPos + exp(1i*angle)
+ }
>
> updatePosition(1+2i)
[1] Angle is  0
[1] 0.091507+2.417901i
> updatePosition(1+2i,pi)
[1] Angle is  3.14159265358979
[1] 0.029198+2.398841i
```
```

在某些情况下，你可能希望检查调用函数时是否指定了某个特定参数。这可以通过 `missing` 命令来完成。在下一个示例中，我们测试是否提供了角度：

```
```r
> updatePosition <- function(currentPos,angle=0.0)
+ {
+     if(missing(angle))
+     {
+         warning("Using the default drift: ",angle)
+     }
+     angle <- angle + runif(1,0.0,2.0*pi)
+     currentPos + exp(1i*angle)
+ }
>
> updatePosition(1+2i)
[1] 0.552725+1.105604i
Warning message:
In updatePosition(1 + (0+2i)) : Using the default drift: 0
> updatePosition(1+2i,pi)
[1] 0.054151+1.675394i
```
```

请注意，这里使用了 `warning` 命令来打印一条消息。在执行函数时，打印警告或由于错误条件而停止函数执行可能是有用的。`stop` 和 `warning` 命令可用于这些情况。`warning` 命令打印一条警告并继续正常执行。`stop` 命令将打印一条消息并退出函数：

```
```r
> updatePosition <- function(currentPos,angle=0.0)
+ {
+     if(abs(angle) > 2.0*pi)
+     {
+             stop("I arbitrarily do not like angles that big")
+     }
+     angle <- angle + runif(1,0.0,2.0*pi)
+     currentPos + exp(1i*angle)
+ }
>
> pos1 <- updatePosition(1+2i)
> pos2 <- updatePosition(1+2i,3.0*pi)
Error in updatePosition(1 + (0+2i), 3 * pi) :
    I arbitrarily do not like angles that big
> pos2
Error: object 'pos2' not found
```
```

请注意，在最后一行中，调用了 `stop` 命令，函数没有返回值。结果是变量 `pos2` 不存在。但要注意，如果之前已经定义了变量 `pos2`，那么它将保留其以前的值。

在前面的示例中，对调用函数时参数的顺序作了一个假设。在前面的示例中，参数按照它们在函数调用中出现的顺序进行匹配。你可以在调用函数时指定名称来绕过这个约定。需要注意的是，R 不要求名称完全匹配，它会尝试使用名称中的前几个字符来匹配名称。如果匹配存在歧义，你将收到一条错误消息：

```
```r
> matching <- function(argOne,argTwo)
+ {
+     return(paste("I got this: ",argOne,' ',argTwo))
+ }
> matching(argTwo="second",argOne="First")
[1] "I got this:  First   second"
> matching(argT="2nd",argO="1st")
[1] "I got this:  1st   2nd"
> matching(argT="two",arg="one")
Error in matching(argT = "two", arg = "one") :
    argument 2 matches multiple formal arguments
```
```

最后一个要讨论的问题是如何限制参数可能取值的范围。参数的默认值可以作为一个值向量给出。如果没有提供参数，则默认使用向量中的第一个条目。如果你希望将值限制为向量中的某一个值，可以使用 `match.arg` 函数来测试该值：

```
```r
> updatePosition <- function(currentPos,angle=0.0,
+                             dist=c("uniform","normal"))
+ {
+     dist <- match.arg(dist)
+     print(dist)
+     # 更新位置的代码将在下面
+   }
> updatePosition(0.0,0.0)
[1] "uniform"
> updatePosition(0.0,0.0,"uniform")
[1] "uniform"
> updatePosition(0.0,0.0,"neither")
Error in match.arg(dist) : 'arg' should be one of "uniform", "normal"
```
```

#### 作用域

处理函数时一个重要的问题是如何决定符号的含义。这个概念称为作用域，用于描述作用域相关概念的语言可能令人困惑。不幸的是，这是需要认真考虑的事情，我们尝试在这里讨论一些相关概念。还需要注意，这里讨论的细节代表了 R 与 S_PLUS 语言不一致的一个领域，因此在推广这些概念时要小心。如需更多信息，你可以在 R 环境中输入 `demo(scoping)` 命令，会给出关于作用域概念的简要演示。如果输入 `help(environment)` 命令，你也可以找到更多细节。

基本思想是 R 维护一个环境的层次结构。每个环境都有一个与该环境关联的符号列表。你可以创建一个嵌入到另一个环境中的新环境。`new.env` 命令用于创建环境。该环境拥有自己的变量，可以使用 `assign` 命令创建变量。可以使用 `get` 命令获取值：

```
```r
> envOne <- new.env()
> typeof(envOne)
[1] "environment"
> ls()
[1] "envOne"
> ls(envOne)
character(0)
>
>
> assign("bubba",12,envir=envOne)
> ls()
[1] "envOne"
> ls(envOne)
[1] "bubba"
> envOne$bubba
[1] 12
> get("bubba",envOne)
[1] 12
> bubba
Error: object 'bubba' not found
```
```

请注意，在前面的示例中，我们使用了 `ls` 命令的可选参数 `environment`，该参数指定使用哪个环境。该环境用于引导 R


我得到了数字 -2.3 。

> if(x < 0)
+     {
+             print("Why would you give me a negative number, jerk?")
+             x <- abs(x)
+     }
[1] "Why would you give me a negative number, jerk?"

> y <- sqrt(x)

该命令有许多选项。这里未探讨的一个选项是 verbose 选项。这是一个对调试有帮助的选项，你应该尝试添加 verbose=TRUE 选项。这里省略了一个示例，因为把它称为 verbose 是一种轻描淡写的说法。

### 总结

本章介绍了指定某些命令可选执行的基本思想以及三种基本循环结构。我们不得不顺带讨论作用域的概念，并探索 R 如何查找和解释变量名的含义。你可以结合这些思想来创建和实现算法，并在文件中执行命令。

本章还首次初步接触了面向对象编程意义上的 S3 类。我们将在下一章中基于这一思想，正式定义 S3 类。在此过程中，我们将探索如何扩展现有函数，使其能够处理包含我们所构造类的参数。
