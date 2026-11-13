
逻辑非，如下所示：

```
if (!wavingWhiteFlag){
  // not surrendering so check everything else
  if(isComingOverTheBridge && friendlyTroops >= enemyTroops){
    // shoot them
  }else if(isComingOverTheBridge && friendlyTroops < enemyTroops) {
    // blow the bridge
  }
}else{
  // this is the else for our first if
  // Take prisoners
}
// Holding position
```

这表明我们可以将 if 和 else 语句相互嵌套，以创建相当深入且详细的决策。

我们可以继续使用 if 和 else 做出越来越复杂的决策，但我们已看到的内容作为入门已足够。值得注意的是，解决一个问题通常不止一种方法。*正确*的方式通常是以最清晰、最简单的方式解决问题的方法。

让我们看看在 Java 中做出决策的其他一些方式，然后我们可以将它们全部整合到一个应用中。

## 使用 switch 做决策

我们已看到将 Java 运算符与 if 和 else 语句结合使用的广阔且几乎无限的可能性。但有时，在 Java 中做决策有其他更好的方式。

当我们需要基于一个清晰的可能性列表做决策，且该列表不涉及复杂组合时，switch 通常是首选。

我们这样开始一个 switch 决策：

```
switch(argument){
}
```

在前一个示例中，**参数** 可以是一个表达式或变量。在花括号 {} 内，我们可以基于参数 使用 case 和 break 元素来做决策：

```
case x:
// code to for case x
break;

case y:
// code for case y
break;
```

在前一个示例中，可以看到每个 case 说明一个可能的结果，每个 break 表示该 case 的结束，同时也是不应再评估其他 case 语句的点。

遇到的第一个 break 会跳出 switch 块，继续执行整个 switch 块右花括号 } 之后的下一行代码。

我们还可以使用不带值的 default 来运行一些代码，以防没有任何 case 语句评估为 true。如下所示：

```
default: // Look no value
// Do something here if no other case statements are true
break;
```

让我们编写一个快速使用 switch 的演示应用。

## Switch 演示应用

要开始，创建一个名为 Switch Demo 的新 Android 项目，使用 Blank Activity，并将所有其他设置保留为默认值。通过左键单击编辑器上方的 MainActivity.java 标签切换到 MainActivity.java 文件，然后我们就可以开始编码了。

假设我们正在编写一款老式文本冒险游戏，即玩家输入诸如 "Go East"、"Go West"、"Take Sword" 等命令的那种游戏。在这种情况下，switch 可以使用以下代码处理该情况，我们可以使用 default 来处理玩家输入未明确处理的命令。

在 onCreate 方法中右花括号 } 之前输入以下代码：

```
switch(command){
    case "go east":
        Log.i("Player: ", "Moves to the east" );
        break;

    case "go west":
        Log.i("Player: ", "Moves to the West" );
        break;

    case "go north":
        Log.i("Player: ", "Moves to the North" );
        break;

    case "go south":
        Log.i("Player: ", "Moves to the South" );
        break;

    case "take sword":
        Log.i("Player: ", "Takes the silver sword" );
        break;

    // more possible cases

    default:
        Log.i("Message: ", "Sorry I don't speak Elfish" );
        break;
}
```

运行几次应用。每次，将 command 的初始化更改为新内容。请注意，当你将 command 初始化为由 case 语句明确处理的内容时，我们会得到预期输出。否则，我们会得到默认值 **对不起，我不说精灵语**。

如果针对某个 case 需要执行大量代码，我们可以将其全部包含在一个方法中，例如，就像接下来的这段代码。我已突出显示新的一行：

```
default:
goWest();
break;
```

当然，我们之后需要编写新的 goWest 方法。然后，当 command 初始化为 "go west" 时，goWest 方法将被执行，执行将返回到 break 语句，这将导致代码在 switch 块之后继续。

当然，这段代码严重缺少的一点是与 UI 的交互。我们已看到如何从按钮点击调用方法，但即便如此，也不足以让这段代码在真实应用中发挥作用。我们将在第 10 章 "Everything's a Class" 中看到如何解决这个问题。

我们面临的另一个问题是，代码执行完毕后就结束了。我们需要它不断地向玩家请求指令，而不仅仅是一次，而是一遍又一遍。我们将在下一节探讨这个问题的解决方案。

## 使用循环重复代码

在这里，我们将学习如何通过查看 Java 中不同类型的循环，以受控且精确的方式反复执行代码的一部分。这些包括 while 循环、do while 循环和 for 循环。我们还将学习使用不同类型循环的最合适情况。

询问循环与编程有什么关系是完全合理的，但它们正是名字所暗示的那样。它们是重复同一部分代码多次的一种方式，或者循环遍历同一部分代码，尽管每次可能会产生不同的结果。

这可能只是简单地一直做同一件事，直到被循环（迭代）的代码提示循环结束。它可能是由循环代码本身指定的预定迭代次数。也可能是在满足预定情况或条件之前。或者，可能是上述多种情况的组合。与 if、else 和 switch 一起，循环是 Java 控制流语句的一部分。

我们将查看 Java 提供给我们用于控制代码的所有主要类型的循环，并将使用其中一些来实现一个可工作的小应用，以确保我们完全理解它们。让我们先看看 Java 中称为 while 循环的第一种也是最简单的循环类型。

### While 循环

Java while 循环具有最简单的语法。回想一下 if 语句。我们可以在 if 语句的条件表达式中放入几乎任何运算符和变量的组合。如果表达式评估为 true，则执行 if 块体中的代码。while 循环中我们也会放入一个可以评估为 true 或 false 的表达式。看一下这段代码：

```
int x = 10;
while(x > 0){
x--;
// x decreases by one each pass through the loop
}
```

这里发生的情况是：while 循环之外，声明并初始化了一个名为 x 的 int，值为 10。然后，while 循环开始。其条件是 x > 0。因此，while 循环将继续循环执行其体中的代码，直到条件评估为 false。所以前面的代码将执行 10 次。

在第一次遍历中，x = 10，然后是 9，然后是 8，以此类推。但一旦 x 等于 0，它自然不再大于 0，程序将退出循环并继续执行 while 循环后的第一行代码。

与 if 语句一样，while 循环可能一次都不执行。看一下这个：

```
int x = 10;
while(x > 10){
// more code here.
// but it will never run unless x is greater than 10.
}
```

此外，条件表达式的复杂性或可以放入循环体中的代码量都没有限制：

```
int newMessages = 3;
int unreadMessages = 0;
while(newMessages > 0 || unreadMessages > 0){
// Display next message
// etc.
}
```

```
// continue here when newMessages and unreadMessages equal 0
```

前面的 while 循环将继续执行，直到 newMessages 和 unreadMessages 都等于或小于零。由于条件使用逻辑 OR 运算符 ||，只要其中任何一个条件为 true，就会导致 while 循环继续执行。

值得注意的是，一旦进入循环体，它将始终完成，即使表达式中途评估为 false，因为它不会再次测试，直到代码尝试开始另一次遍。例如：

```
int x = 1;

while(x > 0){
  x--;
  // x is now 0 so the condition is false
  // But this line still runs
  // and this one
  // and me!
}
```

前面的循环体将恰好执行一次。我们还可以设置一个永远运行的 while 循环，不出所料地称为无限循环。如下所示：

```
int x = 0;

while(true){
  x++; // I am going to get mighty big!
```

### 跳出循环

我们可能像这样使用一个无限循环，以便可以从循环体内部决定何时退出循环。我们会在准备离开循环体时使用 break 关键字来实现这一点。如下所示：

```
int x = 0;

while(true){
  x++; // I am going to get mighty big!
```
