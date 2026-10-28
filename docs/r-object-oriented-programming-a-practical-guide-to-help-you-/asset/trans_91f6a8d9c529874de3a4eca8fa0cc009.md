
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

## 循环结构

另一个重要的编程任务是创建一组可以按结构方式重复执行的指令。R 中有三种循环结构：for 循环、while 循环和 repeat 循环。我们将逐一探讨这些循环结构，并讨论 break 和 next 命令，它们可以控制循环代码块内指令的执行方式。使用 `help(Control)` 命令可以查看更多详细信息。

### for 循环

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

### while 循环

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

### repeat 循环

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

### break 与 next 语句

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

## 函数

另一个常见的编程任务是定义可以通过一次调用执行的函数或子例程。由于在不同上下文中可能存在的同名变量所涉及的技术细节，定义和使用函数可能比较复杂。另一个出现的问题是 R 中一切都是对象。到目前为止，我们一直悄然忽略这个问题，但这是一个我们现在必须考虑的技术问题。在本节中，我们首先演示如何定义函数。然后讨论参数如何传递给函数的细节。最后讨论 R 如何确定变量名含义的技术细节。

在深入这些细节之前，我们先说明一下 R 如何跟踪函数。当我们定义一个变量时，R 会将该变量视为一个对象，可以使用我们分配给该变量的名称来访问它。同样，当你定义一个新函数时，它会被分配一个变量名，并且该变量是一个对象。

### 定义函数

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
