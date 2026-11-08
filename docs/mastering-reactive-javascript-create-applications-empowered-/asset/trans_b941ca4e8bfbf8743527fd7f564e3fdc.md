
在前面的示例中，我们首先创建一个 Observable，它将传播三个数字（1、2 和 3）。然后，我们在此 Observable 上应用 `filter()` 操作符；这将创建一个新的 Observable，它只会发出满足 i % 2 === 1 的值 i。因此新的 Observable 只传播奇数。

如果运行前面的代码，你将看到以下输出：

1
3

此操作也可以用下面的图表表示：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_105_0.png)

在此示例中，我们始终返回 true 或 false，但我们也可以返回任何 truthy 或 falsy 值。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_105_1.png)

Truthy 值是指在作为布尔值求值时表现为 true 的任何对象。Falsy 值是指在作为布尔值求值时表现为 false 的值。

Falsy 值如下：

- False
- 0
- ""(empty string)
- Null
- Undefined
- NaN

JavaScript 中所有其他值都是 truthy 值。

下面是一个仅过滤 Observable 中 truthy 值的示例：

```
Rx.Observable
.of(0,1,"hello",null,"")
.filter((i)=> i )
.subscribe((i)=>console.log(i));
```

如果运行前面的代码，你将看到以下输出：

```
1
hello
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_106_0.png)

该操作符有一个别名叫做 where。

### reduce() 操作符

reduce() 操作符允许你运行一个函数，将 Observable 的所有值累积起来，生成一个仅包含一个值（累积值）的新 Observable。

该操作符的签名如下：

```
observable.reduce(accumulatorFunction,[initialValue]);
```

第一个参数是可选的，第二个参数是强制的：

- `accumulatorFunction`: 这是一个用于从 observable 累积值的函数。该函数最多可接收四个参数：
  - `acc`: 这是累积值
  - `currentValue`: 这是本次迭代中使用的值
  - `currentIndex`: 这是本次迭代的基于零的索引
  - `source`: 这是所使用的 observable
- `initialValue`: 这是初始累加器

我们可以使用此函数将包含数字的 Observable 中的所有值相加，如下所示：

```
Rx.Observable
.of(1,2,3)
.reduce((acc,current)=>acc+current)
.subscribe((i)=>console.log(i));
```

在此示例中，我们首先创建一个 Observable，它将传播三个数字（1、2 和 3）。然后，我们在此 Observable 上应用 reduce() 操作符；这将创建一个仅包含一个值的新 Observable，该值是此序列中每个元素的总和。

此方法按以下迭代运行：

1. 如果提供了初始值，我们将其用作累加器，并将序列中的第一个元素用作当前值；如果没有，则使用序列中的第一个元素作为累加器，第二个作为当前值。
2. 对累加器和当前值运行 `accumulatorFunction`。
3. 将当前值设为序列中的下一个元素。
4. 返回步骤 2，直到处理完整个列表。

因此，如果运行前面的代码，你将看到以下输出：

此操作可以用下面的图表表示：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_108_0.png)

我们也可以为 reduce() 操作符提供初始值，如以下示例所示：

```
Rx.Observable
.of("w","o","r","l","d")
.reduce((acc,current)=>acc+current, "Hello ")
.subscribe((i)=>console.log(i));
```

正如你所想象的，运行此代码将连接整个字符串，因此你可以在控制台看到以下消息：

```
Hello world
```

### Summary

在本章中，我们学习了如何将操作符应用于 observable。这有助于解耦我们的代码并提高其可测试性和可读性。这是在 RxJS 中首次使用以下操作符，如 map()、flatMap()、filter() 和 reduce()。

这些是 RxJS 中使用最广泛的操作符，因此也是最重要的操作符。我们在 bacon.js 中使用过其中一些，但在本章中，我们能够更深入地研究每一个；图表和示例只帮助我们更好地理解它们的使用。

在下一章中，我们将讨论 observable 上的背压问题以及我们可以采取哪些措施来缓解此问题。我们还将了解更多关于从 observable 过滤数据的操作符。

## 5

## 世界变化太快 - 处理背压的操作符

在上一章中，我们学习了如何使用操作符转换 observable 发出的数据，以及如何链式使用此操作符，使其可重用，因为每个操作符都会创建一个新的 observable，该 observable 可被多个观察者使用。

我们学习了如何使用以下四个不同的操作符：

- `map()`
- `flatMap()`
- `filter()`
- `reduce()`

这些操作符在函数式响应式编程中最重要且最常用。我们看到了示例和图表，以确保我们理解如何使用这些操作符。

这些操作符的使用超出了上一章。我们将在整本书中继续使用它们，并在最后两章中大量使用它们，因为我们将使用函数式响应式编程（使用 `RxJS` 库）创建一个实时网络聊天的客户端和服务器

在本章中，我们将介绍程序中的一个常见问题以及如何使用函数式响应式编程操作符来缓解它。此问题称为 **背压**。我们还将介绍更多用于从 observable 过滤数据的操作符。

在本章中，我们将介绍以下主题：

- 什么是背压？
- 如何使用 RxJS 缓解背压？
- 处理背压的有损策略
- 处理背压的无损策略
- 缓冲 observable
- 暂停 observable
- 处理背压的操作符
- 过滤数据的操作符

### 什么是背压？

在使用函数式响应式编程时，我们使用数据或事件流（称为 observable）来建模问题，这些流可以被转换（使用操作符）并最终导致某些效果（通过观察者）。现在想象一下，我们有一个 observable，其发出数据的速度比观察者处理的速度快；这将导致一个称为背压的问题。

当我们希望保持 observable 以一定的节奏运行时，也会出现此问题。想象一下，你想将某个 hashtag 的所有推文记录到控制台，但你希望每隔几秒最多记录一条，以确保用户可以阅读推文。如果该 hashtag 每秒的推文数超过人类阅读能力，也会导致背压。

使用 RxJS，我们有两种可能的策略来处理此问题。

我们可以丢弃一些数据。你可能对用户鼠标的所有移动或某个 hashtag 的所有推文都不感兴趣。你只关心大局。

你也可以将数据排队，直到你有能力处理它或批量处理它。

### 处理背压的常见策略

在 RxJS 中，我们有几个操作符可以用来缓解背压问题，它们属于两种不同的策略家族：有损策略和无损策略。我们将在本章中详细了解每一种。

### 处理背压的有损策略

处理背压的有损策略是我们想要丢弃一些数据时必须使用的策略。正如我们所讨论的，你可能对用户鼠标的所有移动都不感兴趣。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_113_0.png)

有损策略允许你使用常量内存来缓解背压问题，因为我们不保留任何缓冲区。

### throttle() 操作符

处理背压的第一个有损技术是 throttle() 操作符。此操作符允许你以一定的间隔传播 observable 发出的元素。

此操作符非常适合实现速率限制，例如以人类可阅读的速度显示推文的问题。

throttle() 操作符的签名如下：

```
observable.throttle(interval, [scheduler]);
```

第一个参数是强制的，第二个是可选的：

- interval: 这是数据发出之间以毫秒为单位的时间间隔
- scheduler: 用于传播数据

