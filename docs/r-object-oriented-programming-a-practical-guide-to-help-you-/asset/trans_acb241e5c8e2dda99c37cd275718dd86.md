
你调用 UseMethod 命令时，R 会搜索一个其类与向量中第一个元素匹配的函数。如果找不到该函数，它会继续查找与第二个元素匹配的函数，并依次进行直到达到向量中的最后一个元素。如果仍然找不到任何函数，它就会调用默认函数。基于这一点，我们现在来考察这些类的新定义。我们不再手动创建类，而是定义一些函数：这些函数会创建一个表示该类的列表，向 class 属性追加一个类名，然后返回该列表。这里共有三个类，我们将为每个类定义一个函数。第一个函数用于定义 GeometricTrial 类的对象的构造函数：

```r
GeometricTrial <- function()
{
    # Create the basic data structure - a list that keeps track of
    # a set of trials.

    # Create the basic methods as part of a list to be returned.
    me = list(
        # Define the history to keep track of the trials.
        history = character(0)
    )

    # Define my class identifier and return the list.
    class(me) <- append(class(me),"GeometricTrial")
    return(me)
}
```

在返回列表之前，append 函数用于将新的类名添加到当前 class 属性的末尾。这一思路也用于从 GeometricTrial 类派生的类中。现在可以定义 Die 和 Coin 类的构造函数，这两个构造函数都显式调用父类的构造函数，执行与当前类相关的任何操作，然后将当前类名追加到 class 属性：

```r
Die <- function()
{
    # Define the object by first calling the constructor for the base class
    me <- GeometricTrial()
    # Add the class name to the end of the list of class names
    class(me) <- append(class(me),"Die")
    return(me)
}

Coin <- function()
{
    # Define the object by calling the constructor for the base class
    me <- GeometricTrial()

    # Add the class name to the end of the list of class names
    class(me) <- append(class(me),"Coin")
    return(me)
}
```

GeometricTrial 类包含四个方法。reset 方法与上一节讨论的 reset 方法行为完全一致。`getHistory` 方法是某个数据元素的访问器，将在下一节讨论。我们现在来讨论 `simulation` 方法，随后将讨论 `singleTrial` 方法。

`simulation` 方法用于模拟一次单一实验。首先清空历史记录，然后反复调用 `singleTrial` 方法，直到返回成功结果。我们首先定义基础 simulation 函数、默认 simulation 函数，然后是 `GeometricTrial` 类所使用的 simulation 函数，如下所示：

```r
simulation <- function(theObject)
{
    UseMethod("simulation",theObject)
}

simulation.default <- function(theObject)
{
    warning("Default simulation method called on unrecognized object.")
    return(theObject)
}

## Define a method to run a simulation of a geometric trial.
simulation.GeometricTrial = function(theObject)
{
    theObject <- reset(theObject)   # Reset the history
                                     # before the trial.
    repeat
    {
        ## perform a single trial and add it to the history
        thisTrial <- singleTrial(theObject)
        theObject <- appendEvent(theObject,thisTrial$result)
        if(thisTrial$success)
        {
            break   # The trial resulted in a success. Time
                    # to stop!
        } # The trial was not a success. Keep going.

    return(theObject)
}
```

定义一个默认函数的工作看似并非值得付出的努力。然而，这种做法通常是为了确保当你定义的方法被误调用时，系统能够负责任地做出反应。

最后一步是定义 `singleTrial` 方法。该方法由子类 Die 和 Coin 执行。同样，我们需要创建基础方法和默认方法。不过在这种情况下，三个类各自也有对应的方法。基础函数调用 `UseMethod` 函数，该函数在 `class` 属性中逐个查找，以确定第一个要调用的函数。我们使用 `GeometricTrial` 类的方法来演示调用顺序以及 `NextMethod` 函数。`NextMethod` 函数会继续在 `class` 属性中搜索，并根据当前类之后的类名调用下一个函数：

## 封装

现在定义 `getHistory` 类的最后一个方法。将它单独放在一节中是为了强调一个重要观点。S3 对象通常是一个基本数据结构，例如一个向量或列表，并额外定义了 class 属性。为类定义的函数会以可预期的方式对 `class` 属性作出反应。

一个副作用是，某个给定类的对象中的每个元素都是公有数据。对象中包含的元素始终可以被访问。因此，在 R 中编程时，我们必须采取额外步骤来规范对对象所维护数据元素的访问。直接访问对象内部数据元素的代码在刚编写时可能可以工作，但对类构造函数的任何更改都可能导致该类的其他方法中代码出错。

就我们之前的示例而言，我们有一个访问器 `getHistory` 方法。如果我们有一个来自 Die 类的对象 oneDie，我们可以通过 `oneDie$history` 轻松获取历史记录。如果稍后我们决定更改用于存储历史记录的数据结构，那么任何直接访问该变量的代码都很可能失败。

相反，我们编写一个访问器方法 `getHistory`，它旨在返回一个向量，其中历史记录以因子向量的形式存在。保持纪律并仅使用此方法来获取历史记录的副本非常重要。请看以下代码：

```
getHistory <- function(theObject)
{
    UseMethod("getHistory",theObject)
}

getHistory.default <- function(theObject)
{
    return(factor()) # Just return an empty vector of factors
}

getHistory.GeometricTrial <- function(theObject)
{
    return(as.factor(theObject$history))
}
```

## 最后一点说明

关于 S3 类，还有最后一点需要分享。如果你使用过 R，那么你很可能已经用过它们。许多函数被定义为根据其第一个参数的类名来作出反应。请看下面的示意图：

![](img/18eeabe9ab8c81c5a3f4040db5906c67_178_0.png)

一个常见的例子是 `plot` 命令。如果你不带参数地输入 `plot` 命令，可以看到它的定义，如下所示：

```r
> plot
function (x, y, ...)
UseMethod("plot")
<bytecode: 0x32fdd50>
<environment: namespace:graphics>
>
```

`plot` 命令会根据你传递给它的对象类型而作出不同的反应。如果你想查看 `plot` 命令可以处理哪些类，可以使用 `methods` 命令列出它们：

```r
> methods(plot)
[1] plot.HoltWinters*   plot.TukeyHSD*       plot.acf*
[4] plot.data.frame*    plot.decomposed.ts*  plot.default
[7] plot.dendrogram*    plot.density*         plot.ecdf
[10] plot.factor*        plot.formula*         plot.function
[13] plot.hclust*        plot.histogram*       plot.isoreg*
[16] plot.lm*            plot.medpolish*       plot.mlm*
[19] plot.ppr*           plot.prcomp*          plot.princomp*
```

+   [22] `plot.profile.nls*`  `plot.spec*`  `plot.stepfun`
+   [25] `plot.stl*`    `plot.table*`  `plot.ts`
+   [28] `plot.tskernel*`

**带星号的函数为不可见函数**

> >

S3 类定义的最大优点之一是，在现有基础上进行扩展非常简单。在上一节的示例中，我希望 `plot` 命令能根据我传入的是 `Die` 类还是 `Coin` 类对象而作出相应的反应。假设我已经定义了前面的 `Die` 和 `Coin` 类，那么我只需要定义两个新的 plot 函数，如下所示：

```r
> plot.Die <- function(theDie,theTitle)
+     {
+         plot(theDie$getHistory(),
+              xlab="Value After A Die Roll",ylab="Frequency",
+              main=theTitle)
+     }
>
> plot.Coin <- function(theCoin,theTitle)
+     {
+         plot(theCoin$getHistory(),
+              xlab="Value After Coin Flip",ylab="Frequency",
+              main=theTitle)
+     }
> plot(aCoin,"This Here Trial")
> plot(aDie,"A More Better Trial")
```

利用这一思路来扩展许多命令是很常见的。一些常见的例子包括 `print` 和 `format` 函数。

